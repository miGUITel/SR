## 🧪 **Práctica 2 OPCIONAL — Funcionamiento básico de la resolución DNS desde el cliente**

### Objetivo

Visualizar el proceso de resolución de nombres, comprobar el uso de la **caché DNS del cliente** e interpretar los **distintos tipos de respuesta** que devuelve `nslookup`.

---

## Herramientas y comandos que se utilizarán

* `ipconfig /displaydns`
* `ipconfig /flushdns`
* `ping`
* `nslookup`

---

## Tareas a realizar (en orden)

### 1. Consultar la caché DNS inicial

Ejecuta en la consola:

```powershell
ipconfig /displaydns
```

**Qué observar**

* Aparecen entradas aunque no hayas hecho ninguna consulta manual.
* El sistema ya ha resuelto nombres previamente.

---

### 2. Limpiar la caché DNS

Ejecuta:

```powershell
ipconfig /flushdns
```

**Qué observar**

* El sistema muestra un mensaje confirmando que la caché se ha vaciado correctamente.

---

### 3. Volver a consultar la caché DNS

Ejecuta de nuevo:

```powershell
ipconfig /displaydns
```

**Qué observar**

* La caché está vacía o contiene muchas menos entradas.
* A partir de ahora, cualquier entrada nueva será consecuencia directa de tus acciones.

---

### 4. Forzar una resolución de nombre

Ejecuta:

```powershell
ping www.google.com
```

**Qué observar**

* Antes de enviar paquetes, el sistema resuelve el nombre a una dirección IP.
* El éxito o fallo del ping no es lo importante, sino la resolución del nombre.

---

### 5. Consultar la caché DNS tras la resolución

Ejecuta:

```powershell
ipconfig /displaydns
```

**Qué observar**

* Aparecen nuevas entradas relacionadas con `google.com`.
* Se confirma que el sistema **almacena respuestas DNS en caché**.

---

### 6. Repetir la resolución

Ejecuta otra vez:

```powershell
ping www.google.com
```

**Qué observar**

* La resolución suele ser inmediata.
* El sistema reutiliza la información almacenada en caché.

---

### 7. Consulta DNS explícita con `nslookup`

Ejecuta:

```powershell
nslookup www.google.com
```

---

## Interpretación de los resultados de `nslookup` (tal como aparecen en consola)

### 🔹 Respuesta **no autoritativa** (la más habitual)

En la salida aparece una línea como esta:

```text
Respuesta no autoritativa:
Nombre:    www.google.com
Addresses: 142.250.184.36
```

📌 **Dónde fijarse**

* En la línea **“Respuesta no autoritativa”**.
* Significa que el servidor que responde **no es responsable directo del dominio**, solo actúa como intermediario.

---

### 🔹 Respuesta **autoritativa**

No aparece el texto “Respuesta no autoritativa”.

Ejemplo típico:

```text
Nombre:    servidor.ejemplo.local
Address:   192.168.1.10
```

📌 **Dónde fijarse**

* Si **NO aparece** “Respuesta no autoritativa”, la respuesta es **autoritativa**.
* El servidor DNS consultado **es responsable de esa zona**.

---

### 🔹 El nombre no existe

La consola muestra algo como:

```text
*** servidorDNS no encuentra www.ejemploqueNOexiste.com: Non-existent domain
```

📌 **Dónde fijarse**

* En el texto **“no encuentra”** o **“Non-existent domain”**.
* El DNS responde correctamente, pero **el nombre no está registrado**.

---

### 🔹 Tiempo de espera agotado

Ejemplo:

```text
Tiempo de espera agotado.
```

o

```text
*** Se agotó el tiempo de espera de la solicitud.
```

📌 **Dónde fijarse**

* Mensaje de **timeout**.
* El cliente **no ha podido contactar con el servidor DNS configurado**.

---

## Entrega solicitada (EN PDF)

* Captura de `ipconfig /displaydns` antes y después de limpiar la caché.
* Captura de la caché tras el primer `ping`.
* Captura del resultado de `nslookup`.
* Explicación breve (2–3 líneas) del proceso completo.

---

### Idea clave que debe quedar clara

> *Cuando escribes un nombre, el sistema consulta un DNS,
> guarda la respuesta en caché y la reutiliza mientras sea válida.*
>

Feedback:
Aunque ping no funcione (ping www.google.com) sí que realiza la petición al DNS y sí queda registrado en la caché del DNS
Comprobar porqué en algunas máquinas la caché se llena justo después de flush (parece que tiene que ver con el archivo HOSTS)