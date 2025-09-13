# Manual Técnico

**Universidad de San Carlos de Guatemala**
**Escuela de Ciencias y Sistemas**
**Redes de Computadoras 1**
**Estiben Yair López Leveron**
**Carné: 202204578**

---

## 1. Topología de Red

La topología diseñada para la empresa **FIUComm Technologies** se implementó en **Cisco Packet Tracer**, respetando el modelo jerárquico de red y aplicando segmentación mediante **VLANs**, administración con **VTP**, redundancia con **STP** y enlaces agregados por **EtherChannel**.

![Topología de Red](image-8.png)

La red se compone de 5 áreas principales:

- **Redacción Digital (VLAN 18)**
- **Análisis de Datos (VLAN 28)**
- **Infraestructura IT (VLAN 38)**
- **Área Local de Infraestructura IT (VLAN 38)**
- **Gerencia (VLAN 58)**

---

## 2. Vlans y Direccionamiento IP

Cada departamento cuenta con una **VLAN dedicada** y un segmento de red enmascarado /24 (255.255.255.0).
Esto garantiza la segmentación lógica, seguridad y facilidad de administración.

---

### 2.1 Redacción Digital – VLAN 18

Encargados de la generación de contenido multimedia y publicaciones.


| Departamento       | VLAN | Dirección IP   | Dispositivo | Nombre del Dispositivo |
| ------------------ | ---- | --------------- | ----------- | ---------------------- |
| Redacción Digital | 18   | 192.168.18.1/24 | PC          | S2                     |
| Redacción Digital | 18   | 192.168.18.2/24 | Laptop      | RD2                    |
| Redacción Digital | 18   | 192.168.18.3/24 | PC          | G2                     |
| Redacción Digital | 18   | 192.168.18.4/24 | Laptop      | S3                     |

---

### 2.2 Análisis de Datos – VLAN 28

Área responsable de la recopilación, tratamiento y análisis de información estratégica.


| Departamento       | VLAN | Dirección IP    | Dispositivo | Nombre del Dispositivo |
| ------------------ | ---- | ---------------- | ----------- | ---------------------- |
| Análisis de Datos | 28   | 192.168.18.5/24  | Laptop      | IT8                    |
| Análisis de Datos | 28   | 192.168.18.6/24  | Laptop      | AD2                    |
| Análisis de Datos | 28   | 192.168.18.7/24  | PC          | RD3                    |
| Análisis de Datos | 28   | 192.168.18.8/24  | Laptop      | AD1                    |
| Análisis de Datos | 28   | 192.168.18.9/24  | PC          | G3                     |
| Análisis de Datos | 28   | 192.168.18.10/24 | Laptop      | AD5                    |
| Análisis de Datos | 28   | 192.168.18.11/24 | PC          | RD5                    |
| Análisis de Datos | 28   | 192.168.18.12/24 | Laptop      | G4                     |
| Análisis de Datos | 28   | 192.168.18.13/24 | Laptop      | S4                     |
| Análisis de Datos | 28   | 192.168.18.14/24 | PC          | AD3                    |
| Análisis de Datos | 28   | 192.168.18.15/24 | Laptop      | RD4                    |
| Análisis de Datos | 28   | 192.168.18.16/24 | PC          | AD4                    |
| Análisis de Datos | 28   | 192.168.18.17/24 | PC          | RD1                    |
| Análisis de Datos | 28   | 192.168.18.18/24 | PC          | IT7                    |

---

### 2.3 Infraestructura IT – VLAN 38

Soporte técnico, servidores y administración de red.


| Departamento       | VLAN | Dirección IP    | Dispositivo | Nombre del Dispositivo |
| ------------------ | ---- | ---------------- | ----------- | ---------------------- |
| Infraestructura IT | 38   | 192.168.18.19/24 | PC          | S5                     |
| Infraestructura IT | 38   | 192.168.18.20/24 | PC          | IT3                    |
| Infraestructura IT | 38   | 192.168.18.21/24 | Laptop      | IT2                    |
| Infraestructura IT | 38   | 192.168.18.22/24 | Laptop      | G2                     |
| Infraestructura IT | 38   | 192.168.18.23/24 | PC          | RD6                    |
| Infraestructura IT | 38   | 192.168.18.24/24 | PC          | AD8                    |
| Infraestructura IT | 38   | 192.168.18.25/24 | Laptop      | IT1                    |
| Infraestructura IT | 38   | 192.168.18.26/24 | Laptop      | RD7                    |

