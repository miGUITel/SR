En Linux, las interfaces de red se identifican mediante nombres asignados a cada una de ellas, que varían en función de factores como el tipo de interfaz (física o virtual), el tipo de hardware y la configuración del sistema. Existen varios esquemas de nombres de interfaces de red, y aquí te explico cómo se identifican y cómo obtener información sobre ellas.

### 1. **Tipos de nombres de interfaces de red en Linux:**

#### a) **Esquema de nombres tradicionales (interfaz clásica)**:
En versiones más antiguas de Linux o con configuraciones que aún utilizan el esquema tradicional, las interfaces de red suelen tener nombres simples, como:
- **ethX**: Para interfaces Ethernet, donde `X` es un número (por ejemplo, `eth0`, `eth1`, etc.).
- **wlanX**: Para interfaces de red inalámbrica (Wi-Fi), donde `X` es un número (por ejemplo, `wlan0`, `wlan1`, etc.).
- **lo**: La interfaz **loopback**, que es una interfaz virtual que utiliza el propio sistema operativo para comunicarse consigo mismo (la IP suele ser `127.0.0.1`).

#### b) **Esquema de nombres predecibles (Predictable Network Interface Names)**:
A partir de **systemd v197**, muchos sistemas Linux han adoptado un esquema de nombres predecibles para evitar confusiones cuando las interfaces cambian de nombre de una sesión a otra. Estos nombres suelen estar basados en la ubicación del hardware dentro del sistema, lo que ofrece consistencia. Algunos ejemplos comunes incluyen:
- **enpXsY**: Para interfaces Ethernet, donde:
  - `en` significa **Ethernet**.
  - `X` es el número de bus PCI del dispositivo.
  - `Y` es el número de dispositivo en ese bus.
  Ejemplo: `enp1s0` (primera interfaz Ethernet en el bus PCI 1).
  
- **wlpXsY**: Para interfaces inalámbricas, donde:
  - `wl` significa **Wireless LAN**.
  - `X` es el número de bus PCI.
  - `Y` es el número de dispositivo en ese bus.
  Ejemplo: `wlp2s0` (primera interfaz inalámbrica en el bus PCI 2).

- **enxXXXXXXXXXXXX**: Para algunas interfaces Ethernet USB, donde `XXXXXXXXXXXX` representa la dirección MAC de la interfaz.
  
- **sitX**: Para interfaces de túnel IPv6 sobre IPv4.

#### c) **Interfaces virtuales o tunelizadas**:
- **tunX/tapX**: Para interfaces de túnel, como las que se utilizan en conexiones VPN (por ejemplo, `tun0`).
- **brX**: Para interfaces de puente (bridge), que se usan para unir varias interfaces de red en una red virtual (por ejemplo, `br0`).
- **vethX**: Interfaces virtuales que a menudo se utilizan para contenedores o máquinas virtuales (por ejemplo, en Docker).

### 2. **Comandos para identificar las interfaces de red en Linux**:

Para listar y obtener información sobre las interfaces de red, puedes usar una serie de comandos en la terminal:

#### a) **ip command** (comando moderno y recomendado):
El comando `ip` es parte de la suite **iproute2** y es la forma recomendada en sistemas modernos para interactuar con las interfaces de red.

- Para listar todas las interfaces de red activas y su configuración:
  ```bash
  ip addr show
  ```
  Esto mostrará todas las interfaces, tanto físicas como virtuales, junto con sus direcciones IP, estado (UP o DOWN), y detalles adicionales como direcciones MAC.

- Para listar únicamente las interfaces de red con su estado y direcciones:
  ```bash
  ip link show
  ```

#### b) **ifconfig** (comando antiguo):
En sistemas más antiguos o si tienes instaladas herramientas de red más tradicionales, puedes usar el comando **ifconfig**.

- Para listar las interfaces de red activas:
  ```bash
  ifconfig
  ```
  Esto también mostrará detalles sobre cada interfaz, como las direcciones IP y MAC.

#### c) **ls /sys/class/net**:
Puedes listar las interfaces disponibles directamente desde el sistema de archivos virtual de Linux:
  ```bash
  ls /sys/class/net
  ```
  Esto te dará los nombres de todas las interfaces de red, tanto activas como inactivas.

#### d) **nmcli** (NetworkManager):
Si estás utilizando **NetworkManager** para gestionar las conexiones de red, el comando `nmcli` es muy útil para obtener información.

- Para listar todas las interfaces de red:
  ```bash
  nmcli device
  ```
  Este comando te mostrará las interfaces disponibles, su tipo, estado y otras propiedades.

#### e) **ethtool**:
El comando `ethtool` proporciona información avanzada sobre interfaces Ethernet (como la velocidad del enlace o el estado del mismo).

- Para verificar los detalles de una interfaz específica (por ejemplo, `enp1s0`):
  ```bash
  ethtool enp1s0
  ```

#### f) **lshw**:
El comando `lshw` muestra información detallada del hardware de un sistema, incluidas las interfaces de red.

- Para listar las interfaces de red:
  ```bash
  sudo lshw -class network
  ```

### 3. **Ejemplos de nombres de interfaces**:
Aquí tienes algunos ejemplos reales de nombres de interfaces en diferentes configuraciones:

- **enp3s0**: Primera interfaz Ethernet en el bus PCI 3.
- **wlp2s0**: Primera interfaz inalámbrica en el bus PCI 2.
- **lo**: Interfaz loopback, que siempre estará presente en cualquier sistema Linux.
- **enx001122334455**: Una interfaz Ethernet conectada a través de USB con una dirección MAC `00:11:22:33:44:55`.
- **tun0**: Una interfaz de túnel, típicamente utilizada en VPNs.
- **docker0**: Una interfaz de red virtual creada por Docker para permitir la comunicación entre contenedores.

### 4. **Cambiar o personalizar el nombre de la interfaz**:
Si prefieres utilizar nombres tradicionales como `eth0` o quieres personalizar el nombre de la interfaz, puedes hacerlo mediante las reglas de **udev**. Sin embargo, esto no se recomienda en la mayoría de los casos, ya que el esquema de nombres predecibles garantiza consistencia y evita conflictos cuando se agregan o eliminan interfaces de red.