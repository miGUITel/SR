Este manual cubrirá la instalación y configuración del servicio DHCP (Dynamic Host Configuration Protocol) en Windows Server 2019, utilizando **PowerShell** y **Administrador del servidor**.

### **Índice**
1. Instalación de DHCP con PowerShell
2. Instalación de DHCP con el Administrador del servidor
3. Configuración del DHCP
   - Creación de un ámbito (scope)
   - Configuración de exclusiones y reservas
   - Activación del ámbito

---

## 1. Instalación de DHCP con PowerShell

### **Paso 1: Abrir PowerShell como administrador**
- Ve al **Menú Inicio** y busca **Windows PowerShell**.
- Haz clic derecho en **Windows PowerShell** y selecciona **Ejecutar como administrador**.

### **Paso 2: Comando de instalación**
Ejecuta el siguiente comando para instalar el rol DHCP:

```powershell
Install-WindowsFeature -Name DHCP -IncludeManagementTools
```

Esto instalará tanto el servicio DHCP como las herramientas de administración necesarias.

### **Paso 3: Autorizar el servidor DHCP en Active Directory**
Si tu servidor forma parte de un dominio, es necesario autorizarlo en **Active Directory**:

```powershell
Add-DhcpServerInDC -DnsName "NombreDelServidor" -IpAddress "DirecciónIPDelServidor"
```

Reemplaza `"NombreDelServidor"` y `"DirecciónIPDelServidor"` con los valores correspondientes.

### **Paso 4: Verificar la instalación**
Puedes verificar que el servicio DHCP se ha instalado correctamente con el siguiente comando:

```powershell
Get-WindowsFeature -Name DHCP*
```

Deberías ver que el servicio está instalado y habilitado.

---

## 2. Instalación de DHCP con el Administrador del Servidor

### **Paso 1: Abrir el Administrador del servidor**
- Ve al **Menú Inicio** y selecciona **Administrador del servidor**.

### **Paso 2: Agregar roles y características**
- En la ventana del Administrador del servidor, selecciona **Administrar** (esquina superior derecha) y elige **Agregar roles y características**.
- Haz clic en **Siguiente** hasta llegar a la selección del **Tipo de instalación** y selecciona **Instalación basada en características o en roles**.

### **Paso 3: Selección del servidor**
- En la ventana de **Selección de servidor**, elige el servidor donde deseas instalar el rol DHCP y haz clic en **Siguiente**.

### **Paso 4: Selección del rol DHCP**
- En la lista de roles, marca la opción **Servidor DHCP**.
- Se abrirá una ventana para agregar características necesarias, haz clic en **Agregar características** y luego en **Siguiente**.

### **Paso 5: Instalación**
- Haz clic en **Instalar** y espera a que se complete el proceso. Al final, tendrás la opción de **Cerrar**.

### **Paso 6: Configurar el rol DHCP**
- Tras la instalación, aparecerá una ventana para **completar la configuración DHCP**.
- Haz clic en **Completar la configuración DHCP** y luego selecciona **Autorizar el servidor**.

---

## 3. Configuración detallada del DHCP

Una vez instalado el rol de DHCP, es necesario configurar un **ámbito** (scope), que define el rango de direcciones IP que se asignarán automáticamente a los dispositivos en la red. Aquí te proporciono una guía paso a paso, incluyendo la creación de un ámbito, la configuración de **exclusiones** y **reservas** con ejemplos detallados.

#### **Paso 1: Abrir la Consola DHCP**
1. Abre la consola DHCP desde el **Administrador del Servidor** o usando el comando `dhcpmgmt.msc` en el cuadro de búsqueda de **Inicio**.
2. En la ventana de la consola, expande el nodo de tu servidor en el panel izquierdo.
3. Verás dos secciones principales: **IPv4** y **IPv6**. Para este ejemplo, trabajaremos con **IPv4**.

#### **Paso 2: Crear un Ámbito (Scope)**
El ámbito define el rango de direcciones IP que el servidor DHCP puede asignar a los clientes.

1. **Clic derecho en "IPv4"** y selecciona **Nuevo Ámbito**.
2. Aparecerá el **Asistente para Nuevo Ámbito**. Sigue los pasos del asistente:

   - **Nombre del Ámbito**: Asigna un nombre descriptivo a tu ámbito, por ejemplo, **Red-Oficina**.
   - **Descripción**: Añade una descripción si lo deseas, como "Ámbito para red interna de la oficina".
   