---

### 2.4 Área Local – Infraestructura IT – VLAN 78

Segmento de red interno adicional para pruebas y operación local.


| Departamento | VLAN | Dirección IP    | Dispositivo | Nombre del Dispositivo |
| ------------ | ---- | ---------------- | ----------- | ---------------------- |
| Local        | 78   | 192.168.18.27/24 | PC          | Local1                 |
| Local        | 78   | 192.168.18.28/24 | Laptop      | Local2                 |
| Local        | 78   | 192.168.18.29/24 | PC          | Local3                 |

---

### 2.5 Área de Gerencia – VLAN 58

Encargada de la gestión administrativa y estratégica de la empresa.


| Departamento | VLAN | Dirección IP    | Dispositivo | Nombre del Dispositivo |
| ------------ | ---- | ---------------- | ----------- | ---------------------- |
| Gerencia     | 58   | 192.168.18.30/24 | PC          | AD6                    |
| Gerencia     | 58   | 192.168.18.31/24 | Laptop      | S1                     |
| Gerencia     | 58   | 192.168.18.32/24 | PC          | RD8                    |
| Gerencia     | 58   | 192.168.18.33/24 | Laptop      | G1                     |
| Gerencia     | 58   | 192.168.18.34/24 | Laptop      | AD7                    |
| Gerencia     | 58   | 192.168.18.35/24 | PC          | IT4                    |
| Gerencia     | 58   | 192.168.18.36/24 | PC          | IT5                    |

# 3. Configuración de los Switches

En esta sección se documenta la configuración de los switches en las diferentes áreas de la topología.
Se incluye la configuración de **VTP (VLAN Trunking Protocol)** en modo **servidor** y **cliente**, así como la asignación del dominio correspondiente.

---

## 3.1 Configuración de VTP

### Área Central – Switch Multicapa (Servidor VTP)

```Cisco
enable
configure terminal
hostname SERVIDOR

vtp mode server
vtp version 2
vtp domain C5_FIUComm

exit
write


Switches Clientes (MSW1 – MSW6)

```Cisco
enable
configure terminal
  hostname MSW1
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW2
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW3
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW4
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW5
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW6
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

```

Área: Gerencia
Clientes
Switches: SW13, SW14, SW15, SW16, SW17, MSW10

```Cisco
enable
configure terminal
  hostname SW13
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW14
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW15
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW16
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW17
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW10
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

```

Área: Infraestructura IT
Cliente
Switches: SW8, SW9, SW10, SW11, SW12, MSW8
Switch Transparente: Switch19

```Cisco
enable
configure terminal
  hostname SW8
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW9
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW10
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW11
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW12
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW8
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

! Switch en modo transparente
enable
configure terminal
  hostname Switch19
  vtp mode transparent
  vtp version 2
  vtp domain C5_FIUComm
exit
write
```

Área: Análisis de Datos
cliente
Switches: SW1 – SW7, MSW7

```Cisco
enable
configure terminal
  hostname SW1
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW2
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW3
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW4
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW5
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW6
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW7
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW7
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

```

Área: Redacción digital (RD)
cliente
Switches: SW18, SW19, SW20, MSW9

```Cisco
enable
configure terminal
  hostname SW18
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW19
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname SW20
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

enable
configure terminal
  hostname MSW9
  vtp mode client
  vtp version 2
  vtp domain C5_FIUComm
exit
write

```

## configuracion de STP

switch servidor

```
enable 
configure terminal
 spanning-tree vlan 1,18,28,38,48,58 root primary   
end
write
```

swtich clientes (todos los del area central menos el servidor)

```
enable 
configure terminal
 spanning-tree vlan 1,18,28,38,48,58 root secondary
