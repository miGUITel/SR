# 0303 FQDN (opcional)

Refuerzo opcional sobre la jerarquía DNS. No necesitas instalar un servidor ni realizar consultas reales.

### Objetivo de la práctica

Entender que el DNS funciona como un **sistema jerárquico**, identificar un **FQDN** y seguir de forma conceptual el proceso de resolución de un nombre **antes de instalar ningún servidor DNS**.

---

## Tareas a realizar

### **1. Identificación y descomposición de FQDN**

Para cada uno de los siguientes nombres:

* `www.google.com.`
* `www.educacion.gob.es.`
* `pc01.aula.example.com.`
* `pc01`

El punto final representa la raíz y deja explícito que el nombre es absoluto; en muchos programas se omite al escribirlo. Analiza la estructura, sin comprobar si estos nombres existen o responden.

Indica:

* Si expresa un **nombre completo hasta la raíz (FQDN)** o necesita un sufijo para situarlo en la jerarquía.
* En caso afirmativo, descompón el nombre indicando:

  * TLD
  * Dominio de segundo nivel, por ejemplo `example.com.`
  * Subdominios intermedios (si existen)
  * Etiqueta que identifica al host en estos ejemplos

📌 *Se pretende comprobar que sabes leer un nombre DNS de derecha a izquierda.*

---

### **2. Zonas DNS y jerarquía**

Responde brevemente:

* ¿Qué problemas tendría centralizar todo el DNS de Internet en un único servidor?
* ¿Qué es una **zona DNS** y qué problema resuelve dentro de la jerarquía?

📌 *No se buscan definiciones de memoria, sino una explicación razonada.*

---

### **3. Resolución conceptual “desde la raíz”**

Supón este escenario didáctico: un cliente quiere resolver el nombre siguiente. Su DNS configurado no tiene la respuesta en caché y resolverá desde la raíz, sin reenviadores. `example.com.` delega `aula.example.com.` en un servidor autoritativo que contiene el registro A del equipo.

```
pc01.aula.example.com.
```

Describe **el recorrido conceptual** de la resolución DNS, indicando:

* A quién pregunta primero el cliente y desde qué nivel busca después el DNS configurado.
* En qué punto se encuentra el servidor autoritativo.
* Dónde se obtiene finalmente la dirección IP y quién se la devuelve al cliente.

📌 *Es un ejemplo para razonar sobre la jerarquía, no una configuración pública que debas probar. No es necesario usar direcciones IP reales ni servidores concretos.*

---

## Entrega solicitada

* Un PDF con las respuestas a los tres apartados.
* Esquema simple de la jerarquía DNS para el nombre analizado (opcional, pero recomendable).

---

### Rúbrica general

| Calificación | Criterio |
|---|---|
| 0 | **No entregado.** |
| 3 | **La mayoría de las respuestas son incorrectas.** La tarea presenta errores importantes que muestran una comprensión insuficiente de los contenidos o dificultades para aplicar los procedimientos trabajados. |
| 6 | **La mayoría de las respuestas son correctas.** Se demuestra una comprensión general de los contenidos y una aplicación adecuada de los procedimientos, aunque quedan errores u omisiones por corregir. |
| 9 | **Todo correcto.** La tarea responde de forma completa y correcta a lo solicitado, con explicaciones claras y procedimientos bien aplicados. |
| 10 | **Todo correcto y con una aportación extra.** Además de resolver correctamente la tarea, el alumno incorpora una aportación propia y pertinente que enriquece el trabajo, como un ejemplo, una comprobación adicional o una reflexión razonada. |

