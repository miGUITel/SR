## 🧪 **Práctica 1 OPCIONAL — Resolución de nombres sin servicio DNS**

### Contexto

En una red pequeña es posible acceder a los equipos directamente mediante su dirección IP.
Sin embargo, en redes medianas o grandes, trabajar únicamente con direcciones IP no resulta práctico ni escalable.

En esta práctica vas a comprobar **qué ocurre cuando no existe un servicio de resolución de nombres** y qué mecanismos básicos utiliza el sistema operativo para resolver nombres **sin DNS**.

---

### Objetivo de la práctica

* Comprender el problema real de acceder a recursos usando solo direcciones IP.
* Diferenciar entre **acceso por IP** y **acceso por nombre**.
* Observar el comportamiento del sistema cuando **no existe resolución de nombres centralizada**.
* Introducir el concepto de **nombre de host** y su resolución local.

---

### Escenario de trabajo

Dispones de:

* Un equipo cliente (Windows o Linux).
* Al menos otro equipo accesible en la misma red local (real o virtual).

No se utilizará ningún servidor DNS en esta práctica.

---

### Tareas a realizar

1. **Acceso a un equipo mediante su dirección IP**

   * Comprueba que puedes comunicarte con otro equipo de la red utilizando directamente su dirección IP.
   * Anota si el acceso es inmediato y si requiere conocer previamente algún dato adicional.

2. **Intento de acceso mediante nombre**

   * Intenta acceder al mismo equipo utilizando un nombre en lugar de la IP.
   * Observa y anota el resultado.

3. **Análisis del fichero de resolución local**

   * Localiza el fichero de resolución de nombres local del sistema operativo.
   * Observa su contenido y describe para qué sirve.
   * Identifica qué tipo de resolución permite y qué limitaciones tiene.

4. **Resolución manual de un nombre**

   * Añade una entrada manual que asocie un nombre a una dirección IP.
   * Comprueba que, tras esta modificación, el acceso por nombre funciona.

5. **Reflexión guiada**

   * Responde razonadamente:

     * ¿Qué ocurre si la IP del equipo cambia?
     * ¿Sería viable este método en una red con muchos equipos?
     * ¿Qué problemas aparecen si cada equipo mantiene su propio fichero local?

---

### Entrega solicitada (EN PDF)

* Captura que demuestre acceso correcto mediante IP.
* Captura que muestre el fallo de resolución por nombre (antes de la modificación).
* Captura del fichero de resolución local con la entrada añadida.
* Captura que demuestre que el nombre ya se resuelve correctamente.
* Respuestas breves a las preguntas de reflexión.

---

### Criterios de valoración (formativa)

* Comprensión del problema que resuelve DNS.
* Capacidad para observar y explicar el comportamiento del sistema.
* Corrección en las conclusiones extraídas.

> **Esta práctica no se evalúa con nota**, pero es imprescindible para entender el resto de la unidad.

---

### Idea clave que debe quedar clara al finalizar

> *Sin un servicio de resolución de nombres, una red puede funcionar,
> pero deja de ser práctica, mantenible y escalable.*
