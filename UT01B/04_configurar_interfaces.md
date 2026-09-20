# Configurar las interfaces

Identifica primero la interfaz mediante su dirección MAC. Después utiliza la guía correspondiente al sistema operativo.

## Windows Server 2019

[Configurar la red en Windows Server 2019](../UT01/tut/01_red_en_WS19.md)

Permite configurar DHCP o una dirección manual desde las propiedades de IPv4 y comprobar el resultado con PowerShell.

## Ubuntu Desktop

[Configurar la red en Ubuntu Desktop](../UT01/tut/01_red_en_Udkp.md)

Explica la configuración gráfica de una conexión automática o manual.

## Ubuntu Server

[Configurar la red en Ubuntu Server con Netplan](../UT01/tut/01_red_en_US.md)

Incluye un ejemplo con un adaptador NAT mediante DHCP y otro Host-Only con dirección estática.

## Identificar interfaces Linux

[Interfaces de red en Linux](../UT01/UT01_interfaces_red.md)

Utiliza esta ampliación si necesitas interpretar nombres como `enp0s3`, `enp0s8` o `ens33`.

## Comprobación mínima

Al terminar la configuración debes conocer, para cada interfaz:

- su nombre dentro del sistema;
- su dirección MAC;
- el modo de red utilizado en VirtualBox;
- la dirección IP y el prefijo;
- si aporta o no una ruta predeterminada.

Continúa con [Comprobar conectividad y SSH](./05_comprobar_conectividad_y_ssh.md).
