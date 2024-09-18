La notación **CIDR** (Classless Inter-Domain Routing) en IPv6 es un método de representar el rango de direcciones IP mediante una combinación de la dirección y la longitud del prefijo (o máscara de subred). Funciona de manera similar a como lo hace en IPv4, pero con direcciones de 128 bits en lugar de 32 bits.

### ¿Qué significa exactamente la notación CIDR en IPv6?

En la notación **CIDR**, una dirección IPv6 va seguida de una barra inclinada (`/`) y un número, que representa la cantidad de bits que corresponden al **prefijo de red**. El número después de la barra indica cuántos de los primeros bits de la dirección son fijos y representan la red, mientras que los bits restantes pueden variar y se usan para identificar interfaces (o hosts) dentro de esa red.

#### Ejemplo básico:
**2001:0db8:85a3::/64**

- La dirección `2001:0db8:85a3::` es una dirección de red IPv6.
- El sufijo `/64` indica que los primeros 64 bits son el **prefijo de red** y los últimos 64 bits se utilizan para identificar los hosts o interfaces dentro de esa red.

### Cómo interpretar la notación CIDR

- **2001:0db8:85a3::/64**: Aquí los primeros 64 bits (`2001:0db8:85a3`) son el identificador de red, y los últimos 64 bits (en este caso, `::` se usa como abreviatura para los ceros) pueden cambiarse para crear nuevas direcciones dentro de esa red.
  
- **2001:0db8::/32**: En este caso, los primeros 32 bits (`2001:0db8`) representan la red, y los 96 bits restantes se pueden utilizar para direccionar hosts. Esto significa que hay más espacio disponible para dividir en subredes o para asignar direcciones a hosts.

### Ejemplos de notación CIDR en IPv6:

1. **2001:0db8::/48**:
   - Los primeros 48 bits (`2001:0db8`) son el prefijo de red.
   - Los siguientes 80 bits (128 - 48) se utilizan para identificar hosts o subredes dentro de esa red.
   - Este es un prefijo común para asignaciones de redes globales a grandes organizaciones que luego dividen en subredes más pequeñas.

2. **fe80::/10**:
   - Es el prefijo reservado para direcciones de enlace local (Link-local), donde los primeros 10 bits (`fe80`) representan la red.
   - Las direcciones en esta red se utilizan para comunicaciones locales en un segmento de red y no son enrutables fuera de esa red local.

3. **fc00::/7**:
   - Prefijo utilizado para direcciones de redes locales únicas (ULA, Unique Local Address). Estas direcciones son similares a las direcciones privadas en IPv4 (como 192.168.x.x) y se usan para redes privadas.

4. **2001:db8:abcd:0012::/64**:
   - Aquí los primeros 64 bits (`2001:db8:abcd:0012`) representan la red, y los 64 bits restantes están disponibles para identificar hosts o interfaces dentro de esa subred. Esta sería una típica configuración de red local.

### Cómo afecta el tamaño del prefijo (sufijo CIDR)

El tamaño del prefijo determina el número de direcciones que están disponibles dentro de la red:

- **/128**: Solo se refiere a una única dirección IPv6. Se usa para especificar una dirección específica de un dispositivo.
- **/64**: Es el prefijo más común para redes locales en IPv6. Permite una cantidad masiva de direcciones para hosts (2^64 direcciones posibles).
- **/48**: Se usa generalmente para organizaciones que quieren subdividir su red en múltiples subredes más pequeñas.
- **/32**: Generalmente es utilizado por los Proveedores de Servicios de Internet (ISPs) para asignar grandes bloques de direcciones a sus clientes.

### Resumen:

La notación **CIDR** en IPv6 es esencial para la planificación y segmentación de redes. Indica cómo se dividen las direcciones entre el prefijo de red y el identificador de host, permitiendo asignar bloques de direcciones de manera más flexible y eficiente.