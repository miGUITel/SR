- [CONECTAR USB A MÁQUINA VIRTUAL](#conectar-usb-a-máquina-virtual)
  - [ANFITRIÓN LINUX](#anfitrión-linux)
    - [1. **Verificar que el host tenga acceso al disco duro USB:**](#1-verificar-que-el-host-tenga-acceso-al-disco-duro-usb)
    - [2. **Instalar el paquete de “VirtualBox Extension Pack” (si aún no lo has hecho):**](#2-instalar-el-paquete-de-virtualbox-extension-pack-si-aún-no-lo-has-hecho)
    - [3. **Agregar el usuario al grupo `vboxusers`:**](#3-agregar-el-usuario-al-grupo-vboxusers)
    - [4. **Configurar el dispositivo USB en VirtualBox:**](#4-configurar-el-dispositivo-usb-en-virtualbox)
    - [5. **Iniciar la máquina virtual:**](#5-iniciar-la-máquina-virtual)
    - [Notas adicionales:](#notas-adicionales)
  - [ANFITRIÓN WINDOWS](#anfitrión-windows)
    - [1. **Verificar que Windows (el anfitrión) detecte el disco duro USB:**](#1-verificar-que-windows-el-anfitrión-detecte-el-disco-duro-usb)
    - [2. **Instalar el "VirtualBox Extension Pack" (si no lo has hecho):**](#2-instalar-el-virtualbox-extension-pack-si-no-lo-has-hecho)
    - [3. **Configurar USB en la máquina virtual:**](#3-configurar-usb-en-la-máquina-virtual)
    - [4. **Iniciar la máquina virtual:**](#4-iniciar-la-máquina-virtual)
    - [5. **Verificar el dispositivo en la máquina virtual:**](#5-verificar-el-dispositivo-en-la-máquina-virtual)
    - [Notas adicionales:](#notas-adicionales-1)

# CONECTAR USB A MÁQUINA VIRTUAL

## ANFITRIÓN LINUX

Para que tu máquina virtual en VirtualBox acceda a un disco duro USB conectado al host, debes seguir estos pasos:

### 1. **Verificar que el host tenga acceso al disco duro USB:**
   - Asegúrate de que tu sistema operativo anfitrión pueda ver y acceder al disco duro USB. Si no lo ve, verifica la conexión física o la configuración del dispositivo.

### 2. **Instalar el paquete de “VirtualBox Extension Pack” (si aún no lo has hecho):**
   VirtualBox requiere este paquete para habilitar el soporte para dispositivos USB 2.0 y 3.0. Puedes instalarlo desde la [página de descargas de VirtualBox](https://www.virtualbox.org/wiki/Downloads).

   - Descarga la versión correspondiente de “VirtualBox Extension Pack” según tu versión de VirtualBox.
   - Instálalo abriendo VirtualBox, yendo a `Archivo` > `Preferencias` > `Extensiones`, y haciendo clic en el ícono de `+` para agregar la extensión descargada.

### 3. **Agregar el usuario al grupo `vboxusers`:**
   En sistemas basados en Linux, como Ubuntu o Linux Mint, asegúrate de que el usuario que está ejecutando VirtualBox esté en el grupo `vboxusers`. Si no es así, puedes agregarlo ejecutando este comando en la terminal:

   ```bash
   sudo usermod -aG vboxusers $USER
   ```

   Luego, cierra sesión o reinicia para que los cambios surtan efecto.

### 4. **Configurar el dispositivo USB en VirtualBox:**
   1. Abre VirtualBox y selecciona la máquina virtual donde deseas agregar el disco duro USB.
   2. Haz clic en `Configuración`.
   3. En el menú de la izquierda, selecciona `USB`.
   4. Marca la casilla para habilitar el `Controlador USB 2.0 (EHCI)` o `Controlador USB 3.0 (xHCI)` según el tipo de dispositivo USB que tienes.
   5. Haz clic en el ícono del `+` (agregar dispositivo USB) y selecciona el disco duro USB de la lista de dispositivos disponibles.

### 5. **Iniciar la máquina virtual:**
   - Inicia la máquina virtual y debería detectar el disco duro USB conectado. El sistema operativo invitado (por ejemplo, Windows o Linux) debería poder reconocerlo automáticamente. Si es necesario, monta el dispositivo manualmente dentro del sistema operativo de la máquina virtual.

### Notas adicionales:
- Si estás usando un sistema operativo anfitrión Linux y el disco duro USB está montado en el host, es posible que tengas que desmontarlo (umount) en el host antes de poder asignarlo a la máquina virtual.
- Asegúrate de tener los drivers correspondientes en la máquina virtual para que pueda reconocer el dispositivo USB.

Con estos pasos, deberías poder acceder al disco duro USB desde la máquina virtual en VirtualBox.

## ANFITRIÓN WINDOWS

Si el anfitrión es Windows, los pasos son bastante similares, pero con algunos ajustes específicos para este sistema operativo. A continuación te explico cómo proceder en ese caso:

### 1. **Verificar que Windows (el anfitrión) detecte el disco duro USB:**
   - Asegúrate de que el disco duro USB esté conectado y visible en el sistema operativo anfitrión (Windows). Puedes comprobar esto en el `Explorador de archivos` o en `Administración de discos`.

### 2. **Instalar el "VirtualBox Extension Pack" (si no lo has hecho):**
   Esto es esencial para habilitar la compatibilidad con dispositivos USB 2.0 y 3.0. Debes descargarlo desde la [página oficial de VirtualBox](https://www.virtualbox.org/wiki/Downloads).

   - Una vez descargado, abre VirtualBox y ve a `Archivo` > `Preferencias` > `Extensiones`.
   - Haz clic en el ícono `+` y selecciona el archivo del “Extension Pack” que descargaste para instalarlo.

### 3. **Configurar USB en la máquina virtual:**
   - Abre VirtualBox y selecciona la máquina virtual donde deseas conectar el disco duro USB.
   - Haz clic en el botón `Configuración`.
   - En el menú de la izquierda, selecciona la pestaña `USB`.
   - Habilita el `Controlador USB 2.0 (EHCI)` o `Controlador USB 3.0 (xHCI)`, dependiendo del tipo de USB que uses (2.0 o 3.0).
   - Haz clic en el ícono de `+` (agregar dispositivo USB) y selecciona el disco duro USB de la lista de dispositivos conectados.

### 4. **Iniciar la máquina virtual:**
   - Inicia la máquina virtual.
   - Si todo está configurado correctamente, el sistema operativo invitado debería detectar automáticamente el disco duro USB. 

### 5. **Verificar el dispositivo en la máquina virtual:**
   - Si estás usando Windows como sistema operativo en la máquina virtual, deberías poder ver el disco en el `Explorador de archivos`.
   - Si estás usando un sistema operativo Linux en la máquina virtual, puedes ejecutar el siguiente comando para ver si el sistema detecta el disco USB:

     ```bash
     lsblk
     ```
     Esto te mostrará una lista de dispositivos de almacenamiento conectados.

### Notas adicionales:
- **Desmontar el disco en Windows (opcional):** Si Windows anfitrión está utilizando el disco USB, puede que tengas que "quitarlo de manera segura" del anfitrión antes de asignarlo a la máquina virtual. Para hacer esto:
   - Haz clic en el ícono de `Expulsar dispositivo` en la barra de tareas de Windows.
   - Desmonta el disco USB, pero no lo desconectes físicamente. Esto permitirá que la máquina virtual lo capture.
  
- **Permisos de administrador:** Asegúrate de que estás ejecutando VirtualBox con permisos de administrador en el anfitrión (Windows), ya que puede necesitar privilegios para acceder a dispositivos USB.

Con estos pasos, deberías poder acceder al disco duro USB desde tu máquina virtual en VirtualBox en un anfitrión Windows.