# 🧪 **Práctica 5 — Creación de zona directa y resolución de nombres**

**Módulo:** Servicios en Red
**Unidad de Trabajo:** UT03 – DNS
**Sesión:** 5

---

## Objetivo de la práctica

Crear una **zona de búsqueda directa** en el servidor DNS y añadir **registros básicos coherentes**, comprobando desde un cliente que los nombres definidos se resuelven correctamente.

El objetivo no es “llenar el DNS”, sino **entender que DNS funciona a base de datos bien estructurados**.

---

## Escenario de trabajo

* Servidor DNS con **Windows Server 2019**, ya instalado y operativo (Práctica 4).
* Red configurada en **red interna**.
* Al menos un equipo cliente en la misma red interna.

---

## Tareas a realizar

---

## 1. Creación de la zona directa

En el **Administrador DNS** del servidor:

1. Accede a **Zonas de búsqueda directa**.
2. Crea una nueva zona con las siguientes características:

   * Tipo de zona: **Zona principal**
   * Nombre de la zona:

     ```
     ejemplo.local
     ```
   * No permitir actualizaciones dinámicas.

Finaliza el asistente y comprueba que la zona aparece creada.

📎 *Captura reutilizable de tu guion ampliado:*

```md
![alt text](2.png)
```

*(Creación de zona directa)*

---

## 2. Observación del registro SOA y NS

Dentro de la zona `ejemplo.local`, observa los registros creados automáticamente:

* Registro **SOA**
* Registro(s) **NS**

Responde brevemente:

* ¿Por qué estos registros existen aunque no los hayas creado manualmente?
* ¿Qué información general aporta cada uno?

📌 No es necesario modificar estos registros en esta práctica.

---

## 3. Creación de registros A (hosts)

Añade los siguientes registros **A** dentro de la zona:

| Nombre | Dirección IP                      |
| ------ | --------------------------------- |
| `ns1`  | IP del servidor DNS               |
| `pc1`  | IP de un equipo cliente           |
| `www`  | IP de un equipo (real o ficticio) |

📌 **Importante**

* El registro `ns1` debe apuntar a la IP real del servidor DNS.
* Los demás registros deben ser coherentes con la red interna.

📎 *Captura reutilizable de tu guion ampliado:*

```md
![alt text](51.png)
```

*(Alta de registros A)*

---

## 4. Creación de un alias (CNAME)

Crea un registro **CNAME** con las siguientes características:

* Alias:

  ```
  web
  ```
* Nombre canónico:

  ```
  www.ejemplo.local
  ```

📎 *Captura reutilizable de tu guion ampliado:*

```md
![alt text](image-1.png)
```

*(Creación de registro CNAME)*

---

## 5. Comprobación de la resolución desde un cliente

Desde un equipo cliente de la red interna, comprueba la resolución de los siguientes nombres:

* `ns1.ejemplo.local`
* `pc1.ejemplo.local`
* `web.ejemplo.local`

Utiliza herramientas de consulta vistas en sesiones anteriores (`ping`, `nslookup`).

Responde:

* ¿Qué nombres se resuelven correctamente?
* ¿Qué dirección IP devuelve cada uno?

📌 No es necesario que todos los nombres respondan a `ping`; lo importante es la **resolución DNS**.

---

## Instrucciones de entrega

* **Esta práctica es obligatoria**, pero **NO se entrega de forma independiente**.
* Forma parte de la **memoria final de la UT**, junto con las prácticas siguientes.
* Debes:

  * Guardar las capturas que se indiquen.
  * Responder por escrito a las preguntas planteadas.
  * Mantener el material ordenado para la entrega final.

---

## Qué se evalúa en esta práctica

* Creación correcta de una zona directa.
* Comprensión de los registros SOA y NS.
* Coherencia en los registros A creados.
* Uso correcto de un alias CNAME.
* Capacidad para comprobar la resolución desde cliente.

---

## Idea clave que debe quedar clara

> *Configurar DNS no es “activar un servicio”,
> es definir correctamente los datos que describen una red.*
