El **DHCP** (Dynamic Host Configuration Protocol) es un protocolo de red que permite a los dispositivos obtener automáticamente una configuración de red, como una dirección IP, máscara de subred, puerta de enlace predeterminada y servidores DNS, sin necesidad de que un administrador de red lo haga manualmente. Su función principal es facilitar la conexión de dispositivos a una red al proporcionarles configuraciones de manera dinámica y centralizada.

### Funcionamiento teórico del DHCP:

El proceso de obtención de una dirección IP y otros parámetros de red a través de DHCP sigue un ciclo de comunicación de cuatro pasos entre el cliente (el dispositivo que solicita la configuración) y el servidor DHCP (el dispositivo que provee las configuraciones).

#### 1. **Descubrimiento (DHCPDISCOVER):**
   - **Descripción:** Cuando un cliente se conecta a la red y no tiene una dirección IP asignada, envía un mensaje **DHCPDISCOVER** en broadcast (a toda la red) para descubrir si hay algún servidor DHCP disponible. Este mensaje se envía a la dirección IP de broadcast (normalmente 255.255.255.255) ya que el cliente aún no tiene una IP asignada.
   - **Objetivo:** Informar a cualquier servidor DHCP en la red que el cliente está solicitando una configuración IP.

#### 2. **Oferta (DHCPOFFER):**
   - **Descripción:** Uno o más servidores DHCP en la red que recibieron el mensaje de descubrimiento responden con un mensaje **DHCPOFFER**, que contiene una oferta de una dirección IP para el cliente, junto con otra información de red, como la máscara de subred, la puerta de enlace y el tiempo de arrendamiento de la dirección IP.
   - **Objetivo:** Ofrecer una dirección IP válida al cliente junto con los parámetros necesarios para que el cliente pueda operar en la red.

#### 3. **Solicitud (DHCPREQUEST):**
   - **Descripción:** Una vez que el cliente recibe una o varias ofertas, selecciona una de ellas (generalmente la primera que recibe) y responde con un mensaje **DHCPREQUEST** en broadcast. Este mensaje es una confirmación de que el cliente acepta la oferta de ese servidor DHCP en particular y solicita formalmente la dirección IP proporcionada.
   - **Objetivo:** Confirmar al servidor que el cliente ha aceptado su oferta.

#### 4. **Confirmación (DHCPACK):**
   - **Descripción:** El servidor DHCP responde con un mensaje **DHCPACK** (Acknowledgement), confirmando que la dirección IP ha sido asignada al cliente, y enviando los parámetros adicionales de configuración como DNS, duración del arrendamiento (lease), y puerta de enlace.
   - **Objetivo:** Finalizar el proceso de configuración, permitiendo al cliente usar la dirección IP y otros parámetros de red.

### Diagrama del proceso:

```text
Cliente                  Servidor DHCP
   |                            |
   |--DHCPDISCOVER------------->|
   |                            |
   |<--DHCPOFFER----------------|
   |                            |
   |--DHCPREQUEST-------------->|
   |                            |
   |<--DHCPACK------------------|
   |                            |
   |                            |
   |   (T1)                     |
   |                            |
   |--DHCPREQUEST-unicast------>|
   |                            |
   |<--¿DHCPACK?----------------|
   |                            |
   |   (T2)                     |
   |                            |
   |--DHCPREQUEST-broadcast---->|
   |                            |
   |<--¿DHCPACK?----------------|
   |                            |
   |   (LT)                     |   
   |                            |
   |--DHCPDISCOVER------------->|
   |                            |
   |                            |
   |                            |
   |--DHCPRELEASE-------------->|
   |                            |
```

<p><a href="https://commons.wikimedia.org/wiki/File:DHCP_-_A_new_Address_Allocating_Sequence_Diagram-en(TYPO_FIXED).svg#/media/File:DHCP_-_A_new_Address_Allocating_Sequence_Diagram-en(TYPO_FIXED).svg"><img src="https://upload.wikimedia.org/wikipedia/commons/1/14/DHCP_-_A_new_Address_Allocating_Sequence_Diagram-en%28TYPO_FIXED%29.svg" alt="File:DHCP - A new Address Allocating Sequence Diagram-en(TYPO FIXED).svg" height="567" width="509"></a><br>De <a href="//commons.wikimedia.org/w/index.php?title=User:Catdotjs&amp;action=edit&amp;redlink=1" class="new" title="User:Catdotjs (page does not exist)">Catdotjs</a> - <span class="int-own-work" lang="es">Trabajo propio</span>, <a href="https://creativecommons.org/licenses/by-sa/4.0" title="Creative Commons Attribution-Share Alike 4.0">CC BY-SA 4.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=145547002">Enlace</a></p>

