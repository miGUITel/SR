Perfecto. Aquí tienes el **guion completo de la práctica de servidor secundario y transferencia de zona**, numerada como **Práctica 7**, coherente con todo lo anterior y pensada para **SMR**, en red interna y sin dependencias externas.

---

# 🧪 **Práctica 7 — Servidor DNS secundario y transferencia de zona**

**Módulo:** Servicios en Red
**Unidad de Trabajo:** UT03 – DNS
**Sesión:** 7

---

## Objetivo de la práctica

Configurar un **servidor DNS secundario** y comprobar la **transferencia de zona** desde el servidor primario, entendiendo cómo DNS proporciona **redundancia** y evita puntos únicos de fallo.

---

## Idea clave de la práctica

> *Un DNS secundario no pregunta a otro servidor:
> resuelve porque tiene una copia de la zona.*

---

## Escenario de trabajo

* **DNS1**

  * Windows Server 2019
  * Zona directa principal: `ejemplo.local`
  * Servidor DNS **primario**

* **DNS2**

  * Windows Server 2019
  * Servidor DNS **secundario**

* Ambos servidores:

  * En la **misma red interna**.
  * Con IP fija.
  * Sin conexión a Internet.

* El cliente:

  * Puede consultar indistintamente a DNS1 o DNS2 (según se indique).

---

## Tareas a realizar

---

## 1. Situación inicial

Comprueba que:

* DNS1 resuelve correctamente los nombres de la zona `ejemplo.local`.
* DNS2 **no tiene** todavía ninguna zona configurada.

Esto confirma que:

* DNS1 es el único servidor autoritativo en este momento.
* DNS2 aún no dispone de información DNS.

---

## 2. Configuración de la transferencia de zona en DNS1

En el **Administrador DNS** de **DNS1**:

1. Abre la zona directa `ejemplo.local`.
2. Clic derecho → **Propiedades**.
3. Pestaña **Transferencias de zona**:

   * Marca **Permitir transferencias de zona**.
   * Selecciona **Solo a los servidores de nombres**
     *(o especifica explícitamente la IP de DNS2)*.
4. Acepta los cambios.

📌 Con esto autorizas a DNS2 a recibir la zona.

---

## 3. Creación de la zona secundaria en DNS2

En el **Administrador DNS** de **DNS2**:

1. Clic derecho en **Zonas de búsqueda directa** → **Nueva zona…**
2. Tipo de zona: **Zona secundaria**.
3. Nombre de la zona: `ejemplo.local`.
4. Servidor maestro:

   * Introduce la **IP de DNS1**.
5. Finaliza el asistente.

📌 DNS2 solicitará automáticamente la transferencia de zona a DNS1.

---

## 4. Comprobación de la transferencia de zona

En **DNS2**, comprueba que:

* La zona `ejemplo.local` aparece creada.
* Los registros **A**, **CNAME** (y otros existentes) aparecen automáticamente.
* **No se han creado manualmente**.

Esto confirma que:

* La transferencia de zona se ha realizado correctamente.
* DNS2 tiene una **copia de la información DNS**.

---

## 5. Comprobación de resolución desde ambos servidores

Desde un cliente o desde los propios servidores, realiza consultas:

### Consulta al DNS primario

```cmd
nslookup pc1.ejemplo.local <IP_DNS1>
```

### Consulta al DNS secundario

```cmd
nslookup pc1.ejemplo.local <IP_DNS2>
```

Comprueba que:

* Ambos servidores resuelven correctamente.
* Ambos son **autoritativos** para la zona.

---

## 6. Reflexión guiada

Responde brevemente:

* ¿En qué se diferencia un reenviador de un servidor secundario?
* ¿Por qué el secundario no necesita reenviar consultas?
* ¿Qué ocurriría si el servidor primario dejara de funcionar?

---

## Instrucciones de entrega

* **Práctica obligatoria**.
* Forma parte de la **memoria final de la UT**.
* Guarda las capturas que se indiquen y responde a las preguntas.
* No se entrega de forma independiente.

---

## Qué se evalúa en esta práctica

* Configuración correcta de transferencias de zona.
* Creación adecuada de una zona secundaria.
* Comprobación de la resolución desde ambos servidores.
* Comprensión del concepto de redundancia DNS.
* Capacidad de diferenciar:

  * reenviador,
  * servidor secundario.

---

## Errores comunes que debes evitar

* Crear registros manualmente en el servidor secundario.
* Confundir reenvío con transferencia de zona.
* Pensar que el secundario “pregunta” al primario cada vez.

---

## Idea final que debe quedar clara

> *La alta disponibilidad en DNS se basa en compartir información,
> no en reenviar consultas.*

---

Con esta práctica queda **cerrada la UT de DNS**, cubriendo todos los criterios del **RA 2** de forma progresiva, clara y completamente alineada con SMR.


## 7. Sincronización de cambios y tiempo de actualización del secundario

En esta parte se comprobará que el **servidor secundario no se actualiza de forma inmediata**, sino según los **temporizadores definidos en el registro SOA**.

### 7.1 Ajuste del tiempo de actualización (SOA – Refresh)

En **DNS1 (servidor primario)**:

1. Abre el **Administrador DNS**.
2. Accede a la zona `ejemplo.local`.
3. Clic derecho sobre la zona → **Propiedades**.
4. Pestaña **SOA**.
5. Modifica el valor **Refresh** a un tiempo corto (por ejemplo, **1 o 5 minutos**).
6. Acepta los cambios.

> Este valor indica cada cuánto tiempo el servidor secundario comprueba si hay cambios en la zona.

---

### 7.2 Modificación de un registro en el primario

En **DNS1**, realiza **un único cambio visible** en la zona, por ejemplo:

* Cambiar la dirección IP de un registro A existente, **o**
* Añadir un nuevo registro A sencillo.

No realices ningún cambio en DNS2.

---

### 7.3 Comprobación en el servidor secundario

En **DNS2**:

1. Comprueba inmediatamente la zona `ejemplo.local`.

   * El cambio **no debe aparecer todavía**.
2. Espera el tiempo configurado en **Refresh**.
3. Vuelve a comprobar la zona.

Verifica que:

* El cambio **aparece automáticamente** en DNS2.
* No ha sido necesario crear ni modificar nada manualmente.

---

### 7.4 Reflexión guiada

Responde brevemente:

* ¿Por qué el secundario no se actualiza de forma inmediata?
* ¿Dónde se define el tiempo de actualización?
* ¿Qué ventaja tiene este comportamiento en un servicio como DNS?

---

## Idea clave que debe quedar clara

> *El servidor secundario no copia los cambios continuamente:
> se sincroniza según los tiempos definidos en el DNS.*

---

Con este bloque, la práctica queda **cerrada de forma excelente**: no solo hay redundancia, sino **comprensión real del funcionamiento temporal del DNS**.
