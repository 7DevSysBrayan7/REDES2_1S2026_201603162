# Proyecto No. 2

## Descripcion del proyecto

Un país con una población en rápido crecimiento y una demanda de telecomunicaciones en auge ha decidido modernizar y ampliar su infraestructura de red para satisfacer las necesidades actuales y futuras de sus ciudadanos. Tres de las principales empresas de telecomunicaciones han sido seleccionadas para colaborar en este ambicioso proyecto de expansión: Telecom Uno, Redes Nacionales y Link Global.

El gobierno, junto con estas empresas, ha creado un comité de expertos que se encargarán del diseño, simulación y presentación de una solución eficiente, escalable y económicamente viable. Dado su extensa experiencia en el campo, el comité ha decidido seleccionarlo a usted para llevar a cabo este desafío.

El país cuenta con tres empresas de telecomunicaciones interesadas en optimizar sus redes internas y mejorar la interconectividad entre ellas para ofrecer un mejor servicio a nivel nacional. Cada ISP tiene necesidades y requerimientos específicos para sus redes internas, y, además, se debe garantizar una conectividad óptima y segura entre las tres empresas a través de un protocolo de enrutamiento común.

## Vlans y Departamentos

## Topologias

### Arbol

```text
        [ R1 CORE ]
      /    |   
  DNS/HTTP  LACP1  LACP2
    /           
  [R2]          [R3]
Administración  Atención Cliente
      |                |
    R4              R5
      |                |
=================  =================
| SWITCH VLAN 10|  | SWITCH VLAN 20|
=================  =================
    / |            / |
  PC1 PC2 PC3    PC4 PC5 PC6
```

### Jerarquica

```text
                                     [ INTERNET ]
                          |
                  ==================
                  |  R1 CORE ISP  |
                  ==================
                  |          |
                  |        [SERVER PT]
                  |        (DHCP)
                  |
            -------------------------
            |                      |
    R2 (VENTAS)            R3 (FACTURACION)
                                  /
                                /
                                /
              ---- R4 (DISTRIBUCION) ----
                          |
                R5 (SERVICIOS / BACKUP)
                          |
                  ==================
                  | SWITCH CORE  |
                  | (L2 + LACP)  |
                  ==================
                    /           
          SW VENTAS            SW FACTURA
        VLAN 10                VLAN 20
        PCs                  PCs





                R1
              / 
            R2    R3
                /
                R4
                |
        -----------------
        |              |
      R5            R6  ← HSRP (nivel servicios)
        |              |
        ---- SWITCH CORE ----
            /       
      SW VENTAS    SW FACT
```

### Hub n Spoke

```text
                [ INTERNET ]
                    |
              [ R1 HUB CORE ]
            (EIGRP + Control)
                    |
              ==================
              | SWITCH HUB    |
              | VLAN 10/20/30  |
              ==================
                /        |     
              /        |     
        [ WIFI ROUTER ] (LACP 1) (LACP 2)
            |            |        |
        WLAN USERS  [ R2 SOPORTE ] [ R3 SEGURIDAD ]
                        |            |
                  =================  =================
                  | SWITCH SOPORTE| | SWITCH SEGUR |
                  | VLAN 10      | | VLAN 20      |
                  =================  =================
                      | | |          | | |
                    PC1 PC2 PC3    PC4 PC5
```

## Area de Redes Nacionales

### Tabla de Subnetting




---

# SUBNETTING

## VLAN 10 – VENTAS

| Elemento | Valor |
|----------|------|
| Red | 172.16.22.0/27 |
| Máscara | 255.255.255.224 |
| Rango Hosts | 172.16.22.1 – 172.16.22.30 |
| Broadcast | 172.16.22.31 |
| Gateway HSRP | 172.16.22.1 |

---

## VLAN 20 – FACTURACIÓN

| Elemento | Valor |
|----------|------|
| Red | 172.16.22.32/27 |
| Máscara | 255.255.255.224 |
| Rango Hosts | 172.16.22.33 – 172.16.22.62 |
| Broadcast | 172.16.22.63 |
| Gateway HSRP | 172.16.22.33 |

---

## RED SERVIDOR DHCP

| Elemento | Valor |
|----------|------|
| Red | 172.16.22.64/28 |
| Máscara | 255.255.255.240 |
| Rango Hosts | 172.16.22.65 – 172.16.22.78 |
| Broadcast | 172.16.22.79 |
| Server DHCP | 172.16.22.67 |
| Gateway | 172.16.22.65 |

---

# CONEXIONES FÍSICAS

## CORE ROUTERS

| Dispositivo | Interfaz | Conexión |
|-------------|----------|----------|
| R1 | G0/0 | R2 G0/1 |
| R1 | G0/1 | R3 G0/0 |
| R2 | G0/0 | R4 G0/0 |
| R3 | G0/2 | R4 G0/2 |
| R4 | G0/1 | R5 G0/1 |

---

## LAN / SWITCH CORE

| Dispositivo | Interfaz | Conexión | Modo |
|-------------|----------|----------|------|
| R2 | G0/2 | Switch Core | Trunk |
| R3 | G0/1 | Switch Core | Trunk |
| Server DHCP | NIC | Switch Core | Access |
| PCs Ventas | NIC | VLAN 10 | Access |
| PCs Facturación | NIC | VLAN 20 | Access |

---

# HSRP

## VLAN 10 – VENTAS

| Router | Rol | IP |
|--------|-----|----|
| R2 | ACTIVE | 172.16.22.2 |
| R3 | STANDBY | 172.16.22.3 |
| VIP | — | 172.16.22.1 |

---

## VLAN 20 – FACTURACIÓN

| Router | Rol | IP |
|--------|-----|----|
| R3 | ACTIVE | 172.16.22.35 |
| R2 | STANDBY | 172.16.22.34 |
| VIP | — | 172.16.22.33 |

---

# DHCP SERVER

| Parámetro | Valor |
|----------|------|
| IP | 172.16.22.67 |
| Máscara | 255.255.255.240 |
| Gateway | 172.16.22.65 |
| DNS | 8.8.8.8 |

---

## POOL VENTAS

| Campo | Valor |
|------|------|
| Red | 172.16.22.0/27 |
| Gateway | 172.16.22.1 |
| Rango | 172.16.22.10 – 172.16.22.30 |

---

## POOL FACTURACIÓN

| Campo | Valor |
|------|------|
| Red | 172.16.22.32/27 |
| Gateway | 172.16.22.33 |
| Rango | 172.16.22.40 – 172.16.22.62 |

---

# SERVICIOS IMPLEMENTADOS

| Servicio | Estado |
|----------|--------|
| VLANs | ✔ |
| Trunking | ✔ |
| Router-on-a-Stick | ✔ |
| HSRP | ✔ |
| DHCP | ✔ |
| IP Helper | ✔ |
| OSPF | ✔ |
| Conectividad | ✔ |

---

# ESTADO FINAL

| Área | Estado |
|------|--------|
| Acceso | OK |
| Distribución | OK |
| Core | OK |
| Servicios | OK |
| Red | FUNCIONAL |