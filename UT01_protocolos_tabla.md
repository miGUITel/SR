# Tabla resumen de los protocolos más importantes

| Protocolo | Capa OSI                 | Palabras clave                      | Descripción                                                                                   |
|-----------|--------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------|
| ARP       | Capa 2 (Enlace de datos) | Resolución de direcciones, IP a MAC | Traduce direcciones IP a direcciones MAC en una red local.                                     |
| RARP      | Capa 2 (Enlace de datos) | Resolución inversa, MAC a IP        | Permite que un dispositivo sin IP obtenga su dirección IP a partir de su dirección MAC.        |
| ICMP      | Capa 3 (Red)             | Control, Diagnóstico                | Envía mensajes de error y control para diagnosticar problemas de red (p. ej., ping).           |
| TCP       | Capa 4 (Transporte)      | Conexión fiable, Control de flujo   | Protocolo orientado a conexión que garantiza la entrega fiable de los datos.                   |
| UDP       | Capa 4 (Transporte)      | Sin conexión, Velocidad             | Protocolo no orientado a conexión que permite el envío rápido de datos sin garantizar la entrega. |
| SCTP      | Capa 4 (Transporte)      | Multihoming, Flujos múltiples       | Similar a TCP, pero permite múltiples flujos de datos simultáneos y mayor resistencia a fallos. |

## Descripción básica de estos protocolos

A continuación te doy una descripción más detallada de cada uno de los 6 protocolos:

### 1. **ARP (Address Resolution Protocol)**
- **Capa OSI**: Capa 2 (Enlace de datos)
- **Palabras clave**: "Resolución de direcciones", "IP a MAC"
- **Descripción detallada**:  
   ARP es utilizado en redes IPv4 para asociar una dirección IP (que funciona a nivel lógico) con una dirección MAC (que opera a nivel físico). Cuando un dispositivo quiere comunicarse con otro en una red local, necesita conocer la dirección MAC del destinatario. ARP envía una solicitud a todos los dispositivos de la red local (broadcast) preguntando "¿Quién tiene esta IP?" y el dispositivo con esa IP responde con su dirección MAC. Este protocolo es fundamental en redes Ethernet y otros entornos de red local.

### 2. **RARP (Reverse Address Resolution Protocol)**
- **Capa OSI**: Capa 2 (Enlace de datos)
- **Palabras clave**: "Resolución inversa", "MAC a IP"
- **Descripción detallada**:  
   RARP es utilizado para resolver la dirección IP de un dispositivo que solo conoce su propia dirección MAC. Es el proceso inverso a ARP. Dispositivos como estaciones de trabajo sin disco (diskless workstations) o dispositivos sin almacenamiento de configuración pueden utilizar RARP para solicitar su dirección IP al arrancar. Aunque RARP ha sido reemplazado por protocolos más modernos como BOOTP y DHCP, sigue siendo una pieza clave en la evolución de las redes.

### 3. **ICMP (Internet Control Message Protocol)**
- **Capa OSI**: Capa 3 (Red)
- **Palabras clave**: "Control", "Diagnóstico"
- **Descripción detallada**:  
   ICMP es responsable de enviar mensajes de control y error entre dispositivos de red. Este protocolo no transporta datos de usuario, sino que gestiona el estado y la información de la red. Uno de los ejemplos más conocidos de ICMP es el comando `ping`, que envía un mensaje de eco para verificar si un dispositivo está accesible y mide el tiempo de respuesta. ICMP también se utiliza para informar sobre problemas como la imposibilidad de entregar un paquete o la red inalcanzable.

### 4. **TCP (Transmission Control Protocol)**
- **Capa OSI**: Capa 4 (Transporte)
- **Palabras clave**: "Conexión fiable", "Control de flujo"
- **Descripción detallada**:  
   TCP es un protocolo de transporte orientado a la conexión que garantiza la entrega fiable de los datos entre dos dispositivos en red. Utiliza un proceso de establecimiento de conexión (conocido como "handshake" de tres vías) para iniciar una sesión de comunicación. Una vez establecida, TCP segmenta los datos, garantiza su entrega en el orden correcto y maneja el control de flujo, asegurando que el emisor no envíe datos más rápido de lo que el receptor puede procesar. Si algún segmento se pierde o se corrompe, TCP se encarga de retransmitirlo. Esto hace que sea ideal para aplicaciones donde la precisión es crucial, como el correo electrónico y la transferencia de archivos.

### 5. **UDP (User Datagram Protocol)**
- **Capa OSI**: Capa 4 (Transporte)
- **Palabras clave**: "Sin conexión", "Velocidad"
- **Descripción detallada**:  
   A diferencia de TCP, UDP es un protocolo sin conexión que no garantiza la entrega, el orden ni la integridad de los datos. Se utiliza cuando la velocidad es más importante que la fiabilidad, como en la transmisión de video y audio en tiempo real, donde es preferible perder algunos paquetes antes que retrasar la transmisión. UDP envía los datos sin establecer previamente una conexión con el receptor, lo que lo hace más rápido pero también menos fiable. No tiene mecanismos de corrección de errores, lo que significa que si un paquete se pierde, el protocolo no lo retransmitirá.

### 6. **SCTP (Stream Control Transmission Protocol)**
- **Capa OSI**: Capa 4 (Transporte)
- **Palabras clave**: "Multihoming", "Flujos múltiples"
- **Descripción detallada**:  
   SCTP es un protocolo más moderno que combina características de TCP y UDP. Similar a TCP, es un protocolo orientado a conexión y garantiza la entrega fiable de los datos, pero mejora su funcionalidad permitiendo múltiples flujos de datos dentro de una misma conexión. Esto significa que los segmentos de datos se pueden enviar de forma independiente sin bloquear otros flujos si hay retrasos en uno de ellos. Además, SCTP soporta multihoming, lo que permite que un dispositivo tenga múltiples direcciones IP activas en una misma conexión, ofreciendo mayor resiliencia en caso de fallos de red. SCTP se utiliza en aplicaciones que requieren una comunicación fiable pero también flexibilidad, como la señalización en redes de telecomunicaciones.

