# UT02 · DHCP: conceptos esenciales

Consulta la presentación y responde con tus palabras. No necesitas buscar información en Internet ni adjuntar capturas. Tiempo orientativo: 25–30 minutos. Entrega un único documento de una o dos páginas con tu nombre y las respuestas numeradas.

## 1. Para qué sirve

En un aula hay 25 ordenadores y los portátiles cambian de red con frecuencia.

- Explica una ventaja de usar DHCP y enumera tres parámetros de red que puede proporcionar.
- Una impresora necesita conservar su IP. ¿Qué diferencia hay entre escribirle una IP manualmente y crear una reserva DHCP para ella?

Extensión orientativa: cuatro líneas en total.

## 2. Primera conexión

Ordena estos mensajes: **DHCPACK, DHCPREQUEST, DHCPDISCOVER y DHCPOFFER**. Para cada uno indica quién lo envía —cliente o servidor— y su finalidad en una frase breve.

| Orden | Mensaje | Emisor | Finalidad |
|---|---|---|---|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |

## 3. Rango, exclusión y reserva

Un servidor tiene IP fija **192.168.20.1/24**. Su ámbito reparte **192.168.20.100–192.168.20.149**. Se excluyen las direcciones **192.168.20.120–192.168.20.129** y se reserva **192.168.20.140** para una impresora.

- ¿Cuántas direcciones contiene el rango antes de aplicar exclusiones?
- ¿Cuántas quedan después de aplicar la exclusión, contando la reservada?
- ¿Para qué sirve la exclusión y qué dato del cliente se utiliza habitualmente para crear la reserva?

## 4. Concesión y fallo

- La concesión dura ocho horas. Si se usan los valores predeterminados, ¿al cabo de cuánto tiempo comienza T1 y qué intenta hacer el cliente?
- Un cliente Windows configurado para obtener IP automáticamente muestra **169.254.30.8/16**. ¿Qué indica? Escribe una comprobación que harías para recuperar una dirección del servidor DHCP.

## Valoración

Se valoran la corrección de las respuestas, los cálculos y el uso de tus propias palabras. No se puntúan la decoración ni las capturas. Se aplica la siguiente rúbrica general:

| Calificación | Criterio |
|---|---|
| 0 | **No entregado.** |
| 3 | **La mayoría de las respuestas son incorrectas.** La tarea presenta errores importantes que muestran una comprensión insuficiente de los contenidos o dificultades para aplicar los procedimientos trabajados. |
| 6 | **La mayoría de las respuestas son correctas.** Se demuestra una comprensión general de los contenidos y una aplicación adecuada de los procedimientos, aunque quedan errores u omisiones por corregir. |
| 9 | **Todo correcto.** La tarea responde de forma completa y correcta a lo solicitado, con explicaciones claras y procedimientos bien aplicados. |
| 10 | **Todo correcto y con una aportación extra.** Además de resolver correctamente la tarea, el alumno incorpora una aportación propia y pertinente que enriquece el trabajo, como un ejemplo, una comprobación adicional o una reflexión razonada. |
