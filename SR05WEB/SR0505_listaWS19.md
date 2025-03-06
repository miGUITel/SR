# Pasos para crear un sitio web en IIS en WS19

1. **Instalar IIS**:
   - Abre el Administrador del Servidor.
   - Haz clic en "Agregar roles y características".
   - Selecciona "Instalación basada en características o en roles".
   - Selecciona el servidor donde deseas instalar IIS.
   - En la sección "Roles de servidor", selecciona "Servidor web (IIS)".
   - Completa el asistente para instalar IIS.

2. **Configurar un nuevo sitio web**:
   - Abre el Administrador de IIS.
   - En el panel de conexiones, haz clic derecho en "Sitios" y selecciona "Agregar sitio web".
   - Introduce un nombre para el sitio web.
   - Establece la ruta física del sitio web (la carpeta donde se encuentran los archivos del sitio).
   - Configura el enlace (binding) del sitio web, incluyendo el tipo, la dirección IP y el puerto.
   - Haz clic en "Aceptar" para crear el sitio web.

3. **Configurar permisos y autenticación**:
   - Selecciona el nuevo sitio web en el Administrador de IIS.
   - En el panel central, haz clic en "Autenticación".
   - Habilita y configura los métodos de autenticación necesarios (por ejemplo, autenticación anónima, autenticación básica, etc.).
   - Configura los permisos de la carpeta del sitio web para asegurarte de que el usuario de IIS tenga acceso.

4. **Probar el sitio web**:
   - Añade una línea con la dirección IP `127.0.0.1` seguida del nombre del sitio web (por ejemplo, `127.0.0.1 misitio.local`).
   - Guarda los cambios y cierra el editor de texto.
   - Abre un navegador web.
   - Introduce la URL del nuevo sitio web (por ejemplo, `http://localhost` o la dirección IP del servidor).
   - Verifica que el sitio web se carga correctamente.
   - Modifica el archivo de hosts para añadir la dirección del sitio dirigida a 127.0.0.1:
   - Abre el archivo `C:\Windows\System32\drivers\etc\hosts` con un editor de texto **en modo administrador**.


5. **Configurar características adicionales (opcional)**:
   - Configura SSL si es necesario.
   - Configura reglas de redireccionamiento o reescritura de URL.
   - Configura límites de ancho de banda y conexiones.

6. **Monitorear y mantener el sitio web**:
   - Utiliza las herramientas de monitoreo de IIS para revisar el rendimiento y los registros del sitio web.
   - Realiza copias de seguridad regulares de los archivos del sitio web y la configuración de IIS.
