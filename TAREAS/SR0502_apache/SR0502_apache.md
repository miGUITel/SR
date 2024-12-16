### Guion para la práctica: Instalación de Apache y configuración de dos sitios virtuales en Ubuntu Server

---

#### 1. **Configuración previa de la máquina virtual**
   - Configura la red en modo puente en la máquina virtual para permitir que obtenga una dirección IP desde el servidor DHCP.
   - Habilita DHCP en Ubuntu Server editando el archivo de configuración de red de *netplan*:
     1. Abre el archivo de configuración:
        ```bash
        sudo nano /etc/netplan/01-netcfg.yaml
        ```
     2. Ajusta el contenido para activar DHCP:
        ```yaml
        network:
          version: 2
          renderer: networkd
          ethernets:
            enp1s0:
              dhcp4: true
        ```
     3. Aplica los cambios:
        ```bash
        sudo netplan apply
        ```
   - Comprueba que la máquina ha obtenido una dirección IP válida:
     ```bash
     ip a
     ping 8.8.8.8
     ```

---

#### 2. **Instalación del servidor web Apache**
   - Instala Apache en Ubuntu Server:
     ```bash
     sudo apt update
     sudo apt install apache2 -y
     ```

---

#### 3. **Comprobación de que se ha instalado el servicio web**
Verifica que Apache dispone de un directorio y está activo:

```bash
ls /etc/apache2
sudo systemctl status apache2
```

Comprueba que la página por defecto es accesible desde el navegador (utiliza la dirección IP de la máquina virtual en el navegador de otra máquina).
  
Explora el archivo de configuración:
    ```
     vim /etc/apache2/apache2.conf
     ```
![alt text](image.png)

---

#### 4. **Creación de directorios y recursos de los sitios web**
   - Crea los directorios para los sitios web:
     ```bash
     sudo mkdir -p /var/www/sitio1 /var/www/sitio2
     ```
   - Crea un archivo `index.html` básico para cada sitio usando un editor de texto como nano o vim:
  
     - **Para el sitio con Acceso autenticado:**

       Inserta el siguiente código HTML:
        ```html
            <!DOCTYPE html>
            <html lang="es">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <meta name="description" content="Sitio con acceso autenticado">
                <meta name="author" content="[Nombre del autor o institución]">
                <title>Bienvenido a nombre del sitio autenticado</title>
            </head>
            <body>
                <h1>Bienvenido a nombre del sitio autenticado</h1>
                <p>Sitio con acceso autenticado https sobre ubuntu</p>
                <p>Alumno: [Nombre]</p>
                <p>Fecha: [Fecha]</p>
            </body>
            </html>
       ```
     - **Para el sitio con acceso anónimo:**
       
       Abre el archivo con un editor de texto, inserta el siguiente código HTML:
        ```html
            <!DOCTYPE html>
            <html lang="es">
            <head>
                <meta charset="UTF-8">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <meta name="description" content="Sitio con acceso anónimo">
                <meta name="author" content="[Nombre del autor o institución]">
                <title>Bienvenido a [nombre del sitio]</title>
            </head>
            <body>
                <h1>Bienvenido a [nombre del sitio]</h1>
                <p>Sitio con acceso anónimo sobre ubuntu</p>
                <p>Alumno: [Nombre]</p>
                <p>Fecha: [Fecha]</p>
            </body>
            </html>
        ```
       

---

#### 5. **Instalación del módulo de autenticación externa**
Instala el módulo necesario para autenticación externa:
     ```
     sudo apt install libapache2-mod-authnz-external pwauth -y
     ```
![alt text](image-1.png)

Activa los módulos de autenticacion básica y externa:
  
```bash
sudo a2enmod auth_basic
sudo a2enmod authnz_external
```

![alt text](image-2.png)

---

#### 6. **Creación de certificado digital autofirmado para conexiones seguras**

   - Instala openssl, aplicación que usarás para generar el certificado:
     ```bash
     sudo apt install openssl
     ```
   - Crea el certificado:
     ```bash
     sudo make-ssl-cert /usr/share/ssl-cert/ssleay.cnf /etc/ssl/private/ejemplo_local
     ```
![alt text](image-3.png)
> ## Cambiar www.ejemplo.local por el nombre de vuestro sitio:
> 
![alt text](image-4.png)

