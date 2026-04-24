# Proyecto No. 2

## Descripcion del proyecto

Un país con una población en rápido crecimiento y una demanda de telecomunicaciones en auge ha decidido modernizar y ampliar su infraestructura de red para satisfacer las necesidades actuales y futuras de sus ciudadanos. Tres de las principales empresas de telecomunicaciones han sido seleccionadas para colaborar en este ambicioso proyecto de expansión: Telecom Uno, Redes Nacionales y Link Global.

El gobierno, junto con estas empresas, ha creado un comité de expertos que se encargarán del diseño, simulación y presentación de una solución eficiente, escalable y económicamente viable. Dado su extensa experiencia en el campo, el comité ha decidido seleccionarlo a usted para llevar a cabo este desafío.

El país cuenta con tres empresas de telecomunicaciones interesadas en optimizar sus redes internas y mejorar la interconectividad entre ellas para ofrecer un mejor servicio a nivel nacional. Cada ISP tiene necesidades y requerimientos específicos para sus redes internas, y, además, se debe garantizar una conectividad óptima y segura entre las tres empresas a través de un protocolo de enrutamiento común.

## Vlans y Departamentos

## Topologias

### Arbol

```

              [ R1 CORE ]
            /      |     
  DNS/HTTP    LACP1    LACP2
          /             
      [R2]            [R3]
    Administración  Atención Cliente
        |                |
      R4              R5
        |                |
  =================  =================
  | SWITCH VLAN 10|  | SWITCH VLAN 20|
  =================  =================
      /  |            /  | 
    PC1  PC2  PC3    PC4  PC5  PC6
```

### Jerarquica

```

                          [ INTERNET / ISP ]
                                |
                        ===================
                        | R1 CORE (HSRP) |
                        | Gateway Virtual |
                        ===================
                                |
                        ===================
                        | SWITCH CORE    |
                        ===================
                                |
                        [ SERVER PT DHCP ]
                                |
                --------------------------------
                |                              |
          (LACP TRUNK 1)                (LACP TRUNK 2)
                |                              |
        ===================          ===================
        | R2 VENTAS      |          | R3 FACTURACION |
        ===================          ===================
                |                              |
          ==================          ==================
          | SWITCH VENTAS |          | SWITCH FACTURA |
          | VLAN 10      |          | VLAN 20        |
          ==================          ==================
              |  |  |                    |  |  |
            PC1 PC2 PC3                PC4 PC5 PC6
```

### Hub n Spoke

```
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
          [ WIFI ROUTER ]  (LACP 1)  (LACP 2)
                |            |            |
            WLAN USERS  [ R2 SOPORTE ] [ R3 SEGURIDAD ]
                                |              |
                        =================  =================
                        | SWITCH SOPORTE|  | SWITCH SEGUR |
                        | VLAN 10      |  | VLAN 20      |
                        =================  =================
                          |  |  |          |  |  |
                        PC1 PC2 PC3      PC4 PC5
```