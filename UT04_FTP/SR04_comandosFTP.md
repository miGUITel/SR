### 💻 **Comandos FTP básicos (puede variar según la aplicación cliente)**

Estas órdenes se escriben en el cliente interactivo `ftp.exe`, después de iniciarlo desde la consola. El indicador `ftp>` ayuda a distinguirlo de PowerShell. Otros clientes pueden utilizar opciones distintas.

| Comando | Descripción |
| --- | --- |
| `open <ip>` | Conecta con el servidor FTP (por ejemplo, `open 192.168.4.10`). |
| `user <nombre>` | Inicia sesión con el usuario indicado; solicita su contraseña. |
| `ls` / `dir` | Lista los archivos del directorio remoto actual. |
| `pwd` | Muestra el directorio remoto actual. |
| `cd <carpeta>` | Cambia de carpeta en el servidor. |
| `lcd <carpeta>` | Cambia la carpeta local desde la que subes archivos y en la que los descargas. |
| `binary` | Selecciona transferencia binaria para conservar los bytes del archivo. |
| `get <archivo>` | Descarga un archivo del servidor. |
| `put <archivo>` | Sube un archivo al servidor. |
| `mkdir <carpeta>` | Crea una carpeta remota si la cuenta tiene permisos. |
| `delete <archivo>` | Elimina un archivo remoto si la cuenta tiene permisos. |
| `quit` | Cierra la sesión y sale del cliente FTP. |

`lcd` actúa en tu equipo; `cd`, en el servidor. Las carpetas locales deben existir antes de utilizarlas.

`get` y `put` indican el sentido del archivo, no el modo activo o pasivo. En este laboratorio se utiliza `ftp.exe` en activo; para pasivo sigue la opción FileZilla o `curl.exe` indicada en 0402. La sintaxis de `curl.exe` es diferente.

Referencia: [Microsoft Learn: cliente FTP](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp), [lcd](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp-lcd), [binary](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp-binary) y [mkdir](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp-mkdir).