- Activar el módulo de ssl
  ```bash
  sudo a2enmod ssl  
  ```
  ![alt text](image-5.png)
---

#### 7. **Creación de los sitios virtuales (VirtualHosts)**

**Crea** cada sitio **creando** los archivos de configuración para cada sitio virtual:

**Sitio anónimo**
 ```bash
 sudo nano /etc/apache2/sites-available/sitio1.conf
 ```
 Contenido:
 ```bash
<VirtualHost *:80>
    # Configuración del host virtual para el puerto 80 (HTTP)
    
    # Directorio raíz donde se encuentran los archivos del sitio web
    DocumentRoot /var/www/ejemplo_net
    
    # Nombre de dominio asociado a este host virtual (dirección web)
    ServerName www.ejemplo.net
    
    # Correo electrónico del administrador del servidor
    ServerAdmin tic@ejemplo.net
</VirtualHost>

 ```

 [Ampliación ServerName](./ServerNameApache.md)

![alt text](image-7.png)

**Sitio seguro (SSL y autenticación):**
 ```bash
 sudo nano /etc/apache2/sites-available/sitio2.conf
 ```
 Contenido:
 ```apache
<VirtualHost *:443>
    # Configuración del host virtual para el puerto 443 (HTTPS)
    # Directorio donde se alojan los archivos .html y otros
    DocumentRoot /var/www/ejemplo_local
    # Nombre del servidor (direccción web)
    ServerName www.ejemplo.local
    # Email del administrador del servidor
    ServerAdmin tic@ejemplo.local

    # Configuración de autenticación externa con pwauth
    DefineExternalAuth pwauth pipe /usr/sbin/pwauth
    # Directory [el que contenga los archivos]
    <Directory "/var/www/ejemplo_local">
        # Tipo de autenticación (Basic requiere usuario y contraseña)
        AuthType Basic
        # Mensaje mostrado al solicitar credenciales
        AuthName "Sitio de [alumno] - Introduzca usuario y clave"
        # Proveedor de autenticación básica
        AuthBasicProvider external
        # Uso del módulo externo pwauth para la autenticación
        AuthExternal pwauth
        # Requiere que el usuario sea válido
        Require valid-user
    </Directory>

    # Habilitación de SSL
    SSLEngine on
    # Ruta al archivo del certificado SSL
    SSLCertificateFile /etc/ssl/private/ejemplo_local
</VirtualHost>


 ```
 ![alt text](image-6.png)


---

#### 8. **Habilitación y deshabilitación de sitios virtuales**
Puedes consultrar los sitios existentes:

`ls /etc/apache2/sites-availabel`

En el directorio econtrarás los archivos de configuracion de los sitios que has creado y el archivo de configuración del sitio por defecto.

Habilita los sitios creados y desactiva el sitio por defecto:
  ```bash
  sudo a2ensite sitioY.conf sitioX.conf
  sudo a2dissite 000-default.conf

  sudo systemctl reload apache2
  # O BIEN
  sudo systemctl restart apache2

  sudo systemctl status apache2
  ```

  [Diferencia entre restart y reload](./restartVSreload.md)
  
> EN CASO DE ERROR
> `journalctl -xe`
> ![alt text](<Captura de pantalla 2024-12-11 181217.png>)
---

#### 9. **Conexión desde el navegador en una máquina remota**
   - Edita el archivo `hosts` en la máquina cliente para acceder a los sitios virtuales:
     - **En Linux:**
       ```bash
       sudo nano /etc/hosts
       ```
     - **En Windows:**
       Edita el archivo `C:\Windows\System32\drivers\etc\hosts`
     - Añade estas líneas:
       ```
       [IP de la MV] www.ejemplo.local
       [IP de la MV] www.ejemplo.net
       ```

---

#### 10. **Conexión desde un cliente web en modo consola**
   - Instala *lynx*:
     ```bash
     sudo apt install lynx -y
     ```
   - Conéctate a los sitios:
     ```bash
     lynx www.ejemplo.local
     lynx https://www.ejemplo.net
     ```

---

#### 11. **Revisión del log de apache para comprobar las conexiones**

[ampliacion](../../SR05WEB/SR0504_USwebLog.md)

---

