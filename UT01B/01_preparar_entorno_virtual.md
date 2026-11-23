# Sesión 1. Preparar el entorno virtual

El objetivo es disponer de máquinas madre comprobadas y de clones enlazados sobre los que trabajar. No configures todavía las direcciones del escenario.

## 1. Preparar las máquinas madre

Para cada sistema proporcionado:

1. Crea o registra la máquina virtual utilizando el disco VDI indicado.
2. Asigna un nombre que permita reconocer el sistema y su condición de máquina madre.
3. Comprueba memoria, almacenamiento y adaptadores antes de arrancar.
4. Inicia la máquina y verifica que el sistema operativo carga correctamente.
5. Comprueba que puedes iniciar sesión.
6. Apágala desde el sistema operativo.

La máquina madre debe mantenerse como base limpia. Las prácticas se realizarán sobre clones.

## 2. Crear un clon enlazado

Con la máquina apagada:

1. Selecciona la máquina madre.
2. Inicia la operación de clonación.
3. Elige **clon enlazado** cuando aparezca esa opción.![alt text](image-1.png)
4. Solicita que VirtualBox genere nuevas direcciones MAC para los adaptadores del clon.
5. Asigna al clon un nombre que lo identifique como máquina de trabajo.
6. Inicia el clon y comprueba que funciona.

Un clon enlazado ocupa menos espacio porque depende de los discos de la máquina madre. No muevas ni elimines la máquina madre ni sus discos mientras utilices el clon.

![Una máquina madre de la que dependen dos clones enlazados](./img/clones_enlazados.png)

### Primer arranque de un clon de Ubuntu Server

**Realiza estos pasos una sola vez, desde la consola de VirtualBox del clon Ubuntu recién creado.** Después del bloque, reinicia inmediatamente para que los servicios arranquen con la nueva identidad.

Antes de empezar, consulta el identificador actual y comprueba que está disponible la herramienta que generará el nuevo.

```bash
cat /etc/machine-id
command -v dbus-uuidgen
```

Anota el identificador para compararlo después. El segundo comando debe mostrar la ruta de `dbus-uuidgen`. Si falta, instálalo con `sudo apt install dbus-bin` antes de continuar.

Ejecuta las siguientes órdenes en el clon. Si alguna devuelve un error, detén la secuencia y revisa el mensaje antes de seguir.

```bash
sudo rm -f /etc/ssh/ssh_host_*
sudo ssh-keygen -A

sudo rm -f /etc/machine-id /var/lib/dbus/machine-id
sudo dbus-uuidgen --ensure=/etc/machine-id
sudo ln -s /etc/machine-id /var/lib/dbus/machine-id

sudo reboot
```

Se generan nuevas claves de host SSH y un nuevo `machine-id`. El enlace mantiene el identificador de D-Bus coherente con `/etc/machine-id`.

Después del reinicio, inicia sesión en la consola y comprueba el identificador y las direcciones de red.

```bash
cat /etc/machine-id
ip -br a
```

El identificador debe ser distinto del que anotaste. Elige después la red de la práctica y sigue [Comprobar conectividad y SSH](./05_comprobar_conectividad_y_ssh.md#3-comprobar-ssh) para conectar desde una terminal Linux o PowerShell en el equipo físico. En puente utilizarás la IP recibida por DHCP; con NAT y Host-Only utilizarás la IP Host-Only. La guía explica cómo comprobar la huella antes de aceptar la primera conexión.

## 3. Repetir para los tres sistemas

Al terminar debe existir una máquina madre y al menos un clon de trabajo para cada sistema:

- Ubuntu Desktop;
- Ubuntu Server;
- Windows Server.

Registra las máquinas y comprueba sus MAC con el [inventario y lista de comprobación](./02_inventario_y_comprobacion.md).

## Si surge un problema

- Si la VDI no aparece, comprueba que has seleccionado **usar un disco existente**.
- Si una máquina no arranca, revisa el orden de arranque y que el disco esté conectado.
- Si dos clones muestran la misma MAC, apágalos y genera una nueva MAC antes de configurar la red.
- Si un clon deja de encontrar su disco, comprueba que no se ha movido ni eliminado la máquina madre.
