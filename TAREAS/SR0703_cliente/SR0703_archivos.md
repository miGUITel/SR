Aquí tienes un **listado completo** de los servicios utilizados en la guía de instalación, tanto en el **servidor** como en el **cliente**, junto con los **archivos de configuración** y una descripción de lo que se configura en cada uno.

---

# **🔹 Servicios en el Servidor**
Estos servicios son esenciales para el funcionamiento del servidor de correo **Postfix + Dovecot**.

| Servicio | Función | Archivo(s) de configuración | Descripción de lo que se configura |
|----------|--------|----------------------------|------------------------------------|
| **Postfix** | MTA (Mail Transfer Agent), gestiona el envío y la recepción de correos | `/etc/postfix/main.cf`  | Configura los dominios, interfaces de red, política de reenvío (relay), y autenticación SMTP |
| | | `/etc/postfix/master.cf` | Define qué servicios (SMTP, Submission, etc.) están activos y en qué puertos |
| **Dovecot** | MDA (Mail Delivery Agent) e IMAP/POP3 Server | `/etc/dovecot/dovecot.conf` | Configura qué interfaces escucha y los módulos activos de Dovecot |
| | | `/etc/dovecot/conf.d/10-mail.conf` | Define la ubicación del buzón de correo (Maildir o mbox) |
| | | `/etc/dovecot/conf.d/10-master.conf` | Configura los puertos de escucha para IMAP y POP3 |
| **UFW** *(Firewall)* | Permite o bloquea tráfico de red | `/etc/ufw/ufw.conf` (opcional) | Se usa para abrir los puertos 25 (SMTP), 143 (IMAP) y 587 (SMTP Submission) |
| **DNS o `/etc/hosts`** | Traducción de nombres a IPs | `/etc/hosts` (si no hay DNS) | Se usa para definir nombres de dominio locales para los clientes |

---

# **🔹 Servicios en el Cliente**
Estos servicios son necesarios para que los clientes puedan enviar y recibir correos desde el servidor.

| Servicio | Función | Archivo(s) de configuración | Descripción de lo que se configura |
|----------|--------|----------------------------|------------------------------------|
| **Mutt** | Cliente de correo en terminal | `~/.muttrc` | Configura el acceso IMAP al servidor de correo, usuario, buzón, y el método de envío de correos |
| **msmtp** | MTA ligero usado por Mutt para enviar correos | `~/.msmtprc` | Define el servidor SMTP, puerto, usuario y autenticación (si aplica) |
| **DNS o `/etc/hosts`** | Traducción de nombres a IPs | `/etc/hosts` (si no hay DNS) | Permite que el cliente resuelva el nombre del servidor de correo en la red |

---

Sí, en el servidor también utilizamos **`mail`** (el comando de línea de comandos para enviar y leer correos). **`mail`** en Linux es un **MUA (Mail User Agent) muy básico**, que permite leer y enviar correos localmente sin necesidad de un cliente IMAP como Mutt.

---

## **📌 ¿Tiene archivos de configuración?**
Depende de la versión que esté instalada. En sistemas basados en Debian/Ubuntu, el comando `mail` suele ser parte del paquete **`bsd-mailx`** o **`mailutils`**, y puede depender de Postfix o Exim.

- Si usaste `mail` desde la terminal en el servidor, **estaba utilizando Postfix internamente para enviar correos**.
- No tiene archivos de configuración propios, pero usa:
  - **Postfix (`/etc/postfix/main.cf`)** para gestionar el envío de correos.
  - **Dovecot (`~/Maildir/`)** para almacenar los correos en buzones locales.

---

## **📌 Archivos de configuración relacionados con `mail`**
Si `mail` estaba funcionando en el servidor, aquí están los archivos que pueden afectar su comportamiento:

| Archivo | Ubicación | Configuración |
|---------|----------|--------------|
| `/etc/mail.rc` | **(Opcional, si está presente)** | Archivo de configuración global de `mail`, permite definir alias, servidores externos, etc. |
| `~/.mailrc` | **(Opcional, por usuario)** | Archivo de configuración local del usuario para `mail` |
| `/etc/postfix/main.cf` | **Servidor** | Configura cómo se envían los correos cuando usas `mail` |
| `~/Maildir/` | **Servidor** | Si Dovecot está configurado con Maildir, `mail` puede acceder a los correos locales aquí |
| `/var/mail/usuario` | **Servidor** | Si el sistema usa `mbox` en lugar de `Maildir`, los correos locales pueden estar aquí |

---

## **📌 ¿Cómo verificamos dónde almacena los correos?**
Si `mail` está funcionando en el servidor, podemos verificar si está usando **Maildir** o **mbox**:

```bash
echo $MAIL
```
Si devuelve algo como:

```
/var/mail/usuario
```
→ Significa que está usando **mbox** (almacenamiento tradicional en un solo archivo por usuario).

Si `MAIL` no está definido, pero `~/Maildir/` existe y contiene correos, entonces **está usando Maildir**.

Para listar los correos almacenados:

```bash
ls -l ~/Maildir/new/
```
o si usa `mbox`:
```bash
ls -l /var/mail/
```
---

# **📌 Resumen de archivos de configuración clave**

| Archivo | Ubicación | Configuración |
|---------|----------|--------------|
| `/etc/postfix/main.cf` | **Servidor** | Configura Postfix: dominios, políticas de reenvío, interfaces de red |
| `/etc/postfix/master.cf` | **Servidor** | Activa/desactiva servicios de Postfix como SMTP, Submission, Relay |
| `/etc/dovecot/dovecot.conf` | **Servidor** | Habilita IMAP/POP3 y define módulos en Dovecot |
| `/etc/dovecot/conf.d/10-mail.conf` | **Servidor** | Especifica la ubicación de los buzones de correo (Maildir o mbox) |
| `/etc/dovecot/conf.d/10-master.conf` | **Servidor** | Configura qué puertos usa Dovecot (143 IMAP, 110 POP3, etc.) |
| `/etc/ufw/ufw.conf` | **Servidor** (opcional) | Configura reglas de firewall (abrir puertos 25, 143, 587) |
| `/etc/hosts` | **Servidor/Cliente** (si no hay DNS) | Traduce nombres de dominio a IPs en la red local |
| `~/.muttrc` | **Cliente** | Configura acceso IMAP y carpetas de correo en Mutt |
| `~/.msmtprc` | **Cliente** | Define el servidor SMTP para el envío de correos |

---
