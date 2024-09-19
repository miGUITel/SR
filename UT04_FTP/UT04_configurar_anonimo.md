Para permitir que los usuarios anónimos se conecten a tu servidor FTP usando ProFTPD, sigue estos pasos:

### 1. **Editar el archivo de configuración**

Abre el archivo de configuración de ProFTPD:

```bash
sudo nano /etc/proftpd/proftpd.conf
```

### 2. **Habilitar el acceso anónimo**

Dentro de la configuración, busca la sección donde se menciona la configuración de usuarios anónimos o agrega la siguiente configuración al final del archivo:

```bash
<Anonymous ~ftp>
  User                          ftp
  Group                         nogroup
  # Permitir solo descargas, no subir ni borrar archivos
  UserAlias                      anonymous ftp
  RequireValidShell off
  <Directory *>
    <Limit WRITE>
      DenyAll
    </Limit>
  </Directory>
  
  # Limitar el ancho de banda del usuario anónimo
  MaxClients 10
</Anonymous>
```

### Descripción de las opciones:

- **User y Group**: Se define que el usuario anónimo es el sistema `ftp` y el grupo `nogroup`, que es una configuración común para estos casos.
- **UserAlias**: Alias que permite a los usuarios conectarse utilizando `anonymous` o `ftp` como nombre de usuario.
- **Limit WRITE**: Con esta opción, los usuarios anónimos pueden **solo descargar** archivos, pero no pueden subir ni borrar archivos. Si quieres permitir subir archivos, puedes ajustar esta configuración más adelante.
- **MaxClients**: Número máximo de clientes anónimos que pueden conectarse al mismo tiempo.
- **DisplayLogin y DisplayFirstChdir**: Muestra un mensaje de bienvenida y un mensaje en el primer cambio de directorio. Estos archivos (`welcome.msg` y `.message`) deben existir en el directorio raíz FTP para mostrar el mensaje.

### 3. **Crear un directorio FTP para el usuario anónimo (si no existe)**

Necesitas crear el directorio donde los usuarios anónimos tendrán acceso. Por defecto, este directorio es `/srv/ftp`, pero puedes ajustarlo según tus necesidades.

1. Crea el directorio FTP:

   ```bash
   sudo mkdir /srv/ftp
   ```

2. Asigna los permisos adecuados para que los usuarios anónimos puedan acceder pero no puedan escribir:

   ```bash
   sudo chown ftp:nogroup /srv/ftp
   sudo chmod 755 /srv/ftp
   ```

Esto da permisos de lectura y ejecución (navegación) al directorio, pero evita que se puedan modificar los archivos.

### 4. **Permitir que los usuarios anónimos descarguen archivos**

Para permitir que los usuarios anónimos descarguen archivos, coloca los archivos en el directorio `/srv/ftp` o en el que hayas configurado.

```bash
sudo cp archivo.txt /srv/ftp/
```

Asegúrate de que los archivos tengan los permisos correctos para permitir su lectura:

```bash
sudo chmod 644 /srv/ftp/archivo.txt
```

### 5. **(Opcional y peligroso!!) Permitir que los usuarios anónimos suban archivos**

Si deseas permitir que los usuarios anónimos suban archivos, modifica el bloque `<Directory *>` en la configuración anónima:

```bash
<Directory incoming>
  <Limit READ WRITE>
    AllowAll
  </Limit>
  <Limit STOR>
    AllowAll
  </Limit>
</Directory>
```

Esto permite que los usuarios suban archivos al directorio `incoming` dentro del directorio FTP. Luego, crea el directorio `incoming` y asigna permisos de escritura:

```bash
sudo mkdir /srv/ftp/incoming
sudo chown ftp:nogroup /srv/ftp/incoming
sudo chmod 733 /srv/ftp/incoming
```

Con esto, los usuarios pueden subir archivos, pero no pueden sobrescribir ni borrar los archivos que ya están allí.

### 6. **Reiniciar ProFTPD**

Después de realizar estos cambios, reinicia el servicio de ProFTPD para que los cambios tengan efecto:

```bash
sudo systemctl restart proftpd
```

### 7. **Probar la conexión anónima**

Puedes probar la conexión FTP anónima utilizando un cliente FTP, como la línea de comandos o FileZilla. Conéctate a tu servidor y utiliza "anonymous" como nombre de usuario y cualquier correo electrónico como contraseña (a menudo no es verificado).

```bash
ftp localhost
```

Al conectarte, si todo está correctamente configurado, deberías ver un mensaje de bienvenida y acceso al directorio FTP.

---

Con estos pasos, los usuarios anónimos pueden conectarse a tu servidor FTP y descargar (o subir si lo configuras) archivos según los permisos definidos.