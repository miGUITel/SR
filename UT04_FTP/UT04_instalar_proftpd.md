## Guía para instalar y configurar **ProFTPD (proftp-basic)** en Linux:

### 1. **Actualizar los repositorios**
Lo primero es asegurarse de que el sistema esté actualizado. Abre una terminal y ejecuta:

```bash
sudo apt update && sudo apt upgrade -y
```

### 2. **Instalar ProFTPD**
ProFTPD está disponible en los repositorios oficiales de la mayoría de distribuciones basadas en Debian/Ubuntu. Para instalar la versión básica, ejecuta:

```bash
sudo apt install proftpd-basic
```

Te preguntará si deseas instalarlo como un **daemon independiente** o si prefieres usarlo a través de **inetd**. Generalmente, se elige la opción **Standalone** para mayor control y flexibilidad:

1. Selecciona "Standalone" usando las flechas.
2. Presiona Enter.

### 3. **Configurar ProFTPD**

El archivo de configuración principal de ProFTPD se encuentra en `/etc/proftpd/proftpd.conf`.

Para editarlo, abre el archivo con tu editor favorito:

```bash
sudo nano /etc/proftpd/proftpd.conf
```

Algunas configuraciones clave que puedes ajustar son:

#### a. **Permitir conexiones de usuarios locales**

Para permitir que los usuarios locales del sistema se conecten al servidor FTP, asegúrate de que la siguiente línea esté habilitada (sin el carácter `#` delante):

```bash
DefaultRoot ~
```

Esto asegura que los usuarios estén "enjaulados" en su directorio principal y no puedan acceder a otros directorios del sistema.

#### b. **Configurar el puerto del servidor**

Por defecto, ProFTPD escucha en el puerto 21. Si deseas cambiarlo, edita esta línea:

```bash
Port 21
```

Puedes establecer otro puerto si lo deseas, por ejemplo, el 2121:

```bash
Port 2121
```

#### c. **Habilitar FTP sobre TLS (opcional)**

Si deseas asegurar las conexiones FTP usando TLS, puedes añadir o modificar estas líneas:

```bash
<IfModule mod_tls.c>
  TLSEngine             on
  TLSLog                /var/log/proftpd/tls.log
  TLSProtocol           SSLv23
  TLSRSACertificateFile /etc/ssl/certs/proftpd.crt
  TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
  TLSOptions            NoCertRequest
  TLSVerifyClient       off
  TLSRequired           on
</IfModule>
```

Esto permite conexiones FTP seguras mediante SSL/TLS.

Para crear los certificados, puedes usar:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/proftpd.key -out /etc/ssl/certs/proftpd.crt
```

### 4. **Reiniciar el servicio ProFTPD**

Cada vez que modifiques la configuración, es necesario reiniciar el servicio para que los cambios tengan efecto:

```bash
sudo systemctl restart proftpd
```

### 5. **Habilitar el servicio para que se inicie automáticamente**

Si deseas que el servicio se inicie automáticamente al arrancar el sistema, ejecuta:

```bash
sudo systemctl enable proftpd
```

### 6. **Comprobar el estado del servicio**

Para verificar si ProFTPD está funcionando correctamente, ejecuta:

```bash
sudo systemctl status proftpd
```

### 7. **Prueba la conexión FTP**

Puedes probar la conexión usando un cliente FTP como **FileZilla** o la línea de comandos:

```bash
ftp localhost
```

### 8. **Ajustes adicionales**

Si necesitas hacer más configuraciones específicas como añadir usuarios virtuales o ajustar permisos, puedes hacerlo en el archivo de configuración principal o añadiendo más módulos.

### Consideraciones de seguridad:
- Asegúrate de que el firewall permita conexiones FTP, si tienes uno activado.
- Considera utilizar FTP sobre TLS si deseas asegurar la transferencia de datos.
- Verifica que las configuraciones de permisos de usuario estén adecuadamente configuradas para evitar accesos no autorizados.

[Configura el usuario anónimo](./UT04_configurar_anonimo.md)