## 🧪 **Práctica 3 OPCIONAL — Estructura jerárquica del DNS**

### Objetivo de la práctica

Entender que el DNS funciona como un **sistema jerárquico**, identificar un **FQDN** y seguir de forma conceptual el proceso de resolución de un nombre **antes de instalar ningún servidor DNS**.

---

## Tareas a realizar

### **1. Identificación y descomposición de FQDN**

Para cada uno de los siguientes nombres:

* `www.google.com`
* `www.educacion.gob.es`
* `pc01.aula.centro.local`
* `pc01`

Indica:

* Si es un **FQDN válido** o no.
* En caso afirmativo, descompón el nombre indicando:

  * TLD
  * Dominio
  * Subdominio (si existe)
  * Nombre del host

📌 *Se pretende comprobar que sabes leer un nombre DNS de derecha a izquierda.*

---

### **2. Zonas DNS y jerarquía**

Responde brevemente:

* ¿Por qué el sistema DNS **no puede estar centralizado en un único servidor**?
* ¿Qué es una **zona DNS** y qué problema resuelve dentro de la jerarquía?

📌 *No se buscan definiciones de memoria, sino una explicación razonada.*

---

### **3. Resolución conceptual “desde la raíz”**

Supón que un cliente quiere resolver el nombre:

```
pc01.aula.centro.local
```

Describe **el recorrido conceptual** de la resolución DNS, indicando:

* Desde qué nivel comienza la búsqueda.
* En qué punto se encuentra el servidor autoritativo.
* Dónde se obtiene finalmente la dirección IP.

📌 *No es necesario usar direcciones IP reales ni servidores concretos.*

---

## Entrega solicitada

* Documento con las respuestas a las tres tareas.
* Esquema simple de la jerarquía DNS para el nombre analizado (opcional, pero recomendable).

---

### Idea clave que debe quedar clara

> *Antes de configurar DNS, hay que entender su jerarquía.
> Sin ese modelo mental, la configuración no tiene sentido.*