end
write
```

Para los otros switches

```
enable 
configure terminal
 spanning-tree vlan 1,18,28,38,48,58 priority 32768
end
write
```

## swtich transpartente

crea vlan

```
enable
configure terminal
 vlan 78
  name Recepcion
end
write
```

## configuracion de Etherchannels

switch servidor

```
!configuracion rojo LACP de servidor a MSW6
enable 
configure terminal
! Enlaces físicos
interface range fa0/6 - 8
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion rojo LACP de servidor a MSW5
enable 
configure terminal
! Enlaces físicos
interface range fa0/10 - 12
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion verde LACP de servidor a MSW1
enable 
configure terminal
! Enlaces físicos
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 3 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 3
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

!configuracion verde LACP de servidor a MSW1
enable 
configure terminal
! Enlaces físicos
interface range fa0/16 - 18
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 4 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 4
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit



!configuracion azul PagP de servidor a MSW3
enable
configure terminal
interface range fa0/19 - 21
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 5 mode desirable   
exit

! Port-channel lógico
interface port-channel 5
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit



!configuracion azul PagP de servidor a MSW4
enable
configure terminal
interface range fa0/22 - 24
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 6 mode desirable   
exit

! Port-channel lógico
interface port-channel 6
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


```

switch MSW4

```
enable
configure terminal
interface range fa0/22 - 24
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode desirable   
exit

! Port-channel lógico
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion azul PagP de MSW4 a MSW5
enable
configure terminal
interface range fa0/16 - 18
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode desirable   
exit

! Port-channel lógico
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

```

switch MSW3

```
!configuracion azul PagP de MSW3 a SERVIDOR
enable
configure terminal
interface range fa0/19 - 21
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode desirable   
exit

! Port-channel lógico
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion azul PagP de MSW3 a MSW6
enable
configure terminal
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode desirable   
exit

! Port-channel lógico
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

```

switch MSW1

```
!configuracion verde LACP de  MSW1 a servidor
enable 
configure terminal
! Enlaces físicos
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 3 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 3
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion verde LACP de  MSW1 a MSW6
enable 
configure terminal
! Enlaces físicos
interface range fa0/10 - 12
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

```

switch MSW2

```
!configuracion verde LACP de servidor a MSW1
enable 
configure terminal
! Enlaces físicos
interface range fa0/16 - 18
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion verde LACP de MSW2 A MSW5
enable 
configure terminal
! Enlaces físicos
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit



```

switch MSW6

```
!configuracion rojo LACP de MSW6 a servidor
enable 
configure terminal
! Enlaces físicos
interface range fa0/6 - 8
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion verde LACP de MSW6 a MSW1
enable 
configure terminal
! Enlaces físicos
interface range fa0/10 - 12
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion azul PagP de MSW3 a SERVIDOR
enable
configure terminal
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 3 mode desirable   
exit

! Port-channel lógico
interface port-channel 3
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

```

switch MSW5

```
!configuracion rojo LACP de servidor a MSW6
enable 
configure terminal
! Enlaces físicos
interface range fa0/10 - 12
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 1 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 1
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

!configuracion verde LACP de MS26 a MSW2
enable 
configure terminal
! Enlaces físicos
interface range fa0/13 - 15
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 2 mode active
exit

! Interfaz lógica del EtherChannel
interface port-channel 2
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit


!configuracion azul PagP de MSW5 a MSW4
enable
configure terminal
interface range fa0/16 - 18
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 channel-group 3 mode desirable   
exit

! Port-channel lógico
interface port-channel 3
 switchport
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit

```

* Configuracion de Vlans en el switch Servidor
  creacion de vlans Switch Servidor
  carnet 202204578
  como mi carnet termina en 8 se le puso al final de la vlan

```
enable
config terminal

vlan 18
 name RedaccionDigital
 exit

vlan 28
 name AnalisisDatos
 exit

vlan 38
 name InfraestructuraIT
 exit

vlan 48
 name Seguridad
 exit

vlan 58
 name Gerencia
 exit

