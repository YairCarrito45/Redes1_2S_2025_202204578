

# Manual Tecnico

Universidad de San Carlos de Guatemala
Escuela de Ciencias y Sistemas
Redes de Computadores 1
Estiben Yair Lopez Leveron
202204578

# Topologia de Red

![alt text](image.png)

# Vlans y Direccionamiento Ip

# Configuracion de los Switches

* configuracion de Switch Servidor
```
enable 
configure terminal 
    vtp domain C5_FIUCom
    vtp mode server
    vtp password proyecto12025    (lo agrego al final)

    ! configuracion de vlans 
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
    

    !configuracion stp
    spanning-tree mode pvst                            ! activa el protocolo PVST un arbol stp por cada vlan
    spanning-tree valn 18,28,38,48,58 priority 4095    ! Fuerza a este switch como root bridge bajando priorida a 4096


    !configuracion de EtherChannels

    interface port-channel 1               ! Primero se configura el port-channel
    switchport trunk encapsulation dot1q   ! Requiere establecer encapsulación antes de modo trunk
    switchport mode trunk
    switchport trunk allowed vlan 18,28,38,48,58
    exit

    interface range fa0/6 - 8              ! Luego se agrego las interfaces físicas
    switchport trunk encapsulation dot1q   ! ← Define el protocolo de etiquetado para tramas VLAN (IEEE 802.1Q)
    channel-group 1 mode active            ! LACP (modo activo)
    exit


    ! ETHERCHANNEL ROJO hacia MSW5 (LACP)

    interface port-channel 2
    switchport trunk encapsulation dot1q            ! ← Define el protocolo de etiquetado para tramas VLAN (IEEE 802.1Q)
    switchport mode trunk                           ! ← Establece que este puerto será un enlace troncal (pasa múltiples VLANs)
    switchport trunk allowed vlan 18,28,38,48,58
    exit

    interface range fa0/10 - 12
    switchport trunk encapsulation dot1q
    channel-group 2 mode active
    exit


```

* configuracion de Switch MSW6 (cliente) 
```
    enable 
    configure terminal 

    !configuracion VTP

    vtp domain C5_FIUComm
    vtp mode client


    !stp 
    spanning-tree mode pvst


    !configuracion Etherchanel rojo hacia Servidor
    interface port-channel 1
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk allowed vlan 18,28,38,48,58 
    exit

    interface range fa0/6 - 8
    switchport trunk encapsulation dot1q
    channel-group 1 mode active         ! ← LACP = active
    exit    

    ! fin 


    ! configuracion Etherchanel verde hacia msw1 LACP
    interface port-channel 2
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk allowed vlan 18,28,38,48,58

    exit

    interface range fa0/10 - 12
    switchport trunk encapsulation dot1q
    channel-group 2 mode active         ! ← LACP = active
    exit

    ! fin


    !configuracion Etherchannel azul hacia msw3 PAgp
    interface port-channel 3
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk allowed vlan 18,28,38,48,58
    exit

    interface range fa0/13 - 15
    switchport trunk encapsulation dot1q
    channel-group 3 mode desirable      ! ← PAgP = desirable
    exit
    !fin
```
# Pruebas de Conectividad

# Presupuesto
