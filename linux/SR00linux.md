### Guion Básico  de  Linux
- [Guion Básico de Linux](#guion-básico-de-linux)
  - [Cambiar el nombre del equipo host](#Cambiar-el-nombre-del-equipo)
  - [Esquema de directorios ampliación](#esquema-de-directorios-ampliación)
  - [Creación de usuarios y contraseñas ampliación](#creación-de-usuarios-y-contraseñas-ampliación)
  - [Creación de grupos ampliación](#creación-de-grupos-ampliación)
  - [Gestión de usuarios y grupos](#gestión-de-usuarios-y-grupos)
  - [Configuración de red con netplan ampliación](#configuración-de-red-con-netplan-ampliación)
  - [Uso de nano y vim](#uso-de-nano-y-vim)
  - [Comprobación de conectividad](#comprobación-de-conectividad)
  - [Qué es un repositorio](#qué-es-un-repositorio)
  - [Instalación de componentes (apt)](#instalación-de-componentes-apt)
  - [Inicio, parada, reinicio de componentes: systemctl](#inicio-parada-reinicio-de-componentes-systemctl)

#### Cambiar el nombre del equipo editando archivos manualmente

1. **Editar el archivo `/etc/hostname`**  
   Abre el archivo y reemplaza el nombre del host por el nuevo:
   ```bash
   sudo nano /etc/hostname
   ```
   Ejemplo:
   ```
   antiguo-nombre
   ```
   Cambiar a:
   ```
   nuevo-nombre
   ```

2. **Editar el archivo `/etc/hosts`**  
   Abre el archivo y asegúrate de que el nuevo nombre esté asociado a `127.0.1.1`:
   ```bash
   sudo nano /etc/hosts
   ```
   Ejemplo del archivo:
   ```
   127.0.0.1   localhost
   127.0.1.1   antiguo-nombre
   ```
   Cambiar a:
   ```
   127.0.0.1   localhost
   127.0.1.1   nuevo-nombre
   ```

3. **Reiniciar para aplicar los cambios**  
   Aunque no siempre es necesario, reiniciar el sistema asegura que todos los servicios reconozcan el nuevo nombre:
   ```bash
   sudo reboot
   ```

#### Esquema de directorios [ampliación](./SR001directorios.md)
- **/**: Directorio raíz. Contiene todos los directorios del sistema.
- **/home**: Directorio de los usuarios.
- **/var**: Contiene archivos variables, como logs o datos de aplicaciones.
- **/etc**: Archivos de configuración del sistema.
- **/usr**: Programas y librerías instalados para los usuarios.
- **/bin y /sbin**: Binarios esenciales para usuarios y administradores, respectivamente.
- **/tmp**: Archivos temporales.
- **/dev**: Dispositivos del sistema.
- **/proc y /sys**: Información del sistema y procesos.

---

#### Creación de usuarios y contraseñas [ampliación](./SR002usuarios.md)
```bash
sudo adduser nombre_usuario
```
- Asigna una contraseña durante el proceso:
```bash
sudo passwd nombre_usuario
```

---

#### Creación de grupos [ampliación](./SR003grupos.md)
```bash
sudo groupadd nombre_grupo
```
- Añadir un usuario a un grupo:
```bash
sudo usermod -aG nombre_grupo nombre_usuario
```

---

#### Gestión de usuarios y grupos
- **Eliminar usuario:**
```bash
sudo deluser nombre_usuario
```
- **Eliminar grupo:**
```bash
sudo groupdel nombre_grupo
```
- **Listar grupos de un usuario:**
```bash
groups nombre_usuario
```

---

#### Configuración de red con netplan [ampliación](./SR005redNetPlan.md)
1. Editar el archivo de configuración que econtrarás detro de **/etc/netplan/**:
   - Para IP fija: (**recuerda cambiar enp1s0 por el nombre de tu interfaz de red**)
```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```
   - Para DHCP:
```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      dhcp4: true
```
2. Probar la configuración:
```bash
sudo netplan try
```

*error gateway4 deprecated* [explicación](./SR005bDeprecated.md)

3. Aplicar la configuración:
```bash
sudo netplan apply
```

---

#### Uso de nano y vim
- **Nano**:
  - Abrir archivo: `nano archivo`
  - Guardar: `Ctrl + O` y salir: `Ctrl + X`.
- **Vim**:
  - Abrir archivo: `vim archivo`
  - Modo inserción: `i`, guardar y salir: `Esc` + `:wq`.

---

#### Comprobación de conectividad
- Mostrar IP:
```bash
ip a
```
- Comprobar conectividad:
```bash
ping 8.8.8.8
```
- Ver tabla de rutas:
```bash
ip route
```

---

#### Qué es un repositorio
Un repositorio es un conjunto de paquetes de software que se pueden instalar desde un servidor remoto.
- Archivo de configuración: **/etc/apt/sources.list**.

---

#### Instalación de componentes (apt)
- Actualizar lista de paquetes:
```bash
sudo apt update
```
- Instalar un paquete:
```bash
sudo apt install nombre_paquete
```
- Eliminar un paquete:
```bash
sudo apt remove nombre_paquete
```
- Actualizar el sistema:
```bash
sudo apt upgrade
```

---

#### Inicio, parada, reinicio de componentes: systemctl
- **Iniciar un servicio:**
```bash
sudo systemctl start nombre_servicio
```
- **Detener un servicio:**
```bash
sudo systemctl stop nombre_servicio
```
- **Reiniciar un servicio:**
```bash
sudo systemctl restart nombre_servicio
```
- **Habilitar al inicio:**
```bash
sudo systemctl enable nombre_servicio
```
- **Deshabilitar al inicio:**
```bash
sudo systemctl disable nombre_servicio
```
- **Estado de un servicio:**
```bash
sudo systemctl status nombre_servicio
```

