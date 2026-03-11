# MANUAL TECNICO - PROYECTO 1 - 201603162

## Subnetting
Primero que nada antes de comenzar con las vlans, vtp, etherchannel, etc.... Es conveniente mejor hacer primero la parte de la
división de las redes que se disponen para trabajar, y asi conocer en que rango de direcciones se puede utilizar para cada cosa.
Las redes que se cuentan para el proyecto son en este caso:
- **En el uso de VLANS** -> 192.188.62.0/24
- **En el uso de enrutamineto** -> 10.4.62.0/24

Lo primero que se puede notar es que en relación a la topologia utilizar un /24 que son 256 hosts (254 hosts disponibles) es demasiado ya que en este caso
las 5 vlans que tiene el proyecto no pasan de 2 hosts cada una, con lo cual con un /30 que son 4 hosts (2 hosts disponibles) podria cumplir con las necesidades
pero con el motivo de tener un margen se trabaja con /29 que son 6 hosts (4 hosts disponibles). Esto ya pensando un poco mas en el ahorro de direcciones.
Ya que con FLSM considerando que son 5 las vlans a trabajar, 24 (red original) + 3 bits de la subred = 27 con un /27 que daria 32 hosts (30 disponibles) pero aun asi
hay desperdicio de direcciones. Entonces se escogió trabajar con /29 para vlans y con respecto a enrutamiento funciona muy bien la /30. Entonces las redes quedan:

- **En el uso de VLANS** -> 192.188.62.0/29
- **En el uso de enrutamiento** -> 192.188.62.0/30

## Tabla para VLANS
