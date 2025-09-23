Genial 👍, ahora te hago la misma guía pero para **Ubuntu Desktop (22.04 o similar)** usando **solo interfaz gráfica**.

---

# 🌐 Configurar la tarjeta de red en Ubuntu Desktop (Interfaz gráfica)

### 1. Abrir la configuración de red

* Haz clic en el **icono de red** (arriba a la derecha en la barra superior).
* Selecciona **Configuración de red** (*Settings → Network*).

---

### 2. Seleccionar la interfaz

* En el panel lateral izquierdo, elige **Red** (*Network*).
* Verás tus adaptadores:

  * **Cableada (Wired)** para conexión por cable.
  * **Wi-Fi** si estás en inalámbrica.
* Haz clic en el **engranaje ⚙️** de la conexión que quieras configurar.

---

### 3. Configurar IPv4

1. Se abre una ventana con varias pestañas.
2. Entra en la pestaña **IPv4**.
3. Elige uno de los modos:

* **Automático (DHCP)**
  Selecciona *Automático (DHCP)* → Guarda.

* **Manual (IP fija)**
  Selecciona *Manual* y rellena:

  * **Dirección:** (ej. 192.168.1.50)
  * **Máscara de red:** (ej. 24 para 255.255.255.0)
  * **Puerta de enlace (Gateway):** (ej. 192.168.1.1)
  * **DNS:** (ej. 8.8.8.8 o el de tu servidor)

👉 Haz clic en **Añadir** → **Guardar**.

---

### 4. Aplicar cambios

* Desactiva y vuelve a activar la conexión desde el botón de encendido/apagado al lado de “Cableada/Wi-Fi”.
* También puedes desconectar y reconectar el cable.

---

### 5. Comprobar conexión

1. Abre una **terminal** (Ctrl+Alt+T).

2. Escribe:

   ```bash
   ip addr show
   ```

   Aquí verás la IP configurada.

3. Para comprobar conectividad:

   ```bash
   ping 8.8.8.8
   ```

---

✅ Con esto la tarjeta de red queda configurada en Ubuntu Desktop.
