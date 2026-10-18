# 0302 NSLOOKUP: comprueba DNS

Práctica inicial con **entrega opcional**.

### Objetivo

Observar la caché DNS de Windows y distinguir una consulta del sistema de una consulta explícita al servidor DNS. La caché guarda temporalmente información de resolución para poder reutilizarla.

Trabaja en el cliente Windows, con conexión a la red y un DNS configurado. Abre una consola como administrador para limpiar la caché. Si has realizado la práctica de `hosts`, retira antes la entrada de prueba tal como indica su último paso.

### Procedimiento

1. **Observa la caché inicial.** Ejecuta y guarda la captura antes de continuar:

   ```powershell
   ipconfig /displaydns
   ```

2. **Limpia la caché.** Ejecuta y comprueba el mensaje de confirmación:

   ```powershell
   ipconfig /flushdns
   ```

3. **Vuelve a observarla.** Ejecuta:

   ```powershell
   ipconfig /displaydns
   ```

   Guarda la captura y compárala con la inicial. Pueden permanecer entradas de `hosts` y aparecer otras por programas en segundo plano; no tiene por qué quedar vacía.

4. **Provoca resoluciones de nombres.** Ejecuta:

   ```powershell
   ping www.google.com
   ping www.iesinfantaelena.com
   ```

   **Fíjate primero en la IP que aparece junto al nombre.** Si ves una salida como «Haciendo ping a … [dirección IP]», el sistema ha resuelto el nombre y ya sabe a qué IP dirigir el ping. La resolución ha funcionado, aunque después aparezca «Tiempo de espera agotado»: lo que falta es la respuesta ICMP, que puede estar bloqueada. **No necesitas recibir respuestas al ping para completar esta práctica DNS.** Anota la IP obtenida; si no se obtiene ninguna, registra el mensaje y consúltalo con el profesor.

   La IP también puede proceder de una respuesta DNS guardada en caché; resolver el nombre no implica enviar una consulta nueva cada vez.

   **Variante opcional: acceder desde el navegador.** Abre `https://www.google.com` o `https://www.iesinfantaelena.com`. Para conectar con la web por su nombre, el navegador también necesita obtener la IP mediante resolución DNS, salvo que ya disponga de ella en caché. Puede usar su propia caché o DNS seguro, por lo que esa actividad no siempre aparecerá en `ipconfig /displaydns`. Para observar la caché de Windows en el paso siguiente, conserva la prueba con `ping` como referencia.

5. **Busca las entradas en caché.** Ejecuta:

   ```powershell
   ipconfig /displaydns
   ```

   Localiza las entradas relacionadas con los nombres anteriores y guarda la captura. El TTL indica cuántos segundos puede seguir siendo válida una entrada en caché. Si no la encuentras, registra lo observado y consúltalo con el profesor.

6. **Repite la resolución.** Ejecuta:

   ```powershell
   ping www.google.com
   ```

   El cliente puede reutilizar la información almacenada mientras sea válida. La rapidez del ping, por sí sola, no demuestra el uso de la caché.

7. **Consulta directamente al DNS.** Ejecuta:

   ```powershell
   nslookup www.google.com
   ```

   Identifica el **servidor DNS consultado** y su dirección, al principio de la salida. Distingue esos datos del **nombre consultado y sus direcciones**, que aparecen en la respuesta. Guarda la captura completa.

   `nslookup` consulta al servidor DNS y no comprueba las entradas de `hosts` ni la caché del cliente mostrada por `ipconfig`.

### Cómo interpretar la salida

Utiliza esta tabla para interpretar tu resultado. **No tienes que provocar todos los casos.** La respuesta autoritativa se comprobará más adelante con el DNS del laboratorio.

| Resultado | Interpretación |
|---|---|
| Respuesta no autoritativa | El servidor responde sin ser autoridad para ese nombre. Puede utilizar su caché o consultar otros DNS. |
| Respuesta autoritativa | Procede de un servidor responsable de la zona. Se comprobará con la zona del laboratorio; la ausencia de «no autoritativa», por sí sola, no basta para interpretar cualquier salida. |
| `Non-existent domain` / NXDOMAIN | Se ha recibido una respuesta DNS que indica que el nombre consultado no existe. No equivale a un problema de conexión. |
| Tiempo de espera agotado / timeout | No ha llegado una respuesta a tiempo. Hay que revisar conectividad, DNS configurado, filtros y posibles problemas del servidor. |

### Entrega en PDF

- Capturas de la caché antes y después de limpiarla.
- Captura de las entradas tras los primeros pings.
- Captura de `nslookup`, con el servidor y el resultado visibles.
- Explicación del proceso en 2–3 líneas. Explica las incidencias observadas, si las hubo.

### Rúbrica general

| Calificación | Criterio |
|---|---|
| 0 | **No entregado.** |
| 3 | **La mayoría de las respuestas son incorrectas.** La tarea presenta errores importantes que muestran una comprensión insuficiente de los contenidos o dificultades para aplicar los procedimientos trabajados. |
| 6 | **La mayoría de las respuestas son correctas.** Se demuestra una comprensión general de los contenidos y una aplicación adecuada de los procedimientos, aunque quedan errores u omisiones por corregir. |
| 9 | **Todo correcto.** La tarea responde de forma completa y correcta a lo solicitado, con explicaciones claras y procedimientos bien aplicados. |
| 10 | **Todo correcto y con una aportación extra.** Además de resolver correctamente la tarea, el alumno incorpora una aportación propia y pertinente que enriquece el trabajo, como un ejemplo, una comprobación adicional o una reflexión razonada. |
