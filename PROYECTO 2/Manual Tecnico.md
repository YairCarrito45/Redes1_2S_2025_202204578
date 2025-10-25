# Manual Técnico — Proyecto 2: NetUSAC

## 1. Información General

**Curso:** Redes de Computadoras 1
**Proyecto:** NetUSAC — Proyecto de Interconexión
**Universidad:** Universidad de San Carlos de Guatemala
**Estudiante:** Estiben Yair Lopez Leveron
**Carnet:** 202204578

---

## 2. Resumen del Proyecto

Este proyecto consiste en el diseño, configuración y simulación de una red interedificios para el campus central de la USAC, implementando segmentación mediante VLANs, protocolos de enrutamiento (RIP, OSPF, EIGRP), redundancia (HSRP/VRRP), ACLs y redistribución de rutas.
El objetivo principal es lograr una infraestructura segura, escalable y eficiente, tanto a nivel lógico (simulación) como físico.

---

## 3. Topología General

### 3.1. Diagrama General

![Topología NetUSAC](imagenes/topologia_general.png)

### 3.2. Descripción de la topologia

- Conecta los cinco edificios principales: DIGA, Biblioteca Central, T4, S11, S12.
- Protocolos implementados: RIP, OSPF, EIGRP y rutas estáticas.
- Redistribución en R4, R6 y R7.
- EtherChannel entre MS3, MS4 y MS6.

---

## 3.3. Conceptos para entender el proyecto

### VLSM (Variable Length Subnet Masking)

Técnica de subneteo que usa máscaras de red de longitud variable.
Permite asignar subredes de distintos tamaños según la cantidad de hosts requeridos, aprovechando mejor las direcciones IP.
Ejemplo: una subred /26 para 50 equipos y una /30 para un enlace punto a punto.

### FLSM (Fixed Length Subnet Masking)

Todas las subredes usan la misma máscara.
Es más simple, pero desperdicia IPs si las redes no tienen el mismo tamaño.

### Subneteo eficiente

Proceso de dividir una red en subredes del tamaño justo usando VLSM para minimizar desperdicio de IPs y mantener orden lógico entre segmentos.

### Router-on-a-Stick

Método para que un solo router maneje múltiples VLANs usando una interfaz física y varias subinterfaces virtuales.
Cada subinterfaz tiene una IP de puerta de enlace para una VLAN.
Permite el enrutamiento inter-VLAN.

### ACLs (Access Control Lists)

Conjunto de reglas que controla qué tráfico puede pasar por un dispositivo.
Se usa en routers o firewalls para permitir o denegar tráfico según IP, protocolo o puerto.
En este proyecto, solo se permite comunicación entre dispositivos de la misma VLAN.

### HSRP / VRRP

Protocolos de redundancia que permiten tener dos routers o switches activos/pasivos con una IP virtual compartida.
Si el principal falla, el secundario asume automáticamente la puerta de enlace.
HSRP es propietario de Cisco; VRRP es estándar abierto.

### RIP (Routing Information Protocol)

Protocolo dinámico basado en distancia (saltos).
Máximo 15 saltos.
Fácil de configurar, pero limitado y lento.

### OSPF (Open Shortest Path First)

Protocolo dinámico basado en estado de enlace.
Calcula la ruta más corta con el algoritmo Dijkstra.
Escalable, rápido y soporta jerarquías mediante áreas.

### EIGRP (Enhanced Interior Gateway Routing Protocol)

Protocolo propietario de Cisco.
Combina lo mejor de RIP y OSPF.
Usa métricas compuestas (ancho de banda, retardo, confiabilidad, carga) para calcular rutas rápidas y estables.

### Rutas estáticas

Rutas configuradas manualmente en el router.
No cambian automáticamente si una ruta cae.
Se usan para enlaces simples o controlados.

---

## 4. Detalles de configuracion por Edificio




### DIGA

### Direccionamiento IP — DIGA (Carnet 202204578)
**Bases y VIPs**

