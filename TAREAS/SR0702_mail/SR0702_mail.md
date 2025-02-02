# Guía Completa para la Instalación y Configuración de un Servidor de Correo en Linux (Local)

Esta guía proporciona un paso a paso detallado para configurar un servidor de correo local en Linux utilizando Postfix, Dovecot y Roundcube.

## 1. Obtención del Certificado Digital

Para usar versiones seguras de los protocolos HTTP, IMAP y SMTP, es recomendable obtener un certificado digital. Se puede utilizar un certificado **autofirmado** o uno de una entidad certificadora.

### Generar un Certificado Autofirmado con OpenSSL:

```bash
openssl req -newkey rsa:4096 -nodes -sha512 -x509 -days 365 -keyout /etc/ssl/private/ejemplo.local.pem -out /etc/ssl/certs/ejemplo.local.pem
```

## 2. Instalación y Configuración del Servidor SMTP (Postfix)

Instalar Postfix:

```bash
sudo apt update
sudo apt install postfix
```

Configurar Postfix en modo "Internet Site". Editar `/etc/postfix/main.cf` y ajustar:

```bash
myhostname = mailserver1.ejemplo.local
mydomain = ejemplo.local
myorigin = $mydomain
inet_interfaces = all
inet_protocols = ipv4
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
smtpd_tls_cert_file=/etc/ssl/certs/ejemplo.local.pem
smtpd_tls_key_file=/etc/ssl/private/ejemplo.local.pem
home_mailbox = Maildir/
```

Editar `/etc/postfix/master.cf` y descomentar o agregar las siguientes líneas para habilitar el servicio SMTP sobre TLS:

```bash
submission inet n       -       y       -       -       smtpd
-o syslog_name=postfix/submission
-o smtpd_tls_security_level=encrypt
-o smtpd_sasl_auth_enable=yes

smtps     inet  n       -       y       -       -       smtpd
-o syslog_name=postfix/smtps
-o smtpd_tls_wrappermode=yes
-o smtpd_sasl_auth_enable=yes
```

Reiniciar Postfix:

```bash
sudo systemctl restart postfix
```

## 3. Instalación y Configuración del Servidor POP3 e IMAP (Dovecot)

Instalar Dovecot:

```bash
sudo apt install dovecot-core dovecot-pop3d dovecot-imapd
```

Configurar Dovecot en `/etc/dovecot/dovecot.conf`:

```bash
protocols = imap pop3
```

Habilitar SSL en `/etc/dovecot/conf.d/10-ssl.conf`

```bash
ssl = required
ssl_cert = </etc/ssl/certs/ejemplo.local.pem
ssl_key = </etc/ssl/private/ejemplo.local.pem
```

Reiniciar Dovecot:

```bash
sudo systemctl restart dovecot
```

## 4. Creación de Cuentas de Usuario

Crear cuentas de usuario en el sistema:

```bash
sudo adduser usuario1
sudo adduser usuario2
```

Verificar buzones:

```bash
ls /var/mail/
```

## 5. Instalación y Configuración del Servidor Webmail (Roundcube)

Instalar Apache, PHP y MariaDB:

```bash
sudo apt install apache2 php php-mysql mariadb-server
```

Después de instalar MariaDB, es recomendable ejecutar la configuración segura de MySQL para reforzar la seguridad:

```bash
sudo mysql_secure_installation
```

Este proceso permitirá establecer una contraseña para el usuario root, eliminar usuarios anónimos y deshabilitar el acceso remoto del usuario root.

Instalar Roundcube y configurar la base de datos con dbconfig:

```bash
sudo apt install roundcube
```

Durante la instalación, se pedirá configurar la base de datos con dbconfig. Selecciona 'Sí' y proporciona la contraseña de root de MariaDB.

Antes de configurar el VirtualHost de Roundcube, habilita los módulos necesarios en Apache:

```bash
sudo a2enmod alias rewrite ssl
sudo systemctl restart apache2
```

Configurar VirtualHost en `/etc/apache2/sites-available/roundcube.conf`:

```apache
<VirtualHost *:443>
    ServerName webmail.ejemplo.local
    DocumentRoot /usr/share/roundcube
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/ejemplo.local.pem
    SSLCertificateKeyFile /etc/ssl/private/ejemplo.local.pem
</VirtualHost>
```

Activar sitio y reiniciar Apache:

```bash
sudo a2ensite roundcube.conf
sudo systemctl restart apache2
```

## 6. Establecimiento de Parámetros de Conexión en Roundcube

Editar `/etc/roundcube/config.inc.php` y configurar los siguientes parámetros:

```php
$config['default_host'] = 'tls://ejemplo.local';
$config['smtp_user'] = '';
$config['smtp_pass'] = '';
$config['default_port'] = 143;
$config['smtp_server'] = 'tls://ejemplo.local';
$config['smtp_port'] = 587;
$config['imap_conn_options'] = array(
    'ssl' => array(
        'verify_peer' => false,
        'verify_peer_name' => false,
    ),
);
$config['smtp_conn_options'] = array(
    'ssl' => array(
        'verify_peer' => false,
        'verify_peer_name' => false,
    ),
);
```

## 7. Configuración del Firewall y Apertura de Puertos

```bash
sudo ufw allow 25/tcp  # SMTP
sudo ufw allow 110/tcp # POP3
sudo ufw allow 143/tcp # IMAP
sudo ufw allow 443/tcp # HTTPS
sudo ufw reload
```

## 8. Arranque, Parada y Consulta de Estado

```bash
sudo systemctl restart postfix dovecot apache2
sudo systemctl status postfix dovecot apache2
```

## 9. Monitorización y Comprobación de Roundcube

Si `https://webmail.ejemplo.local` no se resuelve correctamente, edita `/etc/hosts` en la máquina cliente:

```bash
sudo nano /etc/hosts
```

Añade:
```
127.0.0.1   webmail.ejemplo.local
```

Abre un navegador y accede a `https://webmail.ejemplo.local`. Si hay problemas, revisa los logs:

```bash
tail -f /var/log/apache2/error.log  # Apache
journalctl -xe | grep roundcube    # Roundcube
```

Con estos pasos, el servidor de correo estará configurado y accesible desde un entorno local.

