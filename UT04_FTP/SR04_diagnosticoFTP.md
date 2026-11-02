# Diagnóstico rápido de FTP

Localiza primero **en qué momento falla** la operación. Lee la orden y la respuesta completa del servidor; cambia una sola cosa cada vez y repite la prueba.

| Qué observas | Qué conviene comprobar primero |
| --- | --- |
| No conecta al servidor. | IP, red interna, servicio iniciado y entrada TCP 21 del servidor. |
| Conecta, pero rechaza la identificación. | Usuario, contraseña, método de autenticación y reglas de autorización FTP. |
| Se identifica, pero no lista ni transfiere. | Canal de datos: modo elegido, dirección anunciada y cortafuegos. Si solo falla activo, revisa la entrada al cliente; si solo falla pasivo, el rango y la entrada al servidor. |
| Lista y descarga, pero rechaza una subida o un borrado. | Si ese resultado está previsto para la cuenta. Después, reglas de IIS, permisos NTFS y ruta utilizada. Un `550` también puede indicar que el archivo no existe. |
| Rechaza una conexión adicional. | Sesiones ya abiertas y máximo de conexiones del sitio. Cierra las que no necesites y repite. |

Estas comprobaciones orientan la búsqueda; un síntoma no demuestra por sí solo una causa.

**Conectar no equivale a transferir.** Una prueba completa incluye identificación, listado y transferencia de un archivo; comprueba también el contenido recibido. Con una cuenta de lectura, verifica que la escritura se rechaza.

Mantén activado el cortafuegos y respeta los permisos previstos. Evita resolver un fallo dando acceso general: primero explica qué operación falla y por qué.

Consulta: [elementos y conexiones](SR04_elementosYconexiones.md), [activo/pasivo y cortafuegos](SR04_pSeguridad_activo.md), [respuestas FTP](UT04_códigosEstado.md) y [usuarios, permisos y límites](SR04_usuarios_permisos_limites.md).