exit
write
```

## Banners MOTD (5 switches, uno por área)

switch de bienvenida

switch SW1 (Anilisis de Datos)

```
enable
configure terminal
 hostname SW1
 banner motd #
 Bienvenido a Analisis de Datos - FIUComm_202204578
 #
end
write

```

switch SW18 (Redaccion Digital)

```
enable
configure terminal
 hostname SW18
 banner motd #
 Bienvenido a Redaccion Digital - FIUComm_202204578
 #
end
write

```

switch SW8 (Infraestructura IT)

```
enable
configure terminal
 hostname SW8
 banner motd #
 Bienvenido a Infraestructura IT - FIUComm_202204578
 #
end
write

```

switch SW8 (Area de Gerencia)

```
enable
configure terminal
 hostname SW15
 banner motd #
 Bienvenido a Area de Gerencia - FIUComm_202204578
 #
end
write
```

switch SW8 (Area central Backone)

```
enable
configure terminal
 banner motd #
 Bienvenido a Area Central Backone - FIUComm_202204578
 #
end
write

```

switch SW19 (Area Local)

```
enable
configure terminal
 banner motd #
 Bienvenido a Area Local - FIUComm_202204578
 #
end
write

```

## configuracion para conexiones

switch MSW1

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch MSW2

```
enable
configure terminal
interface range fa0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch MSW3

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
interface range fa0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write

```

switch MSW4

```
enable
configure terminal
interface range fa0/5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write
```

switch MSW5

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch MSW6

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch MSW7

```
enable
configure terminal
interface range fa0/1-3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/4-8
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write



```

switch MSW8

```
enable
configure terminal
interface range fa0/1 - 3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/4 - 6
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write


enable
configure terminal
interface range fa0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch MSW10

```
enable
configure terminal
interface range fa0/1 - 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write

enable
configure terminal
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
end
write
```

switch SW1

```
enable
configure terminal
 interface range fa0/1
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/8
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/3 
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/4
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write



enable
configure terminal
 interface range fa0/6
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/11
  description Latpop IT8 - Infraestructura
  switchport mode access
  switchport access vlan 38
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC AD2 - AnalisisdeDatos
  switchport mode access
  switchport access vlan 28
 exit
end
write

```

switch SW2

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/3 - 5
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write



enable
configure terminal
 interface fa0/10
  description PC RD3 - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write
```

switch SW3

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/5
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC G3 - Gerencia
  switchport mode access
  switchport access vlan 58
 exit
end
write


enable
configure terminal
 interface fa0/11
  description Laptop AD5 - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write


enable
configure terminal
 interface fa0/12
  description Laptop AD1 - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write
```

switch SW4

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/4 - 6
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC RD5 - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write

```

switch SW5

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/4 - 6
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write



enable
configure terminal
 interface fa0/11
  description PC G4 - Gerencia
  switchport mode access
  switchport access vlan 58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description Laptop S4 - Seguridad
  switchport mode access
  switchport access vlan 48
 exit
end
write
```

switch SW6

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/3 , fa0/4 , fa0/6
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write



enable
configure terminal
 interface fa0/12
  description Laptop AD3 - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write


enable
configure terminal
 interface fa0/11
  description PC AD4 - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write

enable
configure terminal
 interface fa0/10
  description LAPTOP RD4 - Redaccion Digital
  switchport mode access
  switchport access vlan 18
 exit
end
write


```

switch SW7

```
enable
configure terminal
 interface range fa0/1 , fa0/7
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/3 - 5
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC RD1 - Redaccion Digital
  switchport mode access
  switchport access vlan 18
 exit
end
write


enable
configure terminal
 interface fa0/11
  description PC IT7 - Infraestructura
  switchport mode access
  switchport access vlan 38
 exit
end
write

```

switch SW8

```
enable
configure terminal
 interface range fa0/1
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/6
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description Laptop IT1 - Infraestructura
  switchport mode access
  switchport access vlan 38
 exit
end
write

enable
configure terminal
 interface fa0/11
  description Laptop RD7 - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write




