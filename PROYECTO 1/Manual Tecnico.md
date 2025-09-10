Manual Tecnico

Universidad de San Carlos de Guatemala
Escuela de Ciencias y Sistemas
Redes de Computadores 1
Estiben Yair Lopez Leveron
202204578

###### Topologia de Red

![alt text](image.png)

# Vlans y Direccionamiento Ip

creacion de vlans Switch Servidor
carnet 202204578
como mi carnet termina en 8 se le puso al final de la vlan
enable 
```Cisco
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

# Configuracion de los Switches

**configuracion de vtp**

Área central:

Switch Multicapa (Servidor VTP)

```Cisco
enable
configure terminal
  hostname SERVIDOR

  vtp mode server
  vtp version 2
  vtp domain C5_FIUComm
exit

write

```

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


**configuracion de vtp**
---
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

# Pruebas de Conectividad

# Presupuesto
