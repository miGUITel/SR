### Elementos y Conexiones en un Protocolo FTP

FTP (File Transfer Protocol) permite transferir archivos entre un cliente y un servidor. Utiliza una conexión de **control** para órdenes y respuestas y otra de **datos** para listados y archivos. El cliente inicia el control; activo y pasivo indican quién inicia los datos. Ambos modos permiten subir y descargar.

Las siglas PI y DTP describen los componentes que gestionan estas conexiones y se mantienen aquí como consulta.

#### 1. **Intérprete del Protocolo (PI - Protocol Interpreter)**
La PI es la encargada de interpretar y gestionar los comandos que se envían entre el cliente y el servidor. En el contexto de FTP, tanto el cliente como el servidor tienen su propia PI, y estas son las responsables de:

- **Cliente PI**: inicia la conexión de control hacia TCP 21 del servidor, salvo que se configure otro puerto; envía comandos e interpreta respuestas.
- **Servidor PI**: interpreta los comandos y responde con códigos de estado (por ejemplo, 200, 421, 550).

En la comunicación entre cliente y servidor FTP, la PI gestiona la "conexión de control", que es la línea dedicada a la transmisión de comandos y respuestas.

#### 2. **Conexión de Control**
La conexión de control la inicia el cliente hacia el puerto TCP 21 del servidor, en la configuración habitual. Esta conexión se mantiene activa durante toda la sesión y es la encargada de transmitir los comandos y respuestas de control. Por ejemplo, comandos como `USER`, `PASS`, `LIST`, `STOR`, entre otros, viajan a través de esta conexión.

- **Cliente PI** envía los comandos.
- **Servidor PI** interpreta estos comandos y responde.

La conexión de control no se utiliza para transferir datos, sino solamente para la gestión de la sesión.

#### 3. **Proceso de Transferencia de Datos (DTP - Data Transfer Process)**
El DTP es el componente encargado de gestionar la transferencia de datos entre el cliente y el servidor. Tanto el cliente como el servidor tienen su propio DTP.

- **Servidor DTP**: Se encarga de enviar o recibir datos desde o hacia el cliente.
- **Cliente DTP**: Solicita y gestiona la recepción o envío de datos hacia el servidor.

El DTP es quien realmente se encarga de la transferencia de archivos, ya sean de subida, bajada o listados de directorios.

#### 4. **Conexión de Datos**
La conexión de datos se establece para transferir archivos o listar el contenido de un directorio. En el modo de transferencia utilizado en estas prácticas, se abre para cada listado o transferencia y se cierra al terminar. Puede funcionar de dos formas:

- **Modo Activo**: En el modo activo, el servidor es el encargado de establecer la conexión de datos con el cliente. El cliente informa al servidor a qué puerto debe conectarse, y el servidor inicia la conexión hacia ese puerto. El cliente anuncia su dirección y puerto mediante `PORT` o `EPRT`. El puerto 20 es el origen tradicional del servidor en activo; el puerto de destino lo anuncia el cliente.
- **Modo Pasivo**: En el modo pasivo, el cliente es el que establece la conexión de datos con el servidor. El cliente solicita el modo pasivo mediante `PASV` o `EPSV`; el servidor anuncia dónde escucha y el cliente inicia la conexión de datos hacia ese puerto. Este modo es más adecuado cuando el cliente está detrás de un cortafuegos o NAT, ya que evita que el servidor tenga que iniciar conexiones hacia el cliente.

#### 5. **Resumen de la Comunicación entre Cliente y Servidor**
- **Conexión de Control**: Siempre está activa durante la sesión. Utiliza el puerto 21 para el intercambio de comandos y respuestas.
- **Conexión de Datos**: Se establece de forma temporal cuando se necesita transferir datos (archivos o listados). En modo activo, la inicia el servidor; en modo pasivo, la inicia el cliente.
- **PI (Protocol Interpreter)**: Gestiona los comandos y respuestas de control.
- **DTP (Data Transfer Process)**: Se encarga de la transferencia efectiva de los archivos.

#### 6. **Ejemplo de Flujo de Trabajo**
1. **Cliente PI** se conecta al **Servidor PI** a través del puerto 21. Se establece la conexión de control.
2. Se prepara la conexión de datos: en activo, el cliente anuncia dónde escuchará; en pasivo, el servidor anuncia dónde escucha.
3. El cliente solicita un listado (`LIST`). La conexión de datos la inicia el servidor en activo o el cliente en pasivo, y por ella el servidor envía el listado. En pasivo puede haberse establecido antes de `LIST`.
4. Una vez completada la transferencia de datos, la conexión de datos se cierra, pero la conexión de control permanece abierta para otros comandos.

**Para el laboratorio:** iniciar sesión correctamente no demuestra que el canal de datos funcione. Comprueba también listado, subida y descarga.

Referencia: [RFC 959, modelo y conexiones FTP](https://www.rfc-editor.org/rfc/rfc959) y [RFC 2428, EPRT/EPSV](https://www.rfc-editor.org/rfc/rfc2428).

