Para que el servidor DHCP y el cliente se comuniquen entre ellos dentro de **VirtualBox** sin afectar al anfitrión ni a otras redes, la opción más adecuada es configurar ambas máquinas virtuales en una **red interna (Internal Network)**. Esto crea una red aislada donde solo las máquinas virtuales en esa red interna específica pueden comunicarse entre sí. El anfitrión no verá ni será afectado por esta red.

A continuación te explico los pasos para configurar las máquinas virtuales en VirtualBox para este ejercicio:

### 1. **Configuración del servidor DHCP en VirtualBox**

#### Paso 1: Acceder a la configuración de red de la máquina virtual
- Abre **VirtualBox**.
- Selecciona la máquina virtual que actuará como **servidor DHCP**.
- Haz clic en **Configuración** (Settings).
- En la pestaña **Red** (Network), verás varias adaptadores de red.

#### Paso 2: Configurar el adaptador de red para la red interna
- En el **Adaptador 1**, selecciona la opción **Habilitar adaptador de red** (Enable Network Adapter).
- En el **Modo de conexión** (Attached to), selecciona **Red interna** (Internal Network).
- En el campo **Nombre**, deja el valor predeterminado (por ejemplo, *intnet*) o introduce un nombre específico, como "red-dhcp". Este nombre será compartido con la máquina cliente para que estén en la misma red interna.

#### Paso 3: Confirmar los cambios
- Haz clic en **Aceptar** para guardar la configuración.

### 2. **Configuración del cliente DHCP en VirtualBox**

#### Paso 1: Configurar la máquina cliente
- Repite el proceso para la máquina virtual que actuará como **cliente**.
- Abre **Configuración** (Settings) y ve a la pestaña **Red**.
- En el **Adaptador 1**, selecciona **Red interna** (Internal Network).
- Asegúrate de que el campo **Nombre** tenga el mismo nombre de red que configuraste en el servidor (por ejemplo, "red-dhcp").

#### Paso 2: Confirmar los cambios
- Haz clic en **Aceptar** para guardar la configuración.

### 3. **Verificación de la red interna**

Ambas máquinas virtuales ahora están en una red interna. Esto significa que solo ellas pueden comunicarse entre sí, y no tienen acceso a internet ni a la red del anfitrión.

- El **servidor DHCP** proporcionará direcciones IP a los clientes dentro de esta red interna.
- El **cliente DHCP** solicitará y obtendrá su configuración de red a través de esta conexión.

### 4. **Comprobación de conectividad**

1. **Inicia ambas máquinas virtuales** (servidor y cliente) en VirtualBox.
2. Verifica que el cliente obtenga una dirección IP del servidor DHCP.

   En el cliente, ejecuta:
   ```bash
   ip a   # En Linux
   ipconfig   # En Windows
   ```

3. En el servidor, revisa los logs para verificar que ha respondido correctamente a la solicitud DHCP del cliente.

   En el servidor Linux, puedes verificar los logs con:
   ```bash
   sudo tail -f /var/log/syslog
   ```

Si ambas máquinas se comunican correctamente, el cliente debería obtener una dirección IP dentro del rango definido en tu configuración del servidor DHCP.

### **Opcional: Red interna con NAT**
Si en algún momento quieres que las máquinas tengan acceso a internet (sin afectar al anfitrión), puedes configurar un segundo adaptador en modo **NAT** en cada máquina virtual. Así, las máquinas seguirán comunicándose a través de la red interna, pero tendrán un adaptador adicional para acceder a internet.

- **Adaptador 1**: Red interna (para comunicación DHCP).
- **Adaptador 2**: NAT (para acceso a internet).

Con esta configuración, las máquinas podrán interactuar entre sí y, si es necesario, acceder a internet de manera controlada sin interferir con el sistema anfitrión.

