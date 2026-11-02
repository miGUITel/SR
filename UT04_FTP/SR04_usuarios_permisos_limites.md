# Usuarios, permisos y límites en FTP IIS

Una empresa quiere que Informática pueda actualizar archivos, que Contabilidad solo los consulte y que los visitantes descarguen información pública. Tener el servidor instalado no basta: hay que decidir quién entra y qué puede hacer.

## 1. Tres comprobaciones distintas

| Comprobación | Pregunta | Dónde se configura |
| --- | --- | --- |
| Autenticación | ¿Con qué identidad accedes? | Autenticación FTP de IIS. |
| Autorización FTP | ¿Qué permite hacer el servicio a esa identidad o grupo? | Reglas de autorización FTP de IIS: lectura y escritura. |
| Permisos NTFS | ¿Qué permite Windows sobre la carpeta y sus archivos? | Propiedades de la carpeta → Seguridad. |

Para leer o escribir un archivo, la operación debe estar permitida **tanto en IIS como en NTFS**. Por ejemplo, habilitar escritura en IIS no basta si Windows solo concede lectura.

Un **usuario** identifica una cuenta. Un **grupo** reúne cuentas que comparten una función y facilita asignarles los mismos permisos. En la práctica, `adminftp` es una cuenta ordinaria: su nombre no la convierte en administrador de Windows.

## 2. La política de nuestro laboratorio

| Acceso | Regla FTP | Permiso NTFS |
| --- | --- | --- |
| `adminftp` | Lectura y escritura | Modificar. |
| Grupo `informaticos` | Lectura y escritura | Modificar. |
| Grupo `contables` | Lectura | Lectura y ejecución. |
| Visitante anónimo | Lectura | Lectura y ejecución para `IUSR`. |

En la configuración de 0403, el cliente entra como `anonymous`, pero IIS utiliza la identidad de Windows **`IUSR`** para acceder a la carpeta. No son dos cuentas equivalentes ni hay que crear un usuario local llamado `anonymous`.

Revisa también los permisos heredados y la pertenencia a otros grupos: pueden conceder más acceso del previsto. Para dejar una cuenta en lectura no hace falta añadir una denegación expresa de escritura; sigue la configuración del guion.

**Una operación denegada puede ser el resultado correcto.** Contabilidad y el visitante deben poder descargar, pero no subir ni borrar. Cambiar de activo a pasivo no les concede permisos.

## 3. Límites de conexiones

El máximo de conexiones limita los accesos simultáneos al sitio; no limita cuántas cuentas has creado ni la velocidad de descarga.

En 0403 se establece temporalmente un máximo de **2**. Manteniendo dos sesiones abiertas, la conexión adicional debe rechazarse. Al cerrar una, vuelve a quedar una plaza disponible. Para contar las sesiones con claridad, se utilizan ventanas de `ftp.exe` sin listados ni transferencias: un cliente gráfico puede mantener conexiones adicionales.

Al acabar, restaura el valor de trabajo del guion, **50**, salvo otra indicación del profesor. El rechazo por límite se resuelve revisando las sesiones y el máximo configurado, no cambiando contraseñas ni permisos.

Referencias: Microsoft Learn, [autorización FTP](https://learn.microsoft.com/en-us/iis/configuration/system.ftpserver/security/authorization/), [autenticación anónima](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/ftpserver/security/authentication/anonymousauthentication/) y [conexiones FTP](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/ftpserver/connections/).
