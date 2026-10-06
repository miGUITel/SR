# DHCP en Ubuntu Desktop

**Índice de la práctica**

- [DHCP en Ubuntu Desktop](#dhcp-en-ubuntu-desktop)
  - [1. Instalar el servicio](#1-instalar-el-servicio)
  - [2. IP fija e interfaz del servidor](#2-ip-fija-e-interfaz-del-servidor)
  - [3. Seleccionar la interfaz DHCP](#3-seleccionar-la-interfaz-dhcp)
  - [4. Configurar una subred](#4-configurar-una-subred)
  - [5. Validar y arrancar](#5-validar-y-arrancar)
  - [6. Conectar y comprobar el cliente](#6-conectar-y-comprobar-el-cliente)
  - [7. Renovación y reconfiguración](#7-renovación-y-reconfiguración)
  - [Evidencias de la práctica](#evidencias-de-la-práctica)

Configurarás el servicio DHCP en Ubuntu Desktop y comprobarás la asignación a un cliente. Utiliza los clones de UT01B y un único servidor activo en la red interna. Detén el servicio DHCP de Windows antes de empezar.

Esta guía conserva **isc-dhcp-server** para el laboratorio. Es software antiguo sin soporte del fabricante pero es un *buen sistema de aprendizaje*. [Referencia de Ubuntu](https://ubuntu.com/server/docs/how-to/networking/install-isc-dhcp-server/).

<a id="paso-1"></a>

## 1. Instalar el servicio

Conecta temporalmente el adaptador a **NAT** y ejecuta:

```bash
sudo apt update
sudo apt install isc-dhcp-server
```

El primer arranque puede fallar porque aún no existe una configuración válida. Lo comprobaremos después de configurarlo. Si el paquete no está disponible, detente y consulta al profesor; no cambies de implementación por tu cuenta.

Apaga la máquina. Cambia ese adaptador a **Red interna**, nombre **aula**. Conecta el cliente a la misma red y desactiva los adaptadores adicionales durante la prueba. No necesitas dos subredes.


**Esta red interna está aislada:** durante la prueba no tendrás acceso a Internet, pero podrás comunicarte con los equipos de `aula` que tengan una configuración IP compatible. El servidor DHCP no actúa como router por tener la dirección **192.168.20.1**.

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

Anota el nombre real de la interfaz del laboratorio. En los ejemplos siguientes se usa **enp0s3**: sustitúyelo si el tuyo es distinto. En esta práctica configura la interfaz desde la conexión cableada de Ubuntu Desktop; evita modificar también su configuración mediante archivos de Netplan.

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

`subnet` declara la red y `range` delimita las 50 direcciones que se pueden repartir dinámicamente. La IP fija del servidor queda fuera del rango. `domain-name` entrega un sufijo al cliente; no instala DNS ni crea registros de nombres. Los tiempos de concesión están expresados en segundos: 600 son 10 minutos y 7200 son 2 horas.

`authoritative;` declara que este servidor tiene autoridad sobre la red y permite responder con DHCPNAK ante solicitudes de configuraciones que no sean válidas para ella. No impide que funcionen otros servidores DHCP ni equivale a la autorización en Active Directory; por eso debes detener los demás servidores del laboratorio.

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
ipconfig /release  # Libera la concesión DHCP actual.
ipconfig /renew    # Solicita una concesión DHCP.
ipconfig /all      # Muestra la configuración completa de red.
hostname           # Muestra el nombre del equipo.
```

Debe recibir una IP entre **192.168.20.100 y 192.168.20.149**, máscara /24, servidor DHCP **192.168.20.1** y sufijo **ut02.test**. No debe recibir gateway. Relaciona su MAC e IP con la concesión del servidor:

```bash
sudo cat /var/lib/dhcp/dhcpd.leases
sudo journalctl -u isc-dhcp-server -n 50 --no-pager
```

El fichero puede conservar varias declaraciones de una misma concesión. Si una dirección aparece varias veces, toma como referencia su última declaración en el archivo. Comprueba el estado de la concesión (`binding state active;`), su vigencia y la correspondencia entre IP y MAC. Las fechas `starts` y `ends` están expresadas en UTC por defecto; puedes consultar la hora actual en UTC con `date -u`. Contrasta estos datos con los mensajes recientes de tu cliente: no basta con que aparezca cualquier dirección.

Si usas Ubuntu Desktop como cliente, elige **Automático (DHCP)** en IPv4, activa DNS automático y reconecta el perfil. Consulta `nmcli device show`, `ip -4 address`, `ip -br link` y `hostname`. No necesitas instalar ni ejecutar otro cliente `dhclient` en paralelo con NetworkManager.

<a id="paso-7"></a>

## 7. Renovación y reconfiguración

En el servidor, mantén visible el registro con `sudo journalctl -u isc-dhcp-server -f`. En el cliente Windows, ejecuta únicamente `ipconfig /renew`, sin ejecutar antes `/release`, para solicitar la renovación de la concesión actual. Identifica DHCPREQUEST y DHCPACK si aparecen. Un cliente que conserva una concesión no tiene por qué repetir DORA completo. Cuando termines de observar el registro, pulsa **Ctrl+C**; esto detiene el seguimiento, no el servicio DHCP.

Después de guardar las evidencias iniciales, modifica la configuración para utilizar **192.168.20.0/25** y un nuevo rango DHCP que no se solape con el anterior:

1. En la configuración de la conexión cableada del servidor, conserva **192.168.20.1**, cambia el prefijo a **25** (máscara **255.255.255.128**) y aplica los cambios. Si es necesario, desactiva y vuelve a activar la conexión. Comprueba con `ip -4 address` que la interfaz tiene **192.168.20.1/25**.
2. En `/etc/dhcp/dhcpd.conf`, cambia la máscara de `subnet` y de `option subnet-mask` a **255.255.255.128**, y sustituye la línea del rango por `range 192.168.20.20 192.168.20.69;`. La IP fija del servidor queda fuera del rango.
3. Valida la configuración y reinicia el servicio DHCP.
4. En el cliente Windows, ejecuta `ipconfig /release`, `ipconfig /renew` e `ipconfig /all`. Si utilizas Ubuntu Desktop como cliente, desconecta y vuelve a conectar su perfil de red.
5. Comprueba que el cliente recibe una dirección entre **192.168.20.20 y 192.168.20.69**, distinta de la inicial, y la máscara **255.255.255.128**. El servidor DHCP debe seguir siendo **192.168.20.1**.

Los dos rangos no se solapan: ninguna dirección del rango inicial pertenece al nuevo. Por eso, al obtener una concesión válida para la nueva configuración, el cliente debe cambiar de IP. **Cambiar la máscara, por sí solo, no obliga siempre a cambiar la dirección IP.**

La subred pasa de **254 a 126 hosts utilizables**, pero ambos rangos DHCP contienen **50 direcciones**. Distingue la capacidad total de la subred del número de direcciones que hemos decidido repartir.



<a id="evidencias"></a>

## Evidencias de la práctica

1. Configuración inicial DHCP y servicio activo.
2. Concesión o registro del servidor junto al cliente, mostrando la correspondencia IP/MAC, servidor DHCP y sufijo recibido.
3. Configuración DHCP /25, interfaz del servidor con 192.168.20.1/25 y cliente con una IP del nuevo rango 192.168.20.20–192.168.20.69, distinta de la inicial. Muestra también la nueva máscara y el servidor DHCP recibido.
4. Responde a las preguntas de comprensión.

Incluye los nombres de las máquinas virtuales en las capturas y una frase por evidencia. Una dirección mostrada solo con `ip a` o `ipconfig` no demuestra por sí sola que la haya asignado este servidor.


**Preguntas de comprensión**

1. ¿Por qué la dirección del servidor queda fuera del rango DHCP?
2. ¿Por qué puedes comunicarte con el servidor aunque no tengas puerta de enlace?
3. ¿Puede una renovación mantener la misma dirección IP? Justifica tu respuesta.
4. Al pasar de /24 a /25, ¿cuántos hosts utilizables admite cada subred? ¿Por qué podemos seguir repartiendo 50 direcciones?
5. ¿Qué modificación garantiza que todos los clientes deban recibir una IP distinta de la inicial? Si solo cambiásemos la máscara de /24 a /25, ¿tendrían que cambiar de IP todos los clientes? Razona utilizando como ejemplos las direcciones 192.168.20.100 y 192.168.20.140.