```

switch SW9

```
enable
configure terminal
 interface range fa0/1
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface range fa0/2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC AD8 - AnalisisdeDatos
  switchport mode access
  switchport access vlan 28
 exit
end
write
```

switch SW10

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write

enable
configure terminal
 interface fa0/10
  description Laptop G5 - Gerencia
  switchport mode access
  switchport access vlan 58
 exit
end
write


enable
configure terminal
 interface fa0/11
  description PC RD - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write

```

switch SW11

```
enable
configure terminal
 interface range fa0/1 - 2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC IT2 - InfraestructuraIT
  switchport mode access
  switchport access vlan 38
 exit
end
write
```

switch SW12

```
enable
configure terminal
 interface range fa0/2 , fa0/5
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write



enable
configure terminal
 interface fa0/11
  description PC S5 - Seguridad
  switchport mode access
  switchport access vlan 48
 exit
end
write

enable
configure terminal
 interface fa0/10
  description PC IT3 - InfraestructuraIT
  switchport mode access
  switchport access vlan 38
 exit
end
write

```

switch S13

```
enable
configure terminal
 interface range fa0/1
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC AD - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write
```

switch S14

```
enable
configure terminal
 interface range fa0/1-2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description LAPTOP S1 - Seguridad
  switchport mode access
  switchport access vlan 48
 exit
end
write


enable
configure terminal
 interface fa0/11
  description LAPTOP IT5 - Infraestructura
  switchport mode access
  switchport access vlan 38
 exit
end
write

```

switch SW15

```
enable
configure terminal
 interface range fa0/1-2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description PC RD8 - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write

```

switch SW16

```
enable
configure terminal
 interface range fa0/1-2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description Laptop G1 - Gerencia
  switchport mode access
  switchport access vlan 58
 exit
end
write
```

switch SW17

```
enable
configure terminal
 interface range fa0/2
  switchport mode trunk
  switchport trunk allowed vlan 18,28,38,48,58
 exit
end
write


enable
configure terminal
 interface fa0/10
  description Laptop AD7 - Analisis de Datos
  switchport mode access
  switchport access vlan 28
 exit
end
write


enable
configure terminal
 interface fa0/11
  description PC IT4 - Infraestructura
  switchport mode access
  switchport access vlan 38
 exit
end
write
```

switch MSW9

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write

enable
configure terminal
interface range fa0/5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
interface range fa0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
 interface fa0/10
  description PC S3 - Seguridad
  switchport mode access
  switchport access vlan 48
 exit
end
write



```

switch SW18

```
enable
configure terminal
interface range fa0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write

```

switch SW20

```
enable
configure terminal
interface range fa0/1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
 interface fa0/11
  description PC S2 - Seguridad
  switchport mode access
  switchport access vlan 48
 exit
end
write

enable
configure terminal
 interface fa0/10
  description PC RD2 - RedaccionDigital
  switchport mode access
  switchport access vlan 18
 exit
end
write

```

switch SW19

```
enable
configure terminal
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk allowed vlan 18,28,38,48,58
exit
exit
write


enable
configure terminal
 interface fa0/10
  description LAPTOP G2 - Gerencia
  switchport mode access
  switchport access vlan 58
 exit
end
write
```

switch SW19

```
enable
configure terminal
 interface fa0/10
  description PC Local3 - Local
  switchport mode access
  switchport access vlan 68
 exit
end
write

enable
configure terminal
 interface fa0/11
  description Laptop Local2 - Local
  switchport mode access
  switchport access vlan 68
 exit
end
write

enable
configure terminal
 interface fa0/2
  switchport mode access
  switchport access vlan 45
 exit
