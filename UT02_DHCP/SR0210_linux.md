# DHCP en Ubuntu Desktop

**Índice de la práctica**

- [1. Instalar el servicio](#paso-1)
- [2. IP fija e interfaz del servidor](#paso-2)
- [3. Seleccionar la interfaz DHCP](#paso-3)
- [4. Configurar una subred](#paso-4)
- [5. Validar y arrancar](#paso-5)
- [6. Conectar y comprobar el cliente](#paso-6)
- [7. Renovación y reconfiguración](#paso-7)
- [Evidencias de la práctica](#evidencias)

Configurarás el servicio DHCP en Ubuntu Desktop y comprobarás la asignación a un cliente. Utiliza los clones de UT01B y un único servidor activo en la red interna. Detén el servicio DHCP de Windows antes de empezar.

Esta guía conserva **isc-dhcp-server** para el laboratorio preparado. Es software antiguo sin soporte del fabricante; comprueba su disponibilidad en la imagen del aula. No mezcles esta configuración con Kea. [Referencia de Ubuntu](https://ubuntu.com/server/docs/how-to/networking/install-isc-dhcp-server/).

<a id="paso-1"></a>

## 1. Instalar el servicio

Conecta temporalmente el adaptador a **NAT** y ejecuta:

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

El primer arranque puede fallar porque aún no existe una configuración válida. Lo comprobaremos después de configurarlo. Si el paquete no está disponible, detente y consulta al profesor; no cambies de implementación por tu cuenta.

Apaga la máquina. Cambia ese adaptador a **Red interna**, nombre **aula**. Conecta el cliente a la misma red y desactiva los adaptadores adicionales durante la prueba. No necesitas dos subredes.

<a id="paso-2"></a>

## 2. IP fija e interfaz del servidor

En Ubuntu Desktop, abre la configuración de la conexión cableada. Identifica su MAC y compárala con VirtualBox. En IPv4 selecciona **Manual**:

- Dirección: **192.168.20.1**.
- Máscara: **255.255.255.0** o prefijo **24**.
- Gateway y DNS: vacíos; desactiva DNS automático si aparece.

Aplica y vuelve a activar la conexión. Comprueba:

```bash
ip -br link
ip -4 address
ip route
nmcli device status
```

Anota el nombre real de la interfaz del laboratorio. En los ejemplos siguientes se usa **enp0s3**: sustitúyelo si el tuyo es distinto. En Ubuntu Server conserva la configuración de red de UT01B con Netplan; no supongas que existe un archivo llamado `00-installer-config.yaml` ni configures la misma interfaz por dos vías.

<a id="paso-3"></a>

## 3. Seleccionar la interfaz DHCP

Edita `/etc/default/isc-dhcp-server`:

```bash
sudo nano /etc/default/isc-dhcp-server
```

Establece la interfaz de la red interna:

```text
INTERFACESv4="enp0s3"
INTERFACESv6=""
```

<a id="paso-4"></a>

## 4. Configurar una subred

Guarda una copia antes de editar:

```bash
sudo cp -n /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.ut02.bak
sudo nano /etc/dhcp/dhcpd.conf
```

Deja esta configuración activa, sin otros bloques de subred ni opciones contradictorias:

```text
authoritative;
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.20.0 netmask 255.255.255.0 {
    range 192.168.20.100 192.168.20.149;
    option subnet-mask 255.255.255.0;
    option domain-name "ut02.test";
}
```

`subnet` declara la red y `range` las 50 direcciones disponibles. La IP fija del servidor queda fuera del rango. `domain-name` entrega un sufijo al cliente; no instala DNS. Los tiempos de concesión están expresados en segundos. `authoritative` corresponde aquí al único servidor del laboratorio, no a la autorización en Active Directory.

No añadas `option routers` ni DNS ficticios a esta red aislada. Si otro escenario dispone de router o DNS reales, se usarán las direcciones indicadas por el profesor.

<a id="paso-5"></a>

## 5. Validar y arrancar

```bash
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
sudo systemctl restart isc-dhcp-server
sudo systemctl enable isc-dhcp-server
sudo systemctl status isc-dhcp-server --no-pager
```

Continúa solo si la validación no informa de errores y el servicio aparece **active (running)**. Si falla:

```bash
sudo journalctl -u isc-dhcp-server -n 50 --no-pager
```

Comprueba el nombre de interfaz, su IP fija, la correspondencia de subred, llaves y puntos y coma. La indentación ayuda a leer `dhcpd.conf`, pero no sigue las reglas de YAML. [Consejos para editar archivos](../UT00_editar_conf.md).

<a id="paso-6"></a>

## 6. Conectar y comprobar el cliente

Usa preferentemente el mismo cliente Windows de la práctica anterior. En IPv4 activa IP y DNS automáticos; elimina cualquier configuración manual anterior. Conéctalo a **aula** y ejecuta:

```powershell
ipconfig /release
ipconfig /renew
ipconfig /all
hostname
```

Debe recibir una IP entre **192.168.20.100 y 192.168.20.149**, máscara /24, servidor DHCP **192.168.20.1** y sufijo **ut02.test**. No debe recibir gateway. Relaciona su MAC e IP con la concesión del servidor:

```bash
sudo cat /var/lib/dhcp/dhcpd.leases
sudo journalctl -u isc-dhcp-server -n 50 --no-pager
```

El fichero puede conservar concesiones anteriores; busca el bloque activo y los mensajes recientes de tu cliente. No basta con que aparezca cualquier dirección.

Si usas Ubuntu Desktop como cliente, elige **Automático (DHCP)** en IPv4, activa DNS automático y reconecta el perfil. Consulta `nmcli device show`, `ip -4 address`, `ip -br link` y `hostname`. No necesitas instalar ni ejecutar otro cliente `dhclient` en paralelo con NetworkManager.

<a id="paso-7"></a>

## 7. Renovación y reconfiguración

Mantén visible el registro con `sudo journalctl -u isc-dhcp-server -f` y fuerza una renovación en el cliente. Identifica REQUEST y ACK si aparecen. Un cliente que conserva una concesión no tiene por qué repetir DORA completo.

Después de guardar las evidencias iniciales, ensaya una modificación a **192.168.20.0/25**: servidor **.1/25**, rango **.2–.126**, máscara **255.255.255.128** tanto en `subnet` como en `option subnet-mask`. Valida, reinicia, renueva el cliente y verifica dirección, máscara y servidor DHCP. La IP fija .1 no se reparte.

<a id="evidencias"></a>

## Evidencias de la práctica

1. Configuración inicial DHCP y servicio activo.
2. Concesión o registro del servidor junto al cliente, mostrando la correspondencia IP/MAC, servidor DHCP y sufijo recibido.
3. Configuración /25 y cliente con la nueva máscara tras renovar.

Incluye los nombres de las máquinas virtuales en las capturas y una frase por evidencia. Una dirección mostrada solo con `ip a` o `ipconfig` no demuestra por sí sola que la haya asignado este servidor.
