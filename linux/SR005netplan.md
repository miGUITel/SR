- [🔧 1️⃣ Comprobar red, Internet y configuración de Netplan (Ubuntu Server)](#-1️⃣-comprobar-red-internet-y-configuración-de-netplan-ubuntu-server)
  - [🚨 Si NO hay conexión a Internet](#-si-no-hay-conexión-a-internet)
  - [🧠 Idea clave](#-idea-clave)
## 🔧 1️⃣ Comprobar red, Internet y configuración de Netplan (Ubuntu Server)

1. Comprueba que el equipo tiene dirección IP:

   ```
   ip a
   ```

   * Anota la dirección IP asignada.

2. Comprueba la salida a Internet:

   ```
   ping -c 3 8.8.8.8
   ```

3. Comprueba la resolución de nombres (DNS):

   ```
   ping -c 3 google.com
   ```

---

### 🚨 Si NO hay conexión a Internet

4. Revisa la configuración de red con Netplan:

   * Muestra los archivos de configuración existentes:

     ```
     ls /etc/netplan
     ```

   * Edita el archivo correspondiente (ejemplo habitual):

     ```
     sudo nano /etc/netplan/00-installer-config.yaml
     ```

   * Comprueba que la interfaz de red está configurada en DHCP, por ejemplo:

     ```yaml
     network:
       version: 2
       ethernets:
         enp0s3:
           dhcp4: true
     ```

   > ⚠️ **Respeta estrictamente la indentación** (espacios, no tabuladores).

5. Aplica la configuración:

   ```
   sudo netplan apply
   ```

6. Vuelve a comprobar la conexión a Internet:

   ```
   ping -c 3 8.8.8.8
   ```

---

### 🧠 Idea clave

> Si un equipo no tiene acceso a Internet, **no basta con instalar programas**:
> primero hay que revisar la **configuración de red**.


Con esto:

* corriges el error conceptual IP ≠ Internet,
* introduces Netplan **solo cuando hace falta**,
* refuerzas el método profesional: **comprobar → diagnosticar → corregir**.

