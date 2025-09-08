

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

* configuracion de Swtich Servidor
```
enable 
configure terminal 
    vtp domain C5_FIUCom
    vtp mode server
    vtp password proyecto12025

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
    spanning-tree mode pvst
```

# Pruebas de Conectividad

# Presupuesto
