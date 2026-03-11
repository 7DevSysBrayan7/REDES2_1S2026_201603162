# MANUAL TECNICO - PROYECTO 1 - 201603162

## Subnetting
Primero que nada antes de comenzar con las vlans, vtp, etherchannel, etc.... Es conveniente mejor hacer primero la parte de la
división de las redes que se disponen para trabajar, y asi conocer en que rango de direcciones se puede utilizar para cada cosa.
Las redes que se cuentan para el proyecto son en este caso:
- **En el uso de VLANS** -> 192.188.62.0/24
- **En el uso de enrutamineto** -> 10.4.62.0/24

Lo primero que se puede notar es que en relación a la topologia utilizar un /24 que son 256 hosts (254 hosts disponibles) es demasiado ya que en este caso las 5 vlans que tiene el proyecto no pasan de 2 hosts cada una, con lo cual con un /30 que son 4 hosts (2 hosts disponibles) podria cumplir con las necesidades. Pero con el motivo de tener un margen se trabaja con /29 que son 8 hosts (6 hosts disponibles). Esto seria para el caso de **VLSM**

- **En el uso de VLANS** -> 192.188.62.0/29
- **En el uso de enrutamiento** -> 192.188.62.0/30

## Tabla de VLANS (VLSM)
| VLAN | Subred/29        | Rango de Hosts   | Broadcast     | Gateway       |
|------|------------------|------------------|---------------|---------------|
| 10   | 192.188.62.0/29  | 192-188.62.1-6   | 192.188.62.7  | 192.188.62.1  |
| 20   | 192.188.62.8/29  | 192-188.62.9-14  | 192.188.62.15 | 192.188.62.9  |
| 50   | 192.188.62.16/29 | 192-188.62.17-22 | 192.188.62.23 | 192.188.62.17 |
| 70   | 192.188.62.24/29 | 192-188.62.25-30 | 192.188.62.31 | 192.188.62.25 |
| 99   | 192.188.62.32/29 | 192-188.62.33-38 | 192.188.62.39 | 192.188.62.33 |

Ahora bien para el caso de **FLSM**:

## Tabla para VLANS (FLSM)
| VLAN | Subred/27         | Rango de Hosts     | Broadcast      | Gateway        |
|------|-------------------|--------------------|----------------|----------------|
| 10   | 192.188.62.0/27   | 192-188.62.1-30    | 192.188.62.31  | 192.188.62.1   |
| 20   | 192.188.62.32/27  | 192-188.62.33-14   | 192.188.62.63  | 192.188.62.33  |
| 50   | 192.188.62.64/27  | 192-188.62.65-94   | 192.188.62.95  | 192.188.62.65  |
| 70   | 192.188.62.96/27  | 192-188.62.97-126  | 192.188.62.127 | 192.188.62.97  |
| 99   | 192.188.62.128/27 | 192-188.62.129-158 | 192.188.62.159 | 192.188.62.129 |

**NOTA:** Se utilizó la tabla de **VLSM**

## Tabla de Enrutamiento Dinamico