| VLAN | Nombre        | Subred              | Máscara            | VIP (Gateway)   | MS1 SVI          | MS2 SVI          |
|------|----------------|---------------------|--------------------|-----------------|-----------------|-----------------|
| 45   | Administración | 192.168.78.0/25     | 255.255.255.128    | 192.168.78.1    | 192.168.78.2    | 192.168.78.3    |
| 35   | Vigilancia     | 192.168.78.128/27   | 255.255.255.224    | 192.168.78.129  | 192.168.78.130  | 192.168.78.131  |
| 15   | Estudiantes    | 192.168.78.160/29   | 255.255.255.248    | 192.168.78.161  | 192.168.78.162  | 192.168.78.163  |
| 25   | Docentes       | 192.168.78.168/29   | 255.255.255.248    | 192.168.78.169  | 192.168.78.170  | 192.168.78.171  |


**SVIs y HSRP**

- MS1: V45 **192.168.78.2**, V35 **192.168.78.130**, V15 **192.168.78.162**, V25 **192.168.78.170**
- MS2: V45 **192.168.78.3**, V35 **192.168.78.131**, V15 **192.168.78.163**, V25 **192.168.78.171**


### Ips y servidores


| Dispositivo            | VLAN | IP             | Máscara        | Gateway (VIP)  |
| ---------------------- | ---: | -------------- | --------------- | -------------- |
| Admin Server           |   45 | 192.168.78.10  | 255.255.255.128 | 192.168.78.1   |
| PC admin1              |   45 | 192.168.78.11  | 255.255.255.128 | 192.168.78.1   |
| PC admin2              |   45 | 192.168.78.12  | 255.255.255.128 | 192.168.78.1   |
| Laptop admin4          |   45 | 192.168.78.13  | 255.255.255.128 | 192.168.78.1   |
| Laptop admin3          |   45 | 192.168.78.14  | 255.255.255.128 | 192.168.78.1   |
| PC admin5              |   45 | 192.168.78.15  | 255.255.255.128 | 192.168.78.1   |
| Cámara/PC vigilancia2 |   35 | 192.168.78.132 | 255.255.255.224 | 192.168.78.129 |
| Cámara/PC vigilancia3 |   35 | 192.168.78.133 | 255.255.255.224 | 192.168.78.129 |
| Estudiantes Server     |   15 | 192.168.78.164 | 255.255.255.248 | 192.168.78.161 |
| Docentes Server        |   25 | 192.168.78.172 | 255.255.255.248 | 192.168.78.169 |



**Red base:** 192.168.78.0/24

#### VLANs


| VLAN | Nombre          | ID | Equipos | Gateway        |
| ---- | --------------- | -- | ------- | -------------- |
| 15   | Estudiantes     | 15 | 5       | 192.168.78.1   |
| 25   | Docentes        | 25 | 5       | 192.168.78.33  |
| 35   | Vigilancia      | 35 | 20      | 192.168.78.65  |
| 45   | Administración | 45 | 120     | 192.168.78.129 |

#### Configuraciones

- **VTP**
  - Dominio: `202204578`
  - Password: `usac2025`
  - Modo: Server (SW5), Cliente (resto)
- **HSRP:** Entre MS1 y MS2
- **RIP:** Comunicación con Biblioteca
- **ACLs:** Permiten solo tráfico entre VLANs iguales



## Comandos





---


### BIBLIOTECA
### Direccionamiento IP — Biblioteca Central (Carnet 202204578)

**Base:** 192.158.78.0/24  
**Y=5 ⇒ VLANs:** 15 Estudiantes, 35 Vigilancia, 55 Biblioteca.  
**VRRP/HSRP:** VIP = primera IP usable. R1 usa la segunda. R0 usa la tercera.

#### Subredes por VLAN (VLSM)
| VLAN | Nombre      | Subred              | Máscara            | VIP (Gateway)   | R1 subif        | R0 subif        |
|-----:|-------------|---------------------|--------------------|-----------------|-----------------|-----------------|
| 15   | Estudiantes | 192.158.78.0/25     | 255.255.255.128    | 192.158.78.1    | 192.158.78.2    | 192.158.78.3    |
| 35   | Vigilancia  | 192.158.78.128/27   | 255.255.255.224    | 192.158.78.129  | 192.158.78.130  | 192.158.78.131  |
| 55   | Biblioteca  | 192.158.78.160/27   | 255.255.255.224    | 192.158.78.161  | 192.158.78.162  | 192.158.78.163  |



