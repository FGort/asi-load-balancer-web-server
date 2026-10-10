# Práctica: Servidor Web con Balanceo de Carga y Alta Disponibilidad

**Asignatura:** Administración de Servicios de Internet  
**Equipo 1:**
- Fernanda García Ortega (Product Owner)
- Fernando De la Rosa Flores (Scrum Master)
- Paola Aguilar García (Developer)
- Luisa Segura Cedeño (Developer)

---

## Descripción del proyecto
Implementación de una infraestructura virtualizada basada en 3 máquinas virtuales para garantizar alta disponibilidad y tolerancia a fallos en un sitio web. El sistema distribuye las peticiones de red de forma equitativa mediante un balanceador de carga con Nginx y resuelve el acceso por dominio local utilizando Dnsmasq.

---

## Arquitectura de Red e Infraestructura

| VM | Nombre de Host | Rol en la Red | Dirección IP | Servicios / Software |
| :--- | :--- | :--- | :--- | :--- |
| **VM1** | `balanceador` / `lb` | Balanceador / DNS | `192.168.1.10` | Nginx, Dnsmasq |
| **VM2** | `web1` | Servidor Web Primario | `192.168.1.11` | Apache2, MariaDB |
| **VM3** | `web2` | Servidor Web Secundario | `192.168.1.12` | Apache2, MariaDB |

* **Dominio local configurado:** `misitio.local` apuntando a `192.168.1.10`.

---

## Historias de Usuario (Sprint Backlog)
- [x] **HU01:** Configuración de 3 VMs e infraestructura de red con IPs estáticas (`nmcli`).
- [x] **HU02:** Instalación de Apache en WEB1 y WEB2 con páginas web de prueba identificables.
- [x] **HU03:** Configuración del balanceador de carga Nginx con algoritmo *Round-Robin*.
- [x] **HU04:** Configuración de resolución DNS local (`misitio.local`) con Dnsmasq.
- [ ] **HU06 / HU07:** Instalación y configuración de la replicación espejo (maestro-esclavo) en MariaDB.
- [ ] **HU05 / HU08:** Pruebas de disponibilidad y *failover* ante la caída de nodos.

---

## Pasos Principales de Configuración

### 1. Configuración de Red en las VMs
Se configuraron las interfaces de red internas utilizando NetworkManager (`nmcli`) para establecer direcciones IP estáticas en el segmento `192.168.1.0/24`.

### 2. Balanceo de Carga con Nginx (VM1)
En el servidor balanceador se configuró el bloque `upstream` para repartir el tráfico entre los dos servidores web:
```nginx
upstream backend_web {
    server 192.168.1.11:80 max_fails=3 fail_timeout=10s;
    server 192.168.1.12:80 max_fails=3 fail_timeout=10s;
}# asi-load-balancer-web-server
 Servidor web con balanceo de carga para Administración de Servicios de Internet.
