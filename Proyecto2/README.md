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

| Segmento / Enlace          | Subred            | Máscara              | Rango de Hosts                | Gateway / Uso principal |
|---------------------------|-------------------|----------------------|-------------------------------|--------------------------|
| VLAN 10 (Ventas)          | 172.16.22.0/27    | 255.255.255.224      | 172.16.22.1 – 172.16.22.30    | R2 / HSRP VIP 172.16.22.1 |
| VLAN 20 (Facturación)    | 172.16.22.32/27  | 255.255.255.224      | 172.16.22.33 – 172.16.22.62  | R3 / HSRP VIP 172.16.22.33 |
| Red Servicios (R5 + SW + Server) | 172.16.22.64/28 | 255.255.255.240 | 172.16.22.65 – 172.16.22.78 | R5 / DHCP Server 172.16.22.70 |
| R1 ↔ R2 (WAN)            | 172.16.22.80/30  | 255.255.255.252      | 172.16.22.81 – 172.16.22.82  | Punto a punto OSPF |
| R1 ↔ R3 (WAN)            | 172.16.22.84/30  | 255.255.255.252      | 172.16.22.85 – 172.16.22.86  | Punto a punto OSPF |
| R2 ↔ R4 (WAN)            | 172.16.22.88/30  | 255.255.255.252      | 172.16.22.89 – 172.16.22.90  | Punto a punto OSPF |
| R3 ↔ R4 (WAN)            | 172.16.22.92/30  | 255.255.255.252      | 172.16.22.93 – 172.16.22.94  | Punto a punto OSPF |
| R4 ↔ R5 (WAN)            | 172.16.22.96/30  | 255.255.255.252      | 172.16.22.97 – 172.16.22.98  | Backbone OSPF |
| R1 ↔ ISP / SM externo    | 172.16.22.100/30  | 255.255.255.252      | 172.16.22.101 – 172.16.22.102 | Salida exterior |





| Dispositivo A | Interfaz A | Dispositivo B | Interfaz B | Tipo de enlace |
|---------------|------------|---------------|------------|-----------------|
| R1 | G0/1 | R2 | G0/1 | Enlace OSPF |
| R1 | G0/0 | R3 | G0/0 | Enlace OSPF |
| R2 | G0/0 | R4 | G0/0 | Enlace OSPF |
| R3 | G0/2 | R4 | G0/2 | Enlace OSPF |
| R4 | G0/1 | R5 | G0/1 | Enlace OSPF / Backbone |
| R5 | G0/0 | SW CORE | Fa0/5 | Acceso a red LAN |
| R5 | G0/2 | SERVER PT DHCP | Fa0/0 | Servicio DHCP |