end
write
```

configuracion de contraseña en cada switch

```
enable
configure terminal
vtp password proyecto12025
end
write
```

# Pruebas de Conectividad

ping de prueba desde la PC AD6 del area de gerencia a PC AD8 desde area de infraestructura
![alt text](image-1.png)
ping de prueba desde la PC RD1 del area de Analisis de Datos a Laptop RD2 desde area de Redaccion Digital
![alt text](image-2.png)
ping de prueba desde la PC S5 del area de Infraestructura a PC S3 desde area de Redaccion Digital
![alt text](image-3.png)
ping de prueba desde la laptop IT8 del area de Analisis de datos a PC IT5 desde area de Gerencia
![alt text](image-4.png)
ping de prueba desde la laptop G4 del area de Analisis de datos a Laptop G1 desde area de Gerencia
![alt text](image-5.png)
ping de prueba area local de local 2 a local 2
![alt text](image-6.png)

# Presupuesto

# 📑 Presupuesto del Proyecto FIUComm

Este documento presenta el presupuesto estimado para la implementación de la red empresarial **FIUComm**, considerando los equipos de red necesarios, cableado y respaldo eléctrico.
Los costos son aproximados y pueden variar dependiendo del proveedor y del estado del equipo (nuevo/reacondicionado).

## Presupuesto estimado (referencial)


## Equipamiento utilizado en la simulación

| Ítem                        | Cant. | Precio unitario (USD) | Precio unitario (Q) | Subtotal (Q) |
| --------------------------- | :---: | --------------------: | ------------------: | -----------: |
| Switch 2960-24TT            |   18  |              1,050.00 |            8,190.00 |   147,420.00 |
| Switch 3560-24PS            |   10  |              2,780.00 |           21,684.00 |   216,840.00 |
| Laptops                     |   17  |                750.00 |            5,850.00 |    99,450.00 |
| PCs de escritorio           |   18  |                650.00 |            5,070.00 |    91,260.00 |
| Patch Panel (24 puertos)    |   4   |                120.00 |              936.00 |     3,744.00 |
| Rack 12U c/ ventilación     |   2   |                420.00 |            3,276.00 |     6,552.00 |
| UPS 1500VA                  |   2   |                650.00 |            5,070.00 |    10,140.00 |
| Cable UTP Cat6 (305 m)      |   3   |                210.00 |            1,638.00 |     4,914.00 |
| Conectores RJ45 Cat6 (x100) |   2   |                 35.00 |              273.00 |       546.00 |
| Patch cords Cat6 (x20)      |   3   |                 60.00 |              468.00 |     1,404.00 |
| Organizadores cableado      |   6   |                 30.00 |              234.00 |     1,404.00 |
| Licencias SmartNet (3 años) |   10  |                950.00 |            7,410.00 |    74,100.00 |

Subtotal equipos: Q 657,770.00
Mano de obra e instalación (10%): Q 65,777.00
Contingencia (12%): Q 78,932.00
TOTAL ESTIMADO: ~ Q 802,479.00


### Justificación de los precios

Los precios presentados en el presupuesto se basan en referencias de mercado internacional de proveedores de equipos de redes como Cisco, Panduit, APC, Dell y HP. Se tomó como referencia el costo de equipos nuevos con garantía, considerando que un switch Cisco Catalyst 2960-24TT ronda los USD 1,000–1,200, mientras que un Cisco Catalyst 3560-24PS se encuentra entre USD 2,500–3,000 según el distribuidor. Los precios de cableado estructurado (cajas de cable Cat6, patch panels, conectores y patch cords) corresponden a valores promedio de marcas reconocidas como Belden, Panduit y AMP. Para equipos de usuario final, se consideraron laptops y PCs de escritorio empresariales de gama media, con precios aproximados de USD 700–800 y USD 600–700 respectivamente, basados en cotizaciones de fabricantes como Dell, HP y Lenovo. Los UPS y racks corresponden a marcas como APC y TrippLite, con valores de mercado entre USD 400–700.  

Adicionalmente, se incluyó el costo de licencias y soporte Cisco SmartNet por tres años, estimado en USD 900–1,000 por switch, lo cual garantiza actualizaciones y soporte especializado. Todos los precios fueron convertidos a quetzales utilizando un tipo de cambio de Q7.80 por cada USD, valor vigente en el mercado guatemalteco. Finalmente, se agregó un 10% por mano de obra (instalación, pruebas y certificación de red) y un 12% de contingencia para cubrir variaciones de precio o imprevistos, tal como se recomienda en la planificación de proyectos de redes empresariales.
