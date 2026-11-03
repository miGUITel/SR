### Modo activo, pasivo y cortafuegos

Activo y pasivo cambian quién inicia la **conexión de datos**. La conexión de control la inicia siempre el cliente hacia el servidor, normalmente al puerto TCP 21.

### 1. **Conexiones en el Modo Activo**

En activo, el cliente anuncia su dirección y puerto mediante `PORT` o `EPRT` y espera que el servidor inicie la conexión de datos. El cortafuegos del cliente debe permitir esa entrada desde el servidor. Si hay NAT, también debe poder alcanzarse la dirección y el puerto anunciados.

Si puedes identificarte, pero falla el listado o la transferencia solo en activo, revisa la dirección anunciada y las reglas de entrada del cliente.

### 2. **Modo Pasivo y Cortafuegos**

En pasivo, el servidor anuncia dónde escucha mediante la respuesta a `PASV` o `EPSV`, y el cliente inicia la conexión de datos. Suele facilitar el uso de clientes situados detrás de un cortafuegos o NAT, pero requiere permitir la entrada al rango pasivo del servidor y, cuando proceda, configurar su publicación a través del NAT.

Si falla solo en pasivo, revisa el rango configurado en el servidor, sus reglas de entrada y la dirección anunciada.

| Canal en el laboratorio 0402 | Quién inicia → dónde se conecta |
| --- | --- |
| Control, en ambos modos | Cliente → servidor, TCP `21`. |
| Datos pasivos | Cliente → puerto TCP anunciado por el servidor. |
| Datos activos con `ftp.exe` | Servidor → puerto TCP anunciado por el cliente. |

En 0402 se compara FileZilla en pasivo con `ftp.exe` en activo, aprovechando la configuración existente cuando funciona. Los puertos de datos se negocian; no hay que fijar rangos personalizados para realizar la comparación. Mantén activo el cortafuegos y consulta con el profesor cualquier ajuste necesario.

**Cambiar a pasivo no cifra FTP ni cambia los permisos.** FTP simple envía credenciales y datos sin cifrar en ambos modos. Por eso las prácticas se realizan en una red interna con cuentas de laboratorio.

Referencias: [Microsoft Learn: rango pasivo de IIS](https://learn.microsoft.com/en-us/iis/configuration/system.ftpserver/firewallsupport), [RFC 2428, EPRT/EPSV](https://www.rfc-editor.org/rfc/rfc2428) y [RFC 2577, privacidad en FTP](https://www.rfc-editor.org/rfc/rfc2577).
