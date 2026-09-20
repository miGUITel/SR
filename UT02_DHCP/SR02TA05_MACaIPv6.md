# Convertir una dirección MAC en un identificador EUI-64

### Introducción a EUI-64
El formato EUI-64 (*Extended Unique Identifier*) permite generar un **identificador de interfaz** de 64 bits a partir de una dirección MAC de 48 bits. Se estudia para comprender cómo puede formarse una dirección IPv6, pero los sistemas actuales suelen utilizar también identificadores estables o temporales que no revelan directamente la dirección MAC. Por tanto, no debe suponerse que toda dirección IPv6 se deriva de una MAC.

### Pasos para la conversión de MAC a IPv6 con EUI-64

1. **Obtener la dirección MAC de 48 bits**.
   Una dirección MAC tiene 48 bits y está generalmente representada en formato hexadecimal, con dos puntos o guiones separando los bytes. Por ejemplo:

   ```
   MAC: 00:1A:2B:3C:4D:5E
   ```

2. **Dividir la dirección MAC en dos partes**.
   La dirección MAC se divide en dos mitades de 24 bits cada una:

   ```
   00:1A:2B y 3C:4D:5E
   ```

3. **Insertar `FFFE` en el medio**.
   Se debe insertar `FFFE` en medio de las dos partes de la dirección MAC para extenderla de 48 bits a 64 bits:

   ```
   00:1A:2B:FF:FE:3C:4D:5E
   ```

4. **Invertir el séptimo bit (el bit universal/local - U/L)**.
   El séptimo bit de la dirección MAC, llamado el bit U/L (universal/local), indica si la dirección es única globalmente o si es localmente administrada. Para generar el identificador EUI-64, este bit se invierte.

   - Si es 0, lo cambiamos a 1.
   - Si es 1, lo cambiamos a 0.

   En el caso de la dirección MAC `00:1A:2B:3C:4D:5E`, el primer byte en binario es `00000000`. El séptimo bit está en 0, por lo que lo cambiamos a 1. Esto nos da un nuevo primer byte:

   ```
   02:1A:2B:FF:FE:3C:4D:5E
   ```

5. **Formar la dirección IPv6**.
   Ahora que tenemos el identificador de interfaz de 64 bits, se puede usar para formar una dirección IPv6 de enlace local o una dirección unicast global.

   Si estamos generando una dirección de enlace local, la dirección pertenecerá al bloque **`fe80::/10`**. En las interfaces IPv6 se utiliza normalmente una longitud de prefijo `/64`; el identificador EUI-64 ocupa los 64 bits finales.

   ```
   FE80::021A:2BFF:FE3C:4D5E
   ```

### Ejemplo completo

Supongamos que tenemos la dirección MAC `00:1A:2B:3C:4D:5E`. Siguiendo los pasos:

1. **Dirección MAC**: `00:1A:2B:3C:4D:5E`
2. **Dividir en dos partes**: `00:1A:2B` y `3C:4D:5E`
3. **Insertar FFFE**: `00:1A:2B:FF:FE:3C:4D:5E`
4. **Invertir el séptimo bit**: `02:1A:2B:FF:FE:3C:4D:5E`
5. **Formar dirección IPv6**: `FE80::021A:2BFF:FE3C:4D5E`

### Verificación de IPv6 en un equipo

Una vez configurada la dirección IPv6 en una interfaz de red, puedes verificarlo utilizando el comando `ip` en Linux:

```bash
ip addr show
```

Esto te mostrará todas las direcciones IPv6 asignadas a las interfaces, incluyendo las de enlace local (link-local).

### Conclusión
EUI-64 permite entender un procedimiento de formación del identificador de interfaz. Sin embargo, no es el único método utilizado actualmente y no debe esperarse que todas las direcciones IPv6 contengan la MAC del dispositivo.

### Ampliación

En redes IPv6, existen diferentes tipos de direcciones para diversos propósitos. A continuación, te explico dos de los tipos más comunes: **direcciones de enlace local** y **direcciones unicast globales**.

### 1. **Direcciones IPv6 de Enlace Local (Link-Local)**
#### ¿Qué son?
Las direcciones **IPv6 de enlace local** son direcciones que solo son válidas dentro de un segmento o enlace de red local. Estas direcciones permiten que los dispositivos de una misma red física se comuniquen entre sí sin necesidad de un router o de una configuración manual de direcciones IP.

