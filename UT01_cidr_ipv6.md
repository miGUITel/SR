La notación **CIDR** (Classless Inter-Domain Routing) en IPv6 es un método de representar el rango de direcciones IP mediante una combinación de la dirección y la longitud del prefijo (o máscara de subred). Funciona de manera similar a como lo hace en IPv4, pero con direcciones de 128 bits en lugar de 32 bits.

### ¿Qué significa exactamente la notación CIDR en IPv6?

En la notación **CIDR**, una dirección IPv6 va seguida de una barra inclinada (`/`) y un número, que representa la cantidad de bits que corresponden al **prefijo de red**. El número después de la barra indica cuántos de los primeros bits de la dirección son fijos y representan la red, mientras que los bits restantes pueden variar y se usan para identificar interfaces (o hosts) dentro de esa red.

#### Ejemplo básico:
**2001:0db8:85a3:0000::/64**

- La dirección `2001:0db8:85a3:0000::` es una dirección de red IPv6.
- El sufijo `/64` indica que los primeros 64 bits son el **prefijo de red** y los últimos 64 bits se utilizan para identificar los hosts o interfaces dentro de esa red.

### Cómo interpretar la notación CIDR

- **2001:0db8:85a3:0000::/64**: Los primeros 64 bits (`2001:0db8:85a3:0000`) son el identificador de red y los últimos 64 bits pueden cambiarse para crear direcciones dentro de esa red.
  
- **2001:db8::/32**: Los primeros 32 bits representan el prefijo y los 96 bits restantes quedan disponibles para crear ejemplos de subredes y direcciones. Este bloque está reservado para documentación y no debe utilizarse como direccionamiento real en Internet.

### Ejemplos de notación CIDR en IPv6:

1. **2001:db8:abcd::/48**:
   - Los primeros 48 bits (`2001:db8:abcd`) son el prefijo de red.
   - Los siguientes 80 bits pueden utilizarse para crear subredes e identificadores de interfaz en un ejemplo de documentación.

2. **fe80::/10**:
   - Es el bloque reservado para direcciones de enlace local (*link-local*).
   - Estas direcciones se utilizan en el enlace local y no se encaminan hacia otros enlaces.
   - En una interfaz suele mostrarse una longitud de prefijo `/64`. El `/10` identifica el bloque reservado y el `/64` indica el prefijo utilizado en ese enlace concreto.

3. **fc00::/7**:
   - Prefijo utilizado para direcciones de redes locales únicas (ULA, Unique Local Address). Estas direcciones son similares a las direcciones privadas en IPv4 (como 192.168.x.x) y se usan para redes privadas.

4. **2001:db8:abcd:0012::/64**:
   - Aquí los primeros 64 bits (`2001:db8:abcd:0012`) representan la red, y los 64 bits restantes están disponibles para identificar hosts o interfaces dentro de esa subred. Esta sería una típica configuración de red local.
   - Como pertenece a `2001:db8::/32`, se trata de un ejemplo de documentación, no de una dirección para utilizar en Internet.

### Cómo afecta el tamaño del prefijo (sufijo CIDR)

El tamaño del prefijo determina el número de direcciones que están disponibles dentro de la red:

- **/128**: Solo se refiere a una única dirección IPv6. Se usa para especificar una dirección específica de un dispositivo.
- **/64**: Es el prefijo más común para redes locales en IPv6. Permite una cantidad masiva de direcciones para hosts (2^64 direcciones posibles).
- **/48**: Se usa generalmente para organizaciones que quieren subdividir su red en múltiples subredes más pequeñas.
- **/32**: Puede utilizarse en asignaciones amplias, por ejemplo para que un proveedor subdivida el bloque en prefijos menores.

### Resumen:

La notación **CIDR** en IPv6 es esencial para la planificación y segmentación de redes. Indica cómo se dividen las direcciones entre el prefijo de red y el identificador de host, permitiendo asignar bloques de direcciones de manera más flexible y eficiente.
