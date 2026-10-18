[Volver a los apuntes de DNS](./SR03.md#32-funcionamiento-del-dns)

# Resolución local con hosts

El archivo `hosts` permite asociar una IP con un nombre en el propio equipo. Es una de las alternativas locales a consultar un servidor DNS que aparecen en la diapositiva 8, junto a la caché del cliente.

La entrada solo afecta al equipo donde se guarda. No cambia el nombre real del destino ni crea un registro en un servidor DNS.

## Modificar el archivo hosts en Windows y Linux

Haz el cambio **en el cliente desde el que quieres resolver el nombre**. Primero conoce y comprueba la IP del equipo de destino. Conserva las entradas existentes.

El formato es el mismo en ambos sistemas:

```text
IP_DEL_EQUIPO    srv-ut03.test
```

Sustituye `IP_DEL_EQUIPO` por la dirección real del destino. Separa IP y nombre con espacios o tabulaciones. No escribas `#` delante: convierte la línea en un comentario y el sistema la ignora.

### Windows

1. Antes de editar, copia el archivo `C:\Windows\System32\drivers\etc\hosts` a otra carpeta, por ejemplo Documentos. Esa será la copia de seguridad; modifica después el **original**.
2. Busca **Bloc de notas** en Inicio, pulsa con el botón derecho y elige **Ejecutar como administrador**. No basta con que tu cuenta pertenezca al grupo de administradores.
3. En **Archivo → Abrir**, entra en `C:\Windows\System32\drivers\etc`. Cambia el filtro «Documentos de texto» por **«Todos los archivos»** y abre `hosts`.
4. Añade la entrada en una línea nueva y guarda con **Ctrl+S**. El archivo debe seguir llamándose `hosts`, sin extensión `.txt`.
5. Abre una consola como administrador, limpia la caché y comprueba el nombre:

   ```powershell
   ipconfig /flushdns
   ping srv-ut03.test
   ```

### Linux / Ubuntu

1. Conserva una copia de seguridad. Por ejemplo:

   ```bash
   sudo cp /etc/hosts /etc/hosts.antes-ut03
   ```

   Si esa copia ya existe, usa otro nombre para conservarla.

2. Abre el archivo original con permisos de administrador:

   ```bash
   sudo nano /etc/hosts
   ```

3. Añade la entrada en una línea nueva. Conserva las de `localhost` y las del nombre del propio equipo.
4. En Nano, guarda con **Ctrl+O**, confirma `/etc/hosts` con **Intro** y sal con **Ctrl+X**.
5. Comprueba la resolución con:

   ```bash
   getent hosts srv-ut03.test
   ping -c 2 srv-ut03.test
   ```

   El cambio normalmente se aplica al guardar, sin reiniciar el equipo. Algunas aplicaciones pueden conservar información anterior en su propia caché.

### Interpretar la comprobación y retirar la prueba

La IP mostrada debe coincidir con la que escribiste. Si `ping` muestra esa IP pero no recibe respuestas, el nombre se ha resuelto: revisa por separado la conectividad y el bloqueo de ICMP.

`nslookup` y `dig` consultan al DNS; no sirven para comprobar una entrada de `hosts`.

Cuando termines, elimina **solo la línea de prueba** del archivo original y guarda. En Windows, vuelve a ejecutar `ipconfig /flushdns`.

### Referencias

- [Microsoft: permisos necesarios para editar hosts](https://learn.microsoft.com/en-us/troubleshoot/windows-server/networking/cannot-modify-hosts-lmhosts-files).
- [Ubuntu: formato y funcionamiento de /etc/hosts](https://manpages.ubuntu.com/manpages/noble/man5/hosts.5.html).
- [Ubuntu: consulta de nombres mediante getent](https://manpages.ubuntu.com/manpages/noble/man1/getent.1.html).