#### Características:
- **Bloque reservado:** `fe80::/10` identifica las direcciones de enlace local. En la configuración de una interfaz suele aparecer `/64`, que es la longitud del prefijo utilizado en ese enlace; no sustituye al `/10` que define el tipo de dirección.
- **Alcance limitado:** Estas direcciones no son enrutables fuera del enlace local. Es decir, no pueden ser utilizadas para enviar tráfico a través de la red más allá de la red local en la que se encuentran.
- **Asignación automática:** Los sistemas operativos modernos generan automáticamente una dirección de enlace local en cada interfaz de red habilitada para IPv6, lo que permite que los dispositivos en la misma red se comuniquen sin necesidad de un servidor DHCP o de una configuración manual.

#### Ejemplo de una dirección de enlace local:
```
fe80::1a2b:3c4d:5e6f:7a8b
```
Esta dirección solo es válida dentro de la red local donde el dispositivo está conectado.

#### Uso típico:
- **Comunicación entre dispositivos locales:** Las direcciones de enlace local se utilizan principalmente para la comunicación entre dispositivos en la misma red local sin pasar por un router.
- **Protocolo de resolución de direcciones:** Las direcciones de enlace local son clave en la resolución de direcciones mediante **Neighbor Discovery Protocol (NDP)**, que sustituye el ARP en IPv6.

---

### 2. **Direcciones IPv6 Unicast Globales**
#### ¿Qué son?
Las direcciones **IPv6 unicast globales** son equivalentes a las direcciones IP públicas en IPv4. Son direcciones únicas y globalmente enrutables en toda la red de Internet IPv6. Estas direcciones permiten que los dispositivos puedan comunicarse a través de la red global de Internet.

#### Características:
- **Prefijo:** Las direcciones unicast globales suelen tener el prefijo `2000::/3`, lo que significa que las direcciones comienzan con los bits `001` en binario, es decir, cualquier dirección IPv6 que empiece con `2` o `3` probablemente sea una dirección unicast global.
- **Enrutamiento global:** Estas direcciones pueden ser utilizadas para la comunicación a través de diferentes redes, incluso en Internet.
- **Asignación:** Las direcciones unicast globales pueden ser asignadas manualmente o mediante mecanismos de autoconfiguración como SLAAC (Stateless Address Autoconfiguration), que permite que los dispositivos obtengan automáticamente una dirección de un router, o mediante DHCPv6.

#### Ejemplo de una dirección unicast global:
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
```
Esta dirección pertenece a `2001:db8::/32`, un bloque reservado para documentación. Tiene formato de dirección unicast global, pero no debe utilizarse como dirección real en Internet.

#### Uso típico:
- **Comunicación a través de Internet:** Las direcciones unicast globales se usan para identificar dispositivos que necesitan comunicarse a través de Internet, como servidores, routers y dispositivos de usuarios.
- **Acceso a recursos públicos:** Para que un servidor web o cualquier otro servicio en Internet sea accesible globalmente, deberá tener una dirección unicast global.

---

### Diferencias Clave

| **Característica**                | **Dirección Link-Local (Enlace Local)**   | **Dirección Unicast Global**           |
|-----------------------------------|-------------------------------------------|----------------------------------------|
| **Prefijo**                       | `FE80::/10`                               | `2000::/3`                             |
| **Enrutamiento**                  | No es enrutada fuera del enlace local     | Globalmente enrutada en Internet       |
| **Asignación**                    | Automática                                | Asignación automática o manual         |
| **Alcance**                       | Solo dentro de la red local               | Global, accesible en cualquier parte del mundo |
| **Uso principal**                 | Comunicación local (sin routers)          | Comunicación entre redes, Internet     |

### Resumen:
- **Direcciones IPv6 de Enlace Local**: Sirven para comunicación entre dispositivos dentro de una misma red local y no pueden ser usadas para comunicación a través de Internet. Se generan automáticamente y tienen el prefijo `FE80::/10`.
- **Direcciones IPv6 Unicast Globales**: Son direcciones globalmente enrutables, equivalentes a las direcciones públicas de IPv4, y permiten la comunicación entre dispositivos a través de redes, incluyendo Internet. Tienen el prefijo `2000::/3`.
