

## **UT02 TA03 - ASIGNACIÓN IP Fija, Estática, Automática y Dinámica**

[Volver al índice](../README.md)

En esta sección se explicarán las diferencias clave entre los métodos de asignación de direcciones IP: IP fija desde el sistema operativo, IP estática mediante DHCP, IP automática mediante DHCP y IP dinámica. Cada uno tiene sus aplicaciones particulares para optimizar la gestión de dispositivos en una red.

### IP fija desde el sistema operativo
Una IP fija configurada manualmente desde el sistema operativo implica que el administrador asigna manualmente la dirección IP, la máscara de subred, la puerta de enlace y el DNS en el dispositivo. Esta IP no cambiará a menos que el administrador la modifique. Es adecuada para dispositivos que requieren una dirección IP constante, como servidores o impresoras.

**Características**:
- Configurada manualmente en el dispositivo.
- No depende de un servidor DHCP.
- Requiere una gestión cuidadosa para evitar conflictos de IP.
- Ideal para dispositivos que necesitan una dirección IP constante y predecible.

### IP estática mediante DHCP
La asignación estática de IP se realiza desde un servidor DHCP, que asigna una dirección IP concreta a un dispositivo específico, identificado por su dirección MAC o un identificador de cliente. La IP asignada permanece fija para ese cliente, asegurando que siempre reciba la misma IP.

**Características**:
- Asignada y gestionada por un servidor DHCP.
- La IP permanece fija mientras el dispositivo esté conectado a la red.
- Requiere configuración previa en el servidor DHCP para asociar la IP a la dirección MAC del cliente.
- Útil para asegurar que dispositivos críticos mantengan la misma IP sin necesidad de configuración manual.

### IP automática mediante DHCP
En la asignación automática, el servidor DHCP asigna una dirección IP a un cliente cuando solicita conectarse por primera vez. El cliente mantendrá esta IP durante el tiempo en que el lease (concesión) esté activo, y mientras no haya cambios significativos en la red, es probable que siga recibiendo la misma dirección IP cada vez que se conecte. Sin embargo, no está garantizado que esta IP sea siempre la misma, ya que, una vez que el lease expira o si el cliente libera la IP, podría recibir una nueva dirección en futuras solicitudes si el servidor DHCP así lo decide o si otros dispositivos han ocupado esa IP.

**Características**:
- La dirección IP es asignada automáticamente por el servidor DHCP.
- El cliente puede mantener la misma IP si sigue en la red y la concesión no ha expirado.
- No se garantiza que la IP sea siempre la misma si hay cambios en la red.
- Es adecuada para redes donde no es estrictamente necesario que los clientes tengan una IP fija, pero donde mantener la misma IP facilita la gestión.

### IP dinámica mediante DHCP
La asignación dinámica se basa en un rango de direcciones IP que el servidor DHCP distribuye entre los clientes que lo solicitan. Cada cliente recibe una IP por un tiempo limitado (lease time), y esta dirección puede cambiar cada vez que el dispositivo se reconecta a la red. Este método es el único que permite la reutilización dinámica de las IPs, ya que las direcciones se liberan y reasignan una vez que el lease expira. Es ideal para redes con muchos dispositivos que se conectan y desconectan con frecuencia, como redes empresariales o públicas.

**Características**:
- La IP se asigna automáticamente por DHCP con un tiempo de concesión limitado.
- La dirección IP puede cambiar en cada nueva conexión o al expirar el lease.
- Permite la reutilización dinámica de direcciones IP, lo que optimiza el espacio disponible.
- Ideal para redes con un elevado número de dispositivos que no necesitan una IP fija.

[Volver al índice](../README.md)

