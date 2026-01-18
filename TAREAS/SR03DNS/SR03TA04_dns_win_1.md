Módulo: Servicios en Red  
Unidad de Trabajo: UT03 DNS  
Tarea 04  

**Título de la Tarea:**  
**Instalación y Configuración de DNS en Windows Server**  

**Descripción de la Tarea:**  
En esta tarea, deberás realizar la instalación y configuración de un servidor DNS en un entorno de Windows Server, utilizando PowerShell y VirtualBox. Asegúrate de configurar las redes y las direcciones MAC según las instrucciones. Sigue los pasos detallados a continuación:

1. **Configurar red en modo NAT en la máquina maestra:**  
   - Utiliza VirtualBox para configurar la red en modo NAT: 
     1. Selecciona la máquina virtual en VirtualBox.
     2. Haz clic en "Configuración" > "Red" > "Adaptador 1" y selecciona "NAT".

2. **Instalar el servicio DNS en la máquina maestra:**  
   - Utiliza PowerShell para instalar el servicio DNS:  
     ```powershell
     Install-WindowsFeature -Name DNS -IncludeManagementTools
     ```

3. **Configurar IP estática y DNS en la máquina maestra:**  
   - Configuración gráfica en Windows Server 2019:
     1. Abre el "Centro de redes y recursos compartidos". Puedes acceder desde el "Panel de control" o haciendo clic derecho en el icono de red en la barra de tareas y seleccionando "Abrir configuración de red e Internet".
     2. Haz clic en "Cambiar configuración del adaptador".
     3. Haz clic derecho en la conexión de red (normalmente llamada "Ethernet") y selecciona "Propiedades".
     4. Selecciona "Protocolo de Internet versión 4 (TCP/IPv4)" y haz clic en "Propiedades".
     5. Marca la opción "Usar la siguiente dirección IP" e ingresa:
        - Dirección IP: 192.168.1.10
        - Máscara de subred: 255.255.255.0
        - Puerta de enlace predeterminada: 192.168.1.1
     6. En la sección de "Servidor DNS preferido", ingresa la dirección IP del servidor DNS (192.168.1.10).

   - Configura una IP estática utilizando PowerShell:  
     ```powershell
     New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.10 -PrefixLength 24 -DefaultGateway 192.168.1.1
     ```
   - Configura el servidor DNS:  
     ```powershell
     Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.1.10
     ```

4. **Cambiar a red interna y configurar modo promiscuo:**  
   - Cambia la configuración de la red de la máquina maestra a "red interna":
     1. En VirtualBox, selecciona la máquina maestra.
     2. Haz clic en "Configuración" > "Red" > "Adaptador 1" y selecciona "Red Interna".
   - Configura el modo promiscuo:
     1. En "Avanzado", selecciona "Permitir todo" en el modo promiscuo.

5. **Clonar la máquina maestra para crear un servidor esclavo:**  
   - Utiliza VirtualBox para realizar una clonación enlazada:  
     1. Haz clic derecho en la máquina maestra y selecciona "Clonar".
     2. Elige "Clonación enlazada" y nombra la nueva máquina como "Servidor Esclavo".
     3. Asegúrate de que la red esté configurada en "red interna" y que el modo promiscuo esté habilitado.

6. **Clonar la máquina maestra para crear un servidor de subdominio:**  
   - Repite el proceso de clonación enlazada:
     1. Haz clic derecho en la máquina maestra y selecciona "Clonar".
     2. Elige "Clonación enlazada" y nombra la nueva máquina como "Servidor Subdominio".
     3. Configura la red en "red interna" y habilita el modo promiscuo.

7. **Renovar las direcciones MAC de los servidores clonados:**  
   - Utiliza VirtualBox para renovar las direcciones MAC:
     1. Selecciona cada máquina clonada.
     2. En "Configuración" > "Red" > "Adaptador 1", haz clic en el botón de renovación de MAC.

8. **Configurar IPs estáticas en los servidores clonados:**  (se puede hacer gráficamente como en el punto 3)
   - Utiliza PowerShell para asignar IPs estáticas y configurar las IPs de DNS:
     ```powershell
     New-NetIPAddress -InterfaceAlias "Ethernet" -IPAddress 192.168.1.11 -PrefixLength 24 -DefaultGateway 192.168.1.1
     Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 192.168.1.10
     ```
     (Repite el proceso para cada servidor, asignando diferentes direcciones IP dentro del rango de la red interna).

9. **Configurar la zona DNS:**  
   - Utiliza el Administrador de DNS en Windows Server para agregar los registros necesarios:
     1. Abre "Administrador de DNS".
     2. Navega hasta la zona directa del servidor y agrega registros A para los servidores esclavo y subdominio.

**Rúbrica de Evaluación:**

| Calificación | Descripción |
| --- | --- |
| 0 | No ha realizado la tarea o no ha seguido ninguno de los pasos especificados. |
| 3 | Ha realizado parcialmente la instalación y configuración inicial, pero presenta errores significativos. |
| 6 | Ha completado la instalación y configuración básica, con algunos errores menores en la asignación de IPs y DNS. |
| 9 | Ha completado correctamente todos los pasos y configuraciones solicitadas, pero sin mejoras adicionales. |
| 10 | Ha completado la tarea correctamente, incluyendo una configuración avanzada o detalle extra en la funcionalidad. |

**Instrucciones de Entrega:**  
Entrega una memoria en formato PDF que documente cada uno de los pasos realizados, incluyendo capturas de pantalla y descripciones detalladas de la configuración. Además, incluye los archivos necesarios con el código fuente y configuraciones utilizadas.

