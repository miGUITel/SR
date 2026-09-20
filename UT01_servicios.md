# SERVICIOS EN LA CAPA DE APLICACIÓN

## Tabla de Servicios

| Servicio | Puerto(s) | Utilidad                            | Vigencia               |
|----------|-----------|-------------------------------------|------------------------|
| **DHCP** | 67, 68 (UDP) | Asignación dinámica de IPs         | Vigente                |
| **DNS**  | 53 (UDP/TCP) | Resolución de nombres de dominio   | Vigente                |
| **SNMP** | 161, 162 (UDP) | Monitorización y gestión de redes  | Vigente                |
| **HTTP** | 80 (TCP) | Navegación web (sin cifrado)         | Vigente                |
| **FTP**  | 21 (TCP) | Transferencia de archivos            | Vigente (aunque en desuso) |
| **SMB**  | 445 (TCP) | Compartición de archivos e impresoras en redes Windows | Vigente |
| **RTSP** | 554 (TCP/UDP) | Streaming de medios en tiempo real | Vigente                |
| **SIP**  | 5060 (UDP/TCP), 5061 (TCP con TLS) | Establecimiento y control de sesiones multimedia | Vigente |

---

## Explicación Detallada de los Servicios

### **1. DHCP (Dynamic Host Configuration Protocol)**
- **Puerto**: 67 (servidor) y 68 (cliente) en UDP
- **Utilidad**: Proporciona asignación dinámica de direcciones IP a dispositivos en una red, facilitando la gestión de direcciones.
- **Funcionamiento**: Cuando un dispositivo se conecta a la red, envía una solicitud DHCP Discover. El servidor DHCP responde con una oferta de dirección IP, que el cliente acepta, y luego el servidor confirma la asignación.
- **Vigencia**: Es un servicio clave y ampliamente utilizado en redes modernas, desde pequeñas redes domésticas hasta grandes infraestructuras corporativas.

### **2. DNS (Domain Name System)**
- **Puerto**: 53 (UDP/TCP)
- **Utilidad**: Traduce nombres de dominio legibles por humanos (como `www.google.com`) en direcciones IP numéricas que las máquinas pueden utilizar para comunicarse.
- **Funcionamiento**: Los clientes (dispositivos) envían consultas DNS a un servidor DNS para resolver un nombre de dominio en una dirección IP. Las respuestas se almacenan temporalmente (en caché) para agilizar futuras consultas.
- **Vigencia**: DNS es esencial para el funcionamiento de Internet y sigue siendo ampliamente utilizado en redes privadas y públicas.

### **3. SNMP (Simple Network Management Protocol)**
- **Puerto**: 161 (consulta), 162 (notificaciones/traps) en UDP
- **Utilidad**: SNMP se utiliza para monitorizar y gestionar dispositivos de red como routers, switches, servidores, impresoras, etc.
- **Funcionamiento**: Los agentes SNMP (en dispositivos de red) envían información a los **gestores SNMP** sobre el estado del dispositivo. El gestor también puede enviar consultas para solicitar información o modificar configuraciones.
- **Vigencia**: Sigue siendo ampliamente utilizado para la gestión de redes en entornos empresariales y grandes infraestructuras.

### **4. HTTP (Hypertext Transfer Protocol)**
- **Puerto**: 80 (TCP)
- **Utilidad**: Es el protocolo principal para la navegación web, permitiendo la transferencia de páginas web entre servidores y navegadores.
- **Funcionamiento**: Los navegadores envían solicitudes HTTP a los servidores para recuperar páginas web. La información es transmitida en texto plano sin cifrado, lo que ha llevado a la preferencia por **HTTPS** (que incluye cifrado).
- **Vigencia**: Aunque sigue vigente, cada vez más sitios web migran a **HTTPS** por razones de seguridad.

### **5. FTP (File Transfer Protocol)**
- **Puerto**: 21 (TCP)
- **Utilidad**: Se utiliza para la transferencia de archivos entre un cliente y un servidor a través de una red.
- **Funcionamiento**: FTP establece una conexión de control en el puerto 21 y luego crea una conexión de datos para la transferencia de archivos. Funciona en modo activo o pasivo, según cómo se manejen las conexiones.
- **Vigencia**: Aunque es funcional, FTP ha sido reemplazado en muchas aplicaciones por **SFTP** o **FTPS**, que añaden cifrado para mayor seguridad.

### **6. SMB (Server Message Block)**
- **Puerto**: 445 (TCP)
- **Utilidad**: SMB permite el intercambio de archivos, impresoras y otros recursos entre dispositivos en una red, especialmente en entornos Windows.
- **Funcionamiento**: Permite a los usuarios compartir archivos e impresoras de manera transparente en una red local. SMB es la base de las redes de Windows para compartir recursos.
- **Vigencia**: Sigue siendo ampliamente utilizado en redes Windows y con protocolos como **CIFS** (Common Internet File System).

### **7. RTSP (Real-Time Streaming Protocol)**
- **Puerto**: 554 (TCP/UDP)
- **Utilidad**: Se utiliza para controlar la transmisión de medios en tiempo real, como video o audio en streaming.
- **Funcionamiento**: RTSP controla la transmisión de medios en tiempo real, permitiendo la selección de canales, la pausa, y la reanudación de la transmisión de video o audio. Este protocolo es utilizado principalmente en servidores de streaming.
- **Vigencia**: Sigue siendo un protocolo relevante para aplicaciones multimedia y streaming.

### **8. SIP (Session Initiation Protocol)**
- **Puerto**: 5060 mediante UDP o TCP para SIP sin TLS; 5061 mediante TCP para SIP protegido con TLS.
- **Utilidad**: SIP es utilizado para el establecimiento, control y finalización de sesiones multimedia, como llamadas de voz y video a través de VoIP.
- **Funcionamiento**: SIP establece, gestiona y termina sesiones multimedia (por ejemplo, llamadas VoIP). Interactúa con otros protocolos para transportar el contenido real (como **RTP** para audio y video).
- **Vigencia**: Sigue siendo un protocolo clave en sistemas de VoIP, videoconferencias y comunicaciones multimedia.

---