3. **Rango de Direcciones IP**: 
   - Define el rango de direcciones IP que el servidor DHCP asignará automáticamente. Por ejemplo:
     - **IP de inicio**: `192.168.1.100`
     - **IP de fin**: `192.168.1.200`
   - Esto significa que las direcciones entre `192.168.1.100` y `192.168.1.200` se asignarán a los clientes.
   - **Longitud de la máscara de subred**: Si estás usando una red estándar de clase C, la máscara será `255.255.255.0`. Si la red es diferente, ajusta la máscara de subred según sea necesario.

4. **Agregar Exclusiones**: Aquí puedes excluir un rango de direcciones IP que no deseas que el servidor DHCP asigne automáticamente. Las exclusiones son útiles para dispositivos con direcciones IP estáticas, como servidores o impresoras.
   
   - Por ejemplo, si tienes servidores o impresoras en el rango de `192.168.1.1` a `192.168.1.10`, puedes excluirlas.
   - Introduce las exclusiones como un rango. En este ejemplo, excluiremos las direcciones `192.168.1.1` a `192.168.1.10`:
     - **Inicio de exclusión**: `192.168.1.1`
     - **Fin de exclusión**: `192.168.1.10`
   
   Esto evitará que el servidor DHCP asigne estas IPs a otros dispositivos.

5. **Duración de las concesiones**: Define cuánto tiempo una dirección IP será asignada a un cliente antes de expirar. La configuración por defecto es 8 días, pero puedes ajustarla según tus necesidades.
   
   - Para un entorno con clientes estables, el valor predeterminado de 8 días es generalmente adecuado.
   - En redes con muchos dispositivos que se conectan y desconectan (por ejemplo, redes Wi-Fi de oficinas), podrías reducir la duración a algo más corto, como 1 día.

#### **Paso 3: Configuración de la Puerta de Enlace (Gateway) y DNS**
1. **Puerta de Enlace**: Si los dispositivos de tu red necesitan acceso a otras redes (incluyendo Internet), debes especificar la dirección IP del router o gateway. Introduce la IP del gateway de la red, por ejemplo:
   - **Puerta de enlace**: `192.168.1.1`
   
2. **Servidores DNS**: Para permitir que los clientes resuelvan nombres de dominio, introduce las direcciones IP de tus servidores DNS:
   - Si usas un DNS interno, puedes poner la IP de tu servidor DNS interno, por ejemplo, `192.168.1.5`.
   - Si no tienes un DNS interno, puedes utilizar servidores DNS públicos como:
     - **Google DNS**: `8.8.8.8` y `8.8.4.4`
     - **Cloudflare DNS**: `1.1.1.1`

#### **Paso 4: Configurar Reservas**
Las reservas en DHCP permiten asignar direcciones IP fijas a dispositivos específicos basados en su **dirección MAC**. Esto es útil para asegurar que ciertos dispositivos, como impresoras o servidores, siempre reciban la misma IP.

1. **Clic derecho en "Reservas"** bajo el ámbito que creaste y selecciona **Nueva Reserva**.
2. Introduce la siguiente información:
   - **Nombre de la reserva**: Un nombre descriptivo para identificar el dispositivo (por ejemplo, "Servidor-Archivos").
   - **Dirección IP**: La IP que deseas reservar para este dispositivo, por ejemplo, `192.168.1.101`.
   - **Dirección MAC**: Introduce la dirección MAC del dispositivo. Puedes obtenerla ejecutando `ipconfig /all` en el dispositivo o `ifconfig` en Linux.
   - **Descripción**: Puedes agregar una breve descripción del dispositivo, por ejemplo, "Servidor de archivos de la oficina".
   
3. Haz clic en **Agregar** para crear la reserva. Ahora, cada vez que este dispositivo se conecte a la red, recibirá la dirección IP reservada.

#### **Paso 5: Activar el Ámbito**
Una vez que has configurado el ámbito, es necesario **activarlo** para que el servidor DHCP comience a asignar direcciones a los clientes.

1. En la consola DHCP, haz clic derecho sobre el ámbito recién creado y selecciona **Activar**.
2. El servidor DHCP ahora está listo para asignar direcciones IP a los dispositivos en la red.

#### **Ejemplo Completo de Configuración**

- **Ámbito**: `192.168.1.100 - 192.168.1.200`
- **Exclusiones**: `192.168.1.1 - 192.168.1.10` (servidores, impresoras)
- **Reserva**:
   - Nombre: **Servidor-Archivos**
   - Dirección IP reservada: `192.168.1.101`
   - Dirección MAC: `00-1A-2B-3C-4D-5E`
- **Puerta de enlace**: `192.168.1.1`
- **DNS**: `8.8.8.8`, `1.1.1.1`
- **Duración de la concesión**: 8 días

---


**Tip:** Toma capturas de pantalla en las etapas críticas, como la creación del ámbito y la configuración de exclusiones, para futuras referencias o documentación interna.
