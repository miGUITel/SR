- [**SR0401a Práctica: Instalación y prueba básica de un servidor FTP en Windows Server 2019**](#sr0401a-práctica-instalación-y-prueba-básica-de-un-servidor-ftp-en-windows-server-2019)
  - [**1. Instalación del Servidor FTP usando PowerShell**](#1-instalación-del-servidor-ftp-usando-powershell)
  - [**2. Crear el Sitio FTP a través de IIS**](#2-crear-el-sitio-ftp-a-través-de-iis)
  - [**3. Configurar Acceso de Usuario**](#3-configurar-acceso-de-usuario)
  - [**4. Conectar al Servidor FTP desde el CMD del propio servidor**](#4-conectar-al-servidor-ftp-desde-el-cmd-del-propio-servidor)
  - [💻 **Comandos FTP básicos en CMD**](#-comandos-ftp-básicos-en-cmd)

# **SR0401a Práctica: Instalación y prueba básica de un servidor FTP en Windows Server 2019**

**Objetivo:**
Instalar y configurar un servidor FTP básico en Windows Server 2019, crear un sitio con acceso de usuario y comprobar su funcionamiento desde el propio servidor.

> 💡 **Importante:**
> Durante la instalación, el servidor necesita **conexión a Internet** para descargar los componentes de IIS y FTP.
> Asegúrate de que la máquina virtual esté configurada en **modo adaptador en puente** (para tener conexión a Internet).
>
> Una vez finalizada la instalación, configura una **IP fija** y cambia a **red interna** para realizar las pruebas de conexión FTP.

---

## **1. Instalación del Servidor FTP usando PowerShell**

* Abrir **PowerShell como administrador**.
* Ejecutar los siguientes comandos para instalar IIS y el servicio FTP:

```
Install-WindowsFeature -name Web-FTP-Server -IncludeManagementTools
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

* Verificar que la instalación se ha completado correctamente.

![alt text](image-5.png)

---

## **2. Crear el Sitio FTP a través de IIS**

* Crear una carpeta local llamada, por ejemplo, **FTP_LOCAL**, que servirá de directorio raíz.

* Abrir el **Administrador de IIS** y agregar un **nuevo sitio FTP**:

![alt text](image-11.png)

---

![alt text](image-12.png)

* Asignar un nombre al sitio (por ejemplo, `ftp_local`) y seleccionar la carpeta **FTP_LOCAL** como ruta física.
* Configurar el sitio **sin SSL** (en esta práctica no lo usaremos).
* Puedes marcar o no `Habilitar nombres de host virtuales`, por ahora no tiene trascendencia, en prácticas más avanzadas sí.
* Finalizar la creación del sitio.

![alt text](image-13.png)

---

## **3. Configurar Acceso de Usuario**

![alt text](image-14.png)

* En el panel del sitio FTP:

  * Seleccionar **“Autenticación Básica”** → **Habilitar**.
  * Abrir **“Reglas de autorización FTP”** → **Agregar regla de permiso**.
  * Permitir **Lectura y escritura** para el usuario con el que has iniciado sesión en el servidor.

![alt text](image-15.png)

---

## **4. Conectar al Servidor FTP desde el CMD del propio servidor**

* Abrir el **Símbolo del sistema (CMD)**.
* Conectarse al servidor FTP (puede ser `localhost` o la IP del servidor):

```
ftp localhost
```

* Introducir las credenciales del usuario con el que estás conectado al sistema.
* Una vez dentro, probar los comandos básicos:

```
ls
mkdir prueba
quit
```

![alt text](image-21.png)

Si se crea correctamente el directorio `prueba`, la conexión y permisos funcionan.

* Prueba otros:

## 💻 **Comandos FTP básicos en CMD**

| Comando            | Descripción                                                 |
| ------------------ | ----------------------------------------------------------- |
| `open <ip>`        | Conecta con el servidor FTP (por ejemplo, `open localhost`) |
| `user <nombre>`    | Inicia sesión con el usuario indicado                       |
| `ls` / `dir`       | Lista los archivos del directorio actual                    |
| `cd <carpeta>`     | Cambia de carpeta en el servidor                            |
| `get <archivo>`    | Descarga un archivo del servidor                            |
| `put <archivo>`    | Sube un archivo al servidor                                 |
| `delete <archivo>` | Elimina un archivo del servidor                             |
| `quit`             | Cierra la sesión y sale del cliente FTP                     |
