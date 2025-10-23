#  Manual Técnico — Proyecto 2: NetUSAC

## 1. Información General
**Curso:** Redes de Computadoras 1  
**Proyecto:** NetUSAC — Proyecto de Interconexión  
**Universidad:** Universidad de San Carlos de Guatemala  
**Estudiante:** *[Tu Nombre Completo]*  
**Carnet:** 202204578  

---

## 2. Resumen del Proyecto
Este proyecto consiste en el diseño, configuración y simulación de una red interedificios para el campus central de la USAC, implementando segmentación mediante VLANs, protocolos de enrutamiento (RIP, OSPF, EIGRP), redundancia (HSRP/VRRP), ACLs y redistribución de rutas.  
El objetivo principal es lograr una infraestructura segura, escalable y eficiente, tanto a nivel lógico (simulación) como físico.

---

## 3. Topología General

### 3.1. Diagrama General
![Topología NetUSAC](imagenes/topologia_general.png)

### 3.2. Descripción del Backbone
- Conecta los cinco edificios principales: DIGA, Biblioteca Central, T4, S11, S12.  
- Protocolos implementados: RIP, OSPF, EIGRP y rutas estáticas.  
- Redistribución en R4, R6 y R7.  
- EtherChannel entre MS3, MS4 y MS6.  

---

## 4. Detalle por Edificio

---

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
- **HSRP:** Entre MS1 y MS2.  
- **RIP:** Comunicación con Biblioteca.  
- **ACLs:** Permiten solo tráfico entre VLANs iguales.  
- **Comandos:**
