# Configurar la red en Ubuntu Server con Netplan

## 1. Identificar las interfaces

Ejecuta:

```bash
ip link show
ip addr show
```

`ip link show` permite reconocer las interfaces, su estado y su dirección MAC. `ip addr show` muestra también las direcciones IP. Los nombres pueden ser, por ejemplo, `enp0s3`, `enp0s8`, `ens33` o `eth0`.

## 2. Editar la configuración

Los archivos de Netplan están en `/etc/netplan/`. Comprueba primero el nombre del archivo:

```bash
ls /etc/netplan/
```

Después, ábrelo con un editor. Por ejemplo:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

## 3. Ejemplo para UT01B: NAT y Host-Only

En este ejemplo:

- `enp0s3` es el adaptador NAT y obtiene automáticamente su configuración.
- `enp0s8` es el adaptador Host-Only y utiliza una dirección estática para comunicarse con las máquinas del escenario.

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      dhcp4: false
      addresses:
        - 192.168.56.10/24
```

Adapta los nombres de interfaz y la dirección al escenario. En la interfaz Host-Only no se configura una puerta de enlace ni un DNS ficticios. El adaptador NAT proporciona la ruta predeterminada y el DNS cuando se necesita acceso al exterior.

La indentación de YAML debe hacerse con espacios, no con tabuladores.

## 4. Validar y aplicar

Guarda en Nano con `Ctrl+O`, confirma con `Intro` y sal con `Ctrl+X`.

Valida temporalmente la configuración:

```bash
sudo netplan try
```

Si es correcta, aplícala:

```bash
sudo netplan apply
```

## 5. Comprobar

Sustituye `<IP-del-otro-equipo>` por la dirección de otra máquina del escenario:

```bash
ip link show
ip addr show
ip route show
ping <IP-del-otro-equipo>
```

Primero se comprueban la interfaz, la dirección, el prefijo, las rutas y la comunicación entre las máquinas. Solo si el escenario incluye salida exterior se comprueban después la puerta de enlace, una dirección pública y la resolución de nombres.

Consulta también [Comprobar una red interna o Host-Only](../diagnostico_red_virtual.md).