#### IP por dispositivo (según tu diagrama)
| Dispositivo                    | VLAN | IP              | Máscara            | Gateway (VIP)   |
|--------------------------------|-----:|-----------------|--------------------|-----------------|
| PC-PT estudiante1              | 15   | 192.158.78.10   | 255.255.255.128    | 192.158.78.1    |
| Laptop-PT estudiante2          | 15   | 192.158.78.11   | 255.255.255.128    | 192.158.78.1    |
| PC-PT estudiante3              | 15   | 192.158.78.12   | 255.255.255.128    | 192.158.78.1    |
| PC-PT vigilancia1              | 35   | 192.158.78.132  | 255.255.255.224    | 192.158.78.129  |
| PC-PT biblioteca1              | 55   | 192.158.78.170  | 255.255.255.224    | 192.158.78.161  |
| PC-PT biblioteca3              | 55   | 192.158.78.171  | 255.255.255.224    | 192.158.78.161  |
| Router **R1** G0/0.15          | 15   | 192.158.78.2    | 255.255.255.128    | —               |
| Router **R1** G0/0.35          | 35   | 192.158.78.130  | 255.255.255.224    | —               |
| Router **R1** G0/0.55          | 55   | 192.158.78.162  | 255.255.255.224    | —               |
| Router **R0** G0/0.15          | 15   | 192.158.78.3    | 255.255.255.128    | —               |
| Router **R0** G0/0.35          | 35   | 192.158.78.131  | 255.255.255.224    | —               |
| Router **R0** G0/0.55          | 55   | 192.158.78.163  | 255.255.255.224    | —               |




## EDIFICIO T4
### Direccionamiento IP — Edificio T4 (Carnet 202204578)

**Base:** 172.16.78.0/24  
**Y=5 ⇒ VLANs:** 15 Estudiantes, 25 Docentes, 35 Vigilancia, 45 Admin, 55 Biblioteca.  
**Gateway:** SVI en **MS5** = primera IP usable de cada subred.

#### Subredes por VLAN (VLSM)
| VLAN | Nombre       | Req. hosts | Subred             | Máscara          | Gateway (MS5 SVI) |
|----:|---------------|-----------:|--------------------|------------------|-------------------|
| 45  | Administración| 75         | 172.16.78.0/25     | 255.255.255.128  | 172.16.78.1       |
| 15  | Estudiantes   | 60         | 172.16.78.128/26   | 255.255.255.192  | 172.16.78.129     |
| 35  | Vigilancia    | 15         | 172.16.78.192/27   | 255.255.255.224  | 172.16.78.193     |
| 55  | Biblioteca    | 12         | 172.16.78.224/28   | 255.255.255.240  | 172.16.78.225     |
| 25  | Docentes      | 10         | 172.16.78.240/28   | 255.255.255.240  | 172.16.78.241     |

#### IP por dispositivo (según tu diagrama)
| Dispositivo          | VLAN | IP             | Máscara            | Gateway         |
|----------------------|-----:|----------------|--------------------|-----------------|
| PC-PT Admin6         | 45   | 172.16.78.10   | 255.255.255.128    | 172.16.78.1     |
| PC-PT Estudiante10   | 15   | 172.16.78.130  | 255.255.255.192    | 172.16.78.129   |
| PC-PT Docente2       | 25   | 172.16.78.242  | 255.255.255.240    | 172.16.78.241   |
| PC-PT vigilancia4    | 35   | 172.16.78.194  | 255.255.255.224    | 172.16.78.193   |
| PC-PT Biblioteca5    | 55   | 172.16.78.226  | 255.255.255.240    | 172.16.78.225   |
| PC-PT Biblioteca4    | 55   | 172.16.78.227  | 255.255.255.240    | 172.16.78.225   |


## EDIFICIO S11

### Direccionamiento IP — Edificio S11 (Carnet 202204578)

**Base:** 172.148.78.0/24  
**Y=5 ⇒ VLANs:** 15 Estudiantes, 25 Docentes, 35 Vigilancia, 45 Admin.  
**VRRP/HSRP:** VIP = primera IP usable. R10 segunda. R11 tercera.

