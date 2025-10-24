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

## 4. Detalle por Edificio


## DIGA — Direccionamiento por dispositivo

**Bases y VIPs**
- VLAN 45 Admin: 192.168.78.0/25  | VIP **192.168.78.1**  | Máscara 255.255.255.128
- VLAN 35 Vigilancia: 192.168.78.128/27 | VIP **192.168.78.129** | Máscara 255.255.255.224
- VLAN 15 Estudiantes: 192.168.78.160/29 | VIP **192.168.78.161** | Máscara 255.255.255.248
- VLAN 25 Docentes: 192.168.78.168/29 | VIP **192.168.78.169** | Máscara 255.255.255.248

**SVIs y HSRP**
- MS1: V45 **192.168.78.2**, V35 **192.168.78.130**, V15 **192.168.78.162**, V25 **192.168.78.170**
- MS2: V45 **192.168.78.3**, V35 **192.168.78.131**, V15 **192.168.78.163**, V25 **192.168.78.171**

### Hosts y servidores
| Dispositivo                  | VLAN | IP              | Máscara              | Gateway (VIP)     |
|-----------------------------|-----:|-----------------|----------------------|-------------------|
| Admin Server                | 45   | 192.168.78.10   | 255.255.255.128      | 192.168.78.1      |
| PC admin1                   | 45   | 192.168.78.11   | 255.255.255.128      | 192.168.78.1      |
| PC admin2                   | 45   | 192.168.78.12   | 255.255.255.128      | 192.168.78.1      |
| Laptop admin1               | 45   | 192.168.78.13   | 255.255.255.128      | 192.168.78.1      |
| Laptop admin3               | 45   | 192.168.78.14   | 255.255.255.128      | 192.168.78.1      |
| PC admin5                   | 45   | 192.168.78.15   | 255.255.255.128      | 192.168.78.1      |
| Cámara/PC vigilancia2       | 35   | 192.168.78.132  | 255.255.255.224      | 192.168.78.129    |
| Cámara/PC vigilancia3       | 35   | 192.168.78.133  | 255.255.255.224      | 192.168.78.129    |
| Estudiantes Server          | 15   | 192.168.78.164  | 255.255.255.248      | 192.168.78.161    |
| Docentes Server             | 25   | 192.168.78.172  | 255.255.255.248      | 192.168.78.169    |

> Los **switches de acceso (SW3, SW4, SW5, SW6)** no requieren IP salvo **gestión**. Si usas gestión, colócalos en VLAN 45 y toma direcciones libres del rango 192.168.78.16–126 con gateway 192.168.78.1.  
> Los **enlaces troncales** no llevan IP.  
> Los **/30 hacia backbone** se asignan al definir la salida a R4/firewall; quedarán fuera de este bloque.



### 4.1. DIGA
**Red base:** 192.168.78.0/24  

#### VLANs
| VLAN | Nombre | ID | Equipos | Gateway |
|------|---------|----|----------|----------|
| 15 | Estudiantes | 15 | 5 | 192.168.78.1 |
| 25 | Docentes | 25 | 5 | 192.168.78.33 |
| 35 | Vigilancia | 35 | 20 | 192.168.78.65 |
| 45 | Administración | 45 | 120 | 192.168.78.129 |

#### Configuraciones
- **VTP**
  - Dominio: `202204578`
  - Password: `usac2025`
  - Modo: Server (SW5), Cliente (resto)
- **HSRP:** Entre MS1 y MS2  
- **RIP:** Comunicación con Biblioteca  
- **ACLs:** Permiten solo tráfico entre VLANs iguales  
- **Comandos:** *(Agregar configuración real aquí)*

---
