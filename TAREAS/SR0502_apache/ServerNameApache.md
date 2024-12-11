¡Buena observación! La directiva **`ServerName`** en el archivo de configuración de un host virtual tiene un propósito fundamental en cómo Apache maneja las solicitudes, especialmente en entornos con múltiples sitios o dominios. Aquí te explico su importancia:

---

### **Importancia de `ServerName`**
1. **Identificación del host virtual correcto**:
   - Cuando Apache recibe una solicitud, utiliza el valor de `ServerName` (y `ServerAlias`, si está configurado) para determinar **a qué host virtual debe dirigir la solicitud**.
   - Esto es esencial en entornos con múltiples hosts virtuales (VirtualHosts) en el mismo servidor, un enfoque conocido como **Virtual Hosting**.
   - Ejemplo:
     - Si tienes configurados dos VirtualHosts:
       ```apache
       <VirtualHost *:80>
           ServerName www.ejemplo1.net
           DocumentRoot /var/www/ejemplo1
       </VirtualHost>

       <VirtualHost *:80>
           ServerName www.ejemplo2.net
           DocumentRoot /var/www/ejemplo2
       </VirtualHost>
       ```
       Apache dirigirá una solicitud a `www.ejemplo1.net` al directorio `/var/www/ejemplo1`, y las de `www.ejemplo2.net` a `/var/www/ejemplo2`. Esto es gracias a que el navegador incluye el nombre de dominio en el encabezado **Host** de cada solicitud HTTP.

2. **Resolución de conflictos**:
   - Si no se configura un `ServerName` o si no coincide con el encabezado de la solicitud HTTP, Apache puede enviar la solicitud al primer host virtual configurado o al **default host**. Esto puede causar resultados inesperados.
   - Por eso, definir un `ServerName` explícito asegura que Apache maneje las solicitudes correctamente.

3. **Mensajes y redirecciones**:
   - Algunas páginas de error generadas por Apache (como las de mantenimiento) pueden incluir el valor de `ServerName` para mostrar al usuario el dominio del sitio.
   - También se usa para redirecciones automáticas (por ejemplo, si configuras un redireccionamiento de HTTP a HTTPS).

---

### **¿Por qué no funciona si uso directamente la IP?**
Cuando accedes al servidor usando la IP, el navegador no envía un nombre de dominio en el encabezado **Host** de la solicitud HTTP. Si no hay un VirtualHost explícitamente configurado para manejar solicitudes a esa IP, Apache puede:

1. Enviar la solicitud al **default host** (el primer VirtualHost definido en la configuración).
2. Mostrar un error si no hay un VirtualHost que pueda manejar la solicitud.

---

### **¿Cómo acceder con el nombre de dominio (sin usar la IP)?**
Como bien dices, para usar el dominio configurado en `ServerName` necesitas un mecanismo para que el navegador asocie el dominio con la IP del servidor. Puedes hacerlo de dos maneras:

1. **Usar un servidor DNS**:
   - Configuras un registro DNS que apunte el dominio `www.ejemplo.net` a la IP del servidor.
   - Esto es lo habitual en entornos de producción, donde los usuarios acceden al sitio a través de Internet.

2. **Modificar el archivo `hosts` del sistema operativo** (para pruebas locales):
   - Añades una entrada al archivo `hosts` para resolver el dominio localmente. Por ejemplo:
     - En Linux/Unix/Windows: Edita el archivo `hosts` (ubicado en `/etc/hosts` en Linux y `C:\Windows\System32\drivers\etc\hosts` en Windows) y añade:
       ```
       192.168.1.100 www.ejemplo.net
       ```
   - Esto obliga al sistema a asociar el dominio con la IP directamente, sin usar DNS.

---

### **¿Y si no configuro `ServerName`?**
- Si no configuras un `ServerName`, Apache utiliza el primer VirtualHost definido como predeterminado para todas las solicitudes. Esto puede provocar problemas en servidores con múltiples sitios, ya que todos los dominios o IPs resolverán al mismo VirtualHost.

---

### **Conclusión**:
- El dominio en `ServerName` es crucial para que Apache maneje correctamente las solicitudes HTTP, sobre todo en servidores con varios sitios.
- Aunque durante el desarrollo accedas con una IP y puerto, definir `ServerName` es buena práctica, ya que es clave en entornos de producción o pruebas con nombres de dominio.
- Para usar el dominio en un navegador, necesitas un DNS o una entrada en el archivo `hosts` para resolver el dominio a la IP del servidor.