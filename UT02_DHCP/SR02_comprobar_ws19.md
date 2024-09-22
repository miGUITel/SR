Para comprobar el correcto funcionamiento de un servidor DHCP en un entorno **VirtualBox**, puedes crear una red virtual con varias máquinas virtuales (VMs), donde el servidor DHCP asignará direcciones IP a los clientes automáticamente. Aquí te propongo un esquema de red y los pasos para llevarlo a cabo.

### **Esquema de Red Propuesto:**

1. **Servidor DHCP (Windows Server 2019)**: Este será el servidor que se encargue de asignar las direcciones IP a los clientes.
2. **Clientes DHCP**: Múltiples máquinas virtuales que recibirán direcciones IP dinámicas del servidor DHCP.
3. **Red Interna**: Utiliza una red virtual de VirtualBox llamada "Red Interna" para aislar y gestionar las máquinas en una misma red sin acceso externo.

### **Pasos para Configurar y Verificar el DHCP en VirtualBox**

#### **Paso 1: Configuración de la Red en VirtualBox**
1. **Crear una red interna**:
   - En VirtualBox, ve a **Archivo > Preferencias > Red**.
   - En la pestaña de **Redes Internas**, crea una nueva red (por ejemplo, llámala `Red-DHCP`) y asigna el mismo nombre a todas las VMs que estarán en esta red.

2. **Configurar las máquinas virtuales**:
   - Para cada máquina virtual (tanto el servidor DHCP como los clientes), ve a la configuración de la VM en **Configuración > Red**.
   - En **Adaptador 1**, selecciona el **Modo adaptador** como **Red Interna** y elige la red que creaste anteriormente (`Red-DHCP`).
   - Asegúrate de que todas las máquinas (servidor y clientes) están en la misma red interna.

#### **Paso 2: Configuración del Servidor DHCP**
1. **Iniciar el Servidor DHCP (Windows Server 2019)**:
   - Asegúrate de que el rol **DHCP** está instalado y configurado como describimos en los pasos anteriores.
   - Configura un ámbito DHCP que cubra el rango de IPs que los clientes utilizarán. Por ejemplo:
     - Rango de direcciones IP: `192.168.100.10 - 192.168.100.100`
     - Puerta de enlace (opcional si no hay router en esta red virtual): `192.168.100.1`
   - El servidor DHCP debe estar activo y autorizado (si estás en un dominio).

2. **Asignar una IP estática al servidor**:
   - En la **Configuración de red** del servidor DHCP, asigna una dirección IP fija dentro del mismo rango que configurarás en el ámbito (fuera del rango dinámico). Por ejemplo: `192.168.100.5`.

#### **Paso 3: Configuración de Clientes DHCP (máquinas virtuales)**
1. **Iniciar las máquinas clientes**:
   - Puedes crear varias máquinas virtuales con diferentes sistemas operativos (Windows, Linux, etc.) para simular diversos clientes.
   
2. **Configurar los adaptadores de red de los clientes**:
   - Configura las interfaces de red de los clientes para obtener una IP automáticamente mediante DHCP. 
     - En Windows, ve a **Configuración de red** y selecciona **Obtener una dirección IP automáticamente**.
     - En Linux, edita la configuración de red para usar **DHCP**.

#### **Paso 4: Verificar el Funcionamiento del Servidor DHCP**
1. **Obtener IP en los Clientes**:
   - Inicia cada cliente y revisa si reciben automáticamente una dirección IP dentro del rango configurado en el ámbito DHCP.
   - En **Windows**, abre una terminal y ejecuta el comando `ipconfig` para verificar la dirección IP asignada.
   - En **Linux**, usa `ifconfig` o `ip addr`.

2. **Comprobar en el Servidor DHCP**:
   - Abre la consola DHCP en el servidor (usa `dhcpmgmt.msc`).
   - Ve al ámbito que configuraste y revisa la sección de **Concesiones de direcciones**. Aquí deberías ver las direcciones IP que el servidor DHCP ha asignado a los clientes.

3. **Prueba de Conectividad**:
   - Realiza una prueba de conectividad entre el servidor y los clientes:
     - Desde un cliente, ejecuta el comando `ping 192.168.100.5` (la IP del servidor DHCP). Debería haber respuesta.
     - También puedes probar a hacer ping desde el servidor a uno de los clientes.

#### **Paso 5: Solución de Problemas**
1. **Cliente no recibe IP**:
   - Asegúrate de que el adaptador de red está configurado para recibir una dirección IP automáticamente (en lugar de estática).
   - Verifica que el servidor DHCP esté activo y funcionando correctamente.
   - Asegúrate de que todas las máquinas virtuales están en la **misma red interna**.

2. **Error en la concesión de direcciones**:
   - Verifica el rango de IPs asignadas en el ámbito DHCP.
   - Asegúrate de que no haya conflictos de IPs o exclusiones incorrectas.
   - Comprueba la conectividad de red entre el servidor DHCP y los clientes (puedes usar comandos como `ping`).

### **Conclusión**

Con este esquema de red y utilizando VirtualBox, puedes simular un entorno controlado en el que el servidor DHCP asignará direcciones IP a los clientes dentro de una red interna. Siguiendo estos pasos, podrás verificar que tu servidor DHCP está funcionando correctamente y gestionando las direcciones IP de los clientes.