### Otros conceptos importantes en DHCP:

1. **Duración del arrendamiento (Lease Time):**
   - La dirección IP asignada por el servidor DHCP no es permanente. Tiene un tiempo de arrendamiento, que indica cuánto tiempo el cliente puede usar la dirección IP antes de que tenga que renovarla. Al terminar el tiempo de arrendamiento, el cliente debe solicitar una renovación.

2. **Renovación del arrendamiento (DHCPRENEW):**
   - Antes de que expire el tiempo de arrendamiento, el cliente puede enviar una solicitud de renovación (mensaje DHCPREQUEST) al servidor DHCP para continuar usando la misma dirección IP.

3. **Liberación y terminación (DHCPRELEASE):**
   - Cuando un cliente ya no necesita la dirección IP, puede enviar un mensaje **DHCPRELEASE** al servidor, informando que ya no usará la dirección IP asignada. El servidor puede entonces reasignar esa dirección a otro cliente.

4. **Reasignación de direcciones:**
   - El servidor DHCP mantiene un "pool" o conjunto de direcciones IP disponibles, y asigna las direcciones en función de su disponibilidad. Si un cliente deja de usar una dirección IP, esa dirección puede volver al pool para ser asignada a otro dispositivo.

### Ventajas del DHCP:

- **Automatización:** El DHCP elimina la necesidad de configurar manualmente direcciones IP en cada dispositivo, lo que es especialmente útil en redes grandes.
- **Eficiencia:** Permite reutilizar direcciones IP y administrar mejor los recursos de red, evitando conflictos de IP.
- **Flexibilidad:** Facilita el ingreso y salida de dispositivos a la red, ya que las configuraciones son dinámicas y temporales.

### DHCP Relay

El **DHCP Relay** es una función que permite a los clientes DHCP, que están en una red diferente a la del servidor DHCP, obtener direcciones IP y otros parámetros de configuración. Esto es especialmente útil en redes grandes o segmentadas donde un único servidor DHCP debe servir a múltiples subredes.

Cuando un cliente envía una solicitud de DHCP en una subred que no tiene un servidor DHCP local, el dispositivo que actúa como DHCP Relay (generalmente un router o switch) intercepta esta solicitud. El DHCP Relay entonces reenvía la solicitud al servidor DHCP ubicado en otra subred. El servidor DHCP responde a esta solicitud a través del DHCP Relay, que finalmente envía la respuesta al cliente.

El DHCP Relay usa el protocolo **DHCP Relay Agent Information Option (opción 82)**, que permite identificar de qué subred proviene la solicitud, facilitando la asignación de direcciones IP adecuadas para cada segmento de red. En resumen, el DHCP Relay permite la comunicación entre clientes y servidores DHCP en diferentes redes, optimizando la administración centralizada de direcciones IP y otras configuraciones.

<p><a href="https://commons.wikimedia.org/wiki/File:DHCP_Relay.svg#/media/%D5%8A%D5%A1%D5%BF%D5%AF%D5%A5%D6%80:DHCP_Relay.svg"><img src="https://upload.wikimedia.org/wikipedia/commons/f/fb/DHCP_Relay.svg" alt="DHCP Relay.svg" height="5851" width="7938"></a><br>By <a href="//commons.wikimedia.org/w/index.php?title=User:Gmelander&amp;action=edit&amp;redlink=1" class="new" title="User:Gmelander (page does not exist)">Gmelander</a> - <span class="int-own-work" lang="hy">Բեռնողի սեփական աշխատանք</span>, <a href="https://creativecommons.org/licenses/by-sa/4.0" title="Creative Commons Attribution-Share Alike 4.0">CC BY-SA 4.0</a>, <a href="https://commons.wikimedia.org/w/index.php?curid=52484989">Link</a></p>


### Conclusión:

El DHCP es esencial para la gestión eficiente de redes modernas, especialmente en entornos con un gran número de dispositivos, como oficinas, escuelas o redes públicas. Al automatizar la asignación de configuraciones de red, permite una administración centralizada y reduce los errores humanos que pueden ocurrir al configurar direcciones manualmente.

