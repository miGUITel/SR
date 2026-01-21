Perfecto. Te dejo una **versión solucionada**, pensada **solo para el profesor**, con respuestas claras, razonadas y del **nivel adecuado para SMR**. No es excesivamente técnica, pero sí conceptualmente correcta.

---

# 🧪 **Práctica 3 — Estructura jerárquica del DNS**

### **Versión solucionada (profesor)**

---

## **1. Identificación y descomposición de FQDN**

### `www.google.com`

* **¿Es FQDN?** → **Sí**
* **TLD** → `.com`
* **Dominio** → `google`
* **Subdominio** → `www`
* **Host** → `www`

📌 Nombre completamente cualificado, válido a nivel global.

---

### `www.educacion.gob.es`

* **¿Es FQDN?** → **Sí**
* **TLD** → `.es`
* **Dominio** → `gob`
* **Subdominio** → `educacion`
* **Host** → `www`

📌 Ejemplo claro de jerarquía multinivel típica de administraciones públicas.

---

### `pc01.aula.centro.local`

* **¿Es FQDN?** → **Sí**
* **TLD** → `.local`
* **Dominio** → `centro`
* **Subdominio** → `aula`
* **Host** → `pc01`

📌 FQDN válido **en un contexto de red local**, no resoluble en Internet.

---

### `pc01`

* **¿Es FQDN?** → **No**
* Es un **nombre corto**, depende del contexto (sufijo DNS configurado).
* No identifica de forma global ni jerárquica a un equipo.

---

## **2. Zonas DNS y jerarquía**

### ¿Por qué DNS no puede estar centralizado en un único servidor?

Porque:

* El número de dominios es **enorme**.
* Un único servidor sería un **cuello de botella**.
* No habría tolerancia a fallos.
* No sería escalable ni mantenible.

El DNS se organiza de forma **jerárquica y distribuida**, donde cada servidor es responsable solo de una parte.

---

### ¿Qué es una zona DNS y qué problema resuelve?

Una **zona DNS** es una **porción del espacio de nombres** cuya información gestiona un servidor DNS.

Resuelve:

* La **distribución de la información**.
* La **delegación de responsabilidades**.
* La **redundancia y tolerancia a fallos** (zonas primarias/secundarias).

Una zona **no es exactamente lo mismo que un dominio**, aunque estén relacionados.

---

## **3. Resolución conceptual “desde la raíz”**

Para resolver:

```
pc01.aula.centro.local
```

El proceso conceptual sería:

1. El cliente inicia la consulta desde la **raíz DNS** (`.`).
2. Se localiza el servidor responsable del dominio de nivel superior (`local`).
3. Ese servidor conoce (o delega) el dominio `centro.local`.
4. El servidor autoritativo de `centro.local` responde por el subdominio `aula`.
5. Finalmente, el servidor autoritativo devuelve la **dirección IP de `pc01`**.

📌 Lo importante es el **orden jerárquico**, no los servidores concretos.

---

## **Idea clave para el docente**

Si el alumno:

* Lee los nombres de **derecha a izquierda**,
* Entiende que **no todo DNS es Internet**,
* Y comprende que **las zonas existen para repartir responsabilidades**,

entonces está preparado para empezar a **configurar DNS sin hacerlo “a ciegas”** en la siguiente sesión.
