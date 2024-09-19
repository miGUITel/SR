Para instalar y configurar un servidor **ISC DHCP** en Linux Mint en modo anónimo, sigue estos pasos:

### 1. **Instalar el servidor DHCP ISC**

Abre una terminal y ejecuta el siguiente comando para instalar el servidor ISC DHCP:

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

### 2. **Configurar la interfaz de red**
Debes indicar en qué interfaz de red quieres que el servidor DHCP opere. Por ejemplo, si tu interfaz de red es `eth0`, edita el archivo de configuración del servidor:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Busca la línea donde se define la variable `INTERFACESv4` y modifícala así:

```bash
INTERFACESv4="eth0"
```

Cambia `eth0` por la interfaz de red que corresponda en tu caso. Puedes verificar las interfaces de red con el comando:

```bash
ip a
```

### 3. **Configurar el archivo dhcpd.conf**

El archivo principal de configuración del servidor DHCP está en `/etc/dhcp/dhcpd.conf`. Primero, crea una copia de seguridad:

```bash
sudo cp /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bak
```

Luego, edita el archivo con:

```bash
sudo nano /etc/dhcp/dhcpd.conf
```

Agrega o modifica las siguientes líneas para configurar el servidor en modo "anónimo", es decir, sin reservas de IP específicas:

```bash
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0 {
  range 192.168.1.100 192.168.1.200;
  option routers 192.168.1.1;
  option subnet-mask 255.255.255.0;
  option domain-name-servers 8.8.8.8, 8.8.4.4;
  option broadcast-address 192.168.1.255;
}
```

- **range**: Define el rango de IP que se asignará dinámicamente a los dispositivos.
- **option routers**: Define la puerta de enlace predeterminada (tu router).
- **option domain-name-servers**: Define los servidores DNS (puedes usar los de Google, 8.8.8.8 y 8.8.4.4).

### 4. **Configurar permisos y reiniciar el servicio**

Asegúrate de que la configuración esté correcta ejecutando el comando de comprobación:

```bash
sudo dhcpd -t
```

Si no hay errores, reinicia el servicio para que los cambios surtan efecto:

```bash
sudo systemctl restart isc-dhcp-server
```

### 5. **Habilitar el servicio DHCP en el arranque**

Para asegurarte de que el servidor DHCP arranca automáticamente con el sistema, usa:

```bash
sudo systemctl enable isc-dhcp-server
```

### 6. **Verificar funcionamiento**

Finalmente, puedes verificar que el servidor DHCP está funcionando correctamente revisando los logs:

```bash
journalctl -xe | grep dhcpd
```

O bien, comprobando que los dispositivos en la red están recibiendo direcciones IP en el rango configurado.

