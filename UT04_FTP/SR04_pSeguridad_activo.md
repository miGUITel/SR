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
| Datos pasivos | Cliente → servidor, TCP `50000–50049`. |
| Datos activos con FileZilla | Servidor → cliente, TCP `51000–51049`. |

Los dos rangos de datos son decisiones de nuestro laboratorio, no puertos fijos del protocolo. `ftp.exe` elige su puerto de datos activo y utiliza la regla específica prevista en 0402; no hereda el rango de FileZilla. Mantén activo el cortafuegos y sigue las reglas limitadas a las IP del guion.

**Cambiar a pasivo no cifra FTP ni cambia los permisos.** FTP simple envía credenciales y datos sin cifrar en ambos modos. Por eso las prácticas se realizan en una red interna con cuentas de laboratorio.

Referencias: [Microsoft Learn: rango pasivo de IIS](https://learn.microsoft.com/en-us/iis/configuration/system.ftpserver/firewallsupport), [RFC 2428, EPRT/EPSV](https://www.rfc-editor.org/rfc/rfc2428) y [RFC 2577, privacidad en FTP](https://www.rfc-editor.org/rfc/rfc2577).
