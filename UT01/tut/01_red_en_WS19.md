# 🌐 Configurar la tarjeta de red en Windows Server 2019 (Interfaz gráfica)

### 1. Abrir el Administrador de Servidores

* Inicia sesión en tu Windows Server 2019.
* Al arrancar, normalmente se abre el **Administrador del Servidor** (Server Manager).
* Si no aparece, ábrelo desde el menú Inicio.

---

### 2. Ir a la Configuración de Red

1. En el **Administrador del Servidor**, arriba a la derecha haz clic en **Local Server** (Servidor local).
2. Busca el apartado **Ethernet** (o el nombre de la tarjeta de red).
3. Haz clic en el enlace azul con el nombre de la conexión.

---

### 3. Abrir Propiedades de la Red

1. Se abrirá la ventana **Network Connections** (Conexiones de red).
2. Haz **clic derecho** sobre la tarjeta de red que quieras configurar → **Properties** (Propiedades).

---

### 4. Seleccionar Protocolo IPv4

1. En la lista, busca **Internet Protocol Version 4 (TCP/IPv4)**.
2. Selecciónalo y haz clic en **Properties** (Propiedades).

---

### 5. Configurar IP Fija o Automática

* **Si quieres usar DHCP (automático):**
  Marca la opción:

  * *Obtain an IP address automatically*
  * *Obtain DNS server address automatically*

* **Si quieres poner IP fija (manual):**
  Marca *Use the following IP address* y rellena:

  * **IP address:** (ej. 192.168.1.100)
  * **Subnet mask:** (ej. 255.255.255.0)
  * **Default gateway:** (ej. 192.168.1.1)
  * **Preferred DNS server:** (ej. 8.8.8.8 o el del servidor local)

👉 Haz clic en **OK** → **Close**.

---

### 6. Comprobar Conexión

1. Abre una consola (cmd o PowerShell).

2. Escribe:

   ```powershell
   ipconfig
   ```

   Aquí verás la IP configurada.

3. Para comprobar conectividad, prueba con:

   ```powershell
   ping 8.8.8.8
   ```
---

✅ Con esto la tarjeta de red ya queda configurada.

