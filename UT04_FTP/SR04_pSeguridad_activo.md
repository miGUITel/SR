El problema de seguridad relacionado con el uso del modo activo en FTP, que podría motivar a un cliente a evitar este modo y preferir el modo pasivo, se basa principalmente en la forma en que se establecen las conexiones de datos.

### 1. **Conexiones en el Modo Activo**
En el modo activo, el cliente abre un puerto y espera que el servidor FTP inicie la conexión de datos hacia ese puerto. Esto implica:
- **Riesgo de Ataques**: Como el cliente debe abrir un puerto y estar a la espera de conexiones entrantes, esto puede ser aprovechado por un atacante que quiera enviar tráfico malicioso al cliente.
- **Restricciones de Firewall**: La mayoría de los clientes se ejecutan detrás de un firewall que bloquea conexiones entrantes no solicitadas. Esto puede impedir la operación correcta del modo activo, ya que las conexiones de datos provenientes del servidor se bloquean por razones de seguridad.

### 2. **Modo Pasivo como Alternativa Segura**
El modo pasivo resuelve estos problemas al hacer que el cliente inicie todas las conexiones (tanto de control como de datos), lo cual es más seguro y funciona mejor con firewalls y NAT. En este modo:
- El cliente se conecta al servidor, y el servidor proporciona un puerto al cual el cliente se conectará para la transferencia de datos.
- Esto reduce significativamente los riesgos de exposición del cliente a ataques o intrusiones no deseadas.

Por lo tanto, el principal problema de seguridad en el modo activo es la vulnerabilidad que se crea al tener que abrir un puerto en el cliente para recibir conexiones entrantes, lo cual podría ser explotado por atacantes.