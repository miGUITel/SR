# 0301 archivo hosts - DNS

Práctica inicial con **entrega opcional**.

**Versión anterior:** [Consultar el enunciado original para las entregas realizadas con esa versión](https://github.com/miGUITel/SR/blob/6576cfad70f4fba25779c9c18c0a02bd8ceaf485/TAREAS/SR03DNS/propuesta/S0301_hosts_OP.md).

### Objetivo y escenario

Relacionar un nombre con una IP mediante `hosts` y comprobar las limitaciones de mantener esa información en cada equipo.

Necesitas un cliente Windows y otro equipo accesible en la misma red, real o virtual. Realiza las pruebas y modifica `hosts` **en el cliente**. No hay que instalar ni modificar un servidor DNS. En Linux, el archivo equivalente es `/etc/hosts`; los comandos de esta guía son para Windows.

`hosts` es un archivo local que asocia direcciones IP con nombres. La entrada que añadas solo se utilizará en el cliente donde la escribas.

**Consulta:** [Cómo modificar el archivo hosts en Windows y Linux](../../../UT03/SR03_hosts.md#modificar-el-archivo-hosts-en-windows-y-linux).

### Procedimiento

1. **Comprueba el acceso por IP.** Obtén la IP del otro equipo (en Windows, con `ipconfig`, mirando la dirección IPv4 del adaptador conectado a esta red). En el cliente, abre una consola como administrador y ejecuta `ping IP_DEL_EQUIPO`, sustituyendo el texto por esa dirección real. Si no responde, comprueba con el profesor la conectividad o el bloqueo de ICMP antes de continuar.

2. **Prueba un nombre nuevo.** Ejecuta `ping srv-ut03.test` y anota si obtiene una IP. Este será un alias de prueba: no necesitas cambiar el nombre real del otro equipo. Si ya se resuelve, utiliza otro nombre libre terminado en `.test` durante toda la práctica. Guarda la evidencia del resultado inicial antes de editar `hosts`.

3. **Consulta y modifica hosts.** Abre el Bloc de notas **como administrador** y, desde él, `C:\Windows\System32\drivers\etc\hosts` (selecciona «Todos los archivos»). Guarda una copia de seguridad, observa su contenido y añade:

   ```text
   IP_DEL_EQUIPO    srv-ut03.test
   ```

   Sustituye `IP_DEL_EQUIPO` por la dirección comprobada. Escribe la entrada en una línea nueva, sin `#` delante: ese símbolo indica un comentario. Guarda **sin añadir `.txt`** y sin borrar las entradas existentes.

4. **Comprueba la resolución.** Ejecuta `ipconfig /flushdns` y después `ping srv-ut03.test`. Comprueba que aparece la IP elegida. Aunque no lleguen respuestas al ping, que aparezca esa IP demuestra la resolución del nombre. **No uses `nslookup` para verificar hosts:** consulta al servidor DNS.

5. **Limpia la prueba.** Tras guardar las evidencias, elimina únicamente la línea añadida y vuelve a ejecutar `ipconfig /flushdns`.

### Entrega en PDF

Incluye estas evidencias, cada una con una explicación breve. Puedes agrupar varias en la misma captura si se leen bien:

- Prueba inicial por IP y resultado inicial por nombre, antes de editar `hosts`.
- Entrada añadida a `hosts`.
- Prueba posterior por nombre, con la IP obtenida visible.

Responde también:

- ¿Qué ocurre si cambia la IP del equipo?
- ¿Sería viable este método con muchos equipos?
- ¿Qué problemas aparecen si cada equipo mantiene su propio archivo?

### Rúbrica general

| Calificación | Criterio |
|---|---|
| 0 | **No entregado.** |
| 3 | **La mayoría de las respuestas son incorrectas.** La tarea presenta errores importantes que muestran una comprensión insuficiente de los contenidos o dificultades para aplicar los procedimientos trabajados. |
| 6 | **La mayoría de las respuestas son correctas.** Se demuestra una comprensión general de los contenidos y una aplicación adecuada de los procedimientos, aunque quedan errores u omisiones por corregir. |
| 9 | **Todo correcto.** La tarea responde de forma completa y correcta a lo solicitado, con explicaciones claras y procedimientos bien aplicados. |
| 10 | **Todo correcto y con una aportación extra.** Además de resolver correctamente la tarea, el alumno incorpora una aportación propia y pertinente que enriquece el trabajo, como un ejemplo, una comprobación adicional o una reflexión razonada. |
