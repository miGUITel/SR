## Códigos de estado FTP

### **1XX**  
*Respuesta informativa. El comando fue recibido y está siendo procesado, pero el cliente debe esperar una respuesta adicional del servidor para continuar.*

### **2XX**  
*El comando se procesó con éxito y no se requiere ninguna acción adicional.*

### **3XX**  
*Respuesta de continuación. El comando fue aceptado parcialmente, pero el cliente debe proporcionar más información o realizar otra acción para completar la solicitud.*

### **4XX**  
*Error transitorio. El comando no se pudo completar debido a un problema temporal, pero se puede volver a intentar más adelante.*

### **5XX**  
*Respuesta negativa permanente para esa petición en las condiciones actuales. Repetirla sin cambiar nada no resolverá el problema; puede ser necesario corregir la orden, las credenciales, la ruta o los permisos.* 

### Ejemplos para las prácticas

| Código | Interpretación |
| --- | --- |
| `220` | Servicio preparado para una nueva conexión. |
| `230` | Usuario identificado correctamente. |
| `331` | Se necesita la contraseña; todavía no se ha completado el acceso. |
| `150` | Preparado para abrir los datos; aún no confirma el resultado final. |
| `226` | Cierre del canal de datos; en una transferencia normal indica que ha terminado correctamente. |
| `227` / `229` | Respuesta a `PASV` / `EPSV`: el servidor anuncia dónde espera la conexión de datos. |
| `425` | No se puede abrir la conexión de datos. |
| `530` | No se ha iniciado sesión correctamente. |
| `550` | Acción no realizada: por ejemplo, archivo inexistente o acceso denegado. |

Lee también el texto completo de la respuesta y la orden que la ha provocado. Un `550` no demuestra por sí solo un problema de permisos.

Referencias: [RFC 959, respuestas FTP](https://www.rfc-editor.org/rfc/rfc959) y [RFC 2428, respuesta 229](https://www.rfc-editor.org/rfc/rfc2428).