#### Subredes por VLAN (VLSM)
| VLAN | Nombre       | Req. | Subred              | Máscara            | VIP (GW)       | R10 subif       | R11 subif       |
|----:|---------------|----:|---------------------|--------------------|----------------|-----------------|-----------------|
| 15  | Estudiantes   | 100 | 172.148.78.0/25     | 255.255.255.128    | 172.148.78.1   | 172.148.78.2    | 172.148.78.3    |
| 45  | Administración| 55  | 172.148.78.128/26   | 255.255.255.192    | 172.148.78.129 | 172.148.78.130  | 172.148.78.131  |
| 25  | Docentes      | 15  | 172.148.78.192/27   | 255.255.255.224    | 172.148.78.193 | 172.148.78.194  | 172.148.78.195  |
| 35  | Vigilancia    | 10  | 172.148.78.224/28   | 255.255.255.240    | 172.148.78.225 | 172.148.78.226  | 172.148.78.227  |



#### IP por dispositivo (según tu diagrama)
| Dispositivo            | VLAN | IP               | Máscara              | Gateway (VIP)    |
|------------------------|-----:|------------------|----------------------|------------------|
| PC-PT Admin7          | 45   | 172.148.78.132   | 255.255.255.192      | 172.148.78.129   |
| PC-PT Docentes3       | 25   | 172.148.78.196   | 255.255.255.224      | 172.148.78.193   |
| Laptop-PT Docentes4   | 25   | 172.148.78.197   | 255.255.255.224      | 172.148.78.193   |
| PC-PT Estudiantes4    | 15   | 172.148.78.10    | 255.255.255.128      | 172.148.78.1     |
| PC-PT Estudiantes5    | 15   | 172.148.78.11    | 255.255.255.128      | 172.148.78.1     |
| Laptop-PT Vigilancia5 | 35   | 172.148.78.228   | 255.255.255.240      | 172.148.78.225   |



# S12
### Direccionamiento IP — Edificio S12 (Carnet 202204578)

**Base:** 192.128.78.0/24  
**Y=5 ⇒ VLANs:** 15 Estudiantes, 25 Docentes, 35 Vigilancia, 45 Admin.  
**HSRP:** VIP = primera IP usable. **MS7** segunda. **MS8** tercera.

#### Subredes por VLAN (VLSM)
| VLAN | Nombre       | Req. | Subred              | Máscara            | VIP (GW)       | MS7 SVI         | MS8 SVI         |
|----:|---------------|----:|---------------------|--------------------|----------------|-----------------|-----------------|
| 15  | Estudiantes   | 125 | 192.128.78.0/25     | 255.255.255.128    | 192.128.78.1   | 192.128.78.2    | 192.128.78.3    |
| 25  | Docentes      | 35  | 192.128.78.128/26   | 255.255.255.192    | 192.128.78.129 | 192.128.78.130  | 192.128.78.131  |
| 45  | Administración| 25  | 192.128.78.192/27   | 255.255.255.224    | 192.128.78.193 | 192.128.78.194  | 192.128.78.195  |
| 35  | Vigilancia    | 20  | 192.128.78.224/27   | 255.255.255.224    | 192.128.78.225 | 192.128.78.226  | 192.128.78.227  |

#### IP por dispositivo (según tu diagrama)
| Dispositivo            | VLAN | IP               | Máscara              | Gateway (VIP)    |
|------------------------|-----:|------------------|----------------------|------------------|
| PC-PT Estudiante6      | 15   | 192.128.78.10    | 255.255.255.128      | 192.128.78.1     |
| PC-PT Estudiante7      | 15   | 192.128.78.11    | 255.255.255.128      | 192.128.78.1     |
| PC-PT Docentes5        | 25   | 192.128.78.132   | 255.255.255.192      | 192.128.78.129   |
| PC-PT Docentes6        | 25   | 192.128.78.133   | 255.255.255.192      | 192.128.78.129   |
| PC-PT Admin8           | 45   | 192.128.78.196   | 255.255.255.224      | 192.128.78.193   |
| PC-PT Vigilancia6      | 35   | 192.128.78.228   | 255.255.255.224      | 192.128.78.225   |

