# **Guía: Instalación y Configuración de un Servidor de Correo Exchange en Windows Server 2019 con Active Directory**

## **1. Requisitos Previos**
Antes de instalar Microsoft Exchange Server 2019, asegúrate de que cumples con los siguientes requisitos:

### **1.1. Hardware Requerido**
- **CPU:** Procesador de 64 bits compatible con Intel VT o AMD-V.
- **RAM:**
  - **Servidor de buzón:** 128 GB recomendado, 64 GB mínimo.
  - **Servidor Edge Transport:** 64 GB recomendado, 32 GB mínimo.
- **Espacio en Disco:**
  - Unidad del sistema: **30 GB mínimo**.
  - **500 MB** de espacio en la unidad que aloja la base de datos de Exchange.
  - **200 MB** en la unidad donde se almacenan los logs de Exchange.

### **1.2. Software Necesario**
- **Windows Server 2019 (Edición Estándar o Datacenter).**
- **Active Directory instalado y configurado.**
- **Certificado SSL para comunicaciones seguras (opcional, pero recomendado).**
- **.NET Framework 4.8** y **Visual C++ 2013 Redistributable Package**.
- **Windows Management Framework 5.1**.

---

## **2. Instalación y Configuración de Active Directory**
Antes de instalar Exchange Server, es necesario configurar Active Directory en el mismo servidor o en otro servidor dentro de la misma red.

### **2.1. Instalación del rol de Active Directory Domain Services (AD DS)**
1. Abre **Administrador del Servidor** y selecciona **Agregar roles y características**.
2. Selecciona **Instalación basada en características y roles** y haz clic en **Siguiente**.
3. Selecciona el servidor en el que instalar Active Directory y haz clic en **Siguiente**.
4. Marca la opción **Servicios de dominio de Active Directory (AD DS)** y haz clic en **Siguiente**.
5. Acepta las características adicionales necesarias y continúa con la instalación.
6. Una vez completada la instalación, haz clic en **Promover este servidor a controlador de dominio**.

### **2.2. Configuración de un Dominio y Promoción del Servidor a Controlador de Dominio**
1. Selecciona **Agregar un nuevo bosque** y escribe un nombre de dominio (por ejemplo, `miempresa.local`).
2. Establece un nivel funcional de dominio y bosque en **Windows Server 2016 o 2019**.
3. Introduce una contraseña para el modo de restauración de Active Directory (DSRM).
4. En la sección **Opciones de DNS**, deja la configuración predeterminada. Si aparece la alerta **"No se puede crear una delegación para este DNS porque la zona principal autoritativa no se encuentra o no ejecuta el servidor DNS de Windows"**, ignórala y continúa con la instalación. Esta alerta aparece porque el servidor DNS aún no está completamente configurado.
5. Asegúrate de que la opción **Este servidor será un catálogo global (GC)** esté seleccionada.
6. Configura los valores predeterminados para rutas de base de datos, archivos de registro y SYSVOL.
7. Haz clic en **Siguiente** y revisa la configuración.
8. Si todo está correcto, haz clic en **Instalar**.
9. El servidor se reiniciará automáticamente una vez completado el proceso.
10. Después del reinicio, verifica que el servidor está correctamente unido al dominio ejecutando:
    ```powershell
    Get-ADDomain
    ```

### **2.3. Configuración del Servidor DNS**
Active Directory requiere un servidor DNS para su correcto funcionamiento. Si el servidor DNS no se configuró automáticamente, sigue estos pasos:

1. Abre **Administrador del Servidor** y selecciona **Herramientas > DNS**.
2. En la consola DNS, verifica que existe una zona directa para `miempresa.local`.
3. Si no existe, agrégala:
   - Clic derecho en **Zonas de búsqueda directa** > **Nueva zona...**.
   - Selecciona **Zona primaria** y haz clic en **Siguiente**.
   - Introduce `miempresa.local` como nombre de la zona y completa el asistente.
4. Configura el servidor para que use su propio DNS:
   - Ve a **Panel de Control > Centro de redes y recursos compartidos > Cambiar configuración del adaptador**.
   - Clic derecho en la interfaz de red y selecciona **Propiedades**.
   - Selecciona **Protocolo de Internet versión 4 (TCP/IPv4)** y haz clic en **Propiedades**.
   - En **Usar las siguientes direcciones de servidor DNS**, introduce:
     - **Servidor DNS preferido:** `127.0.0.1` (o la IP del servidor)
     - **Servidor DNS alternativo:** (déjalo en blanco o usa otro DNS interno)
5. Verifica la resolución de nombres ejecutando en PowerShell:
   ```powershell
   nslookup miempresa.local
   ```
   Si devuelve la IP del servidor, el DNS está correctamente configurado.

---

## **2.4 Instala los requisitos previos si aún no están instalados:**
 - **.NET Framework 4.8:** Descarga desde [este enlace](https://support.microsoft.com/kb/4503548) e instálalo.
 - **UCMA 4.0:** Descarga desde [este enlace](http://go.microsoft.com/fwlink/?LinkId=260990) e instálalo.
 - **Visual C++ Redistributable 2013:** Descarga la versión **x64** desde [aquí](https://www.microsoft.com/download/details.aspx?id=40784) e instálala.
 - **Módulo de Reescritura de URL de IIS:** Descarga desde [Microsoft IIS](https://www.iis.net/downloads/microsoft/url-rewrite) e instálalo. Luego reinicia IIS con:
   ```powershell
   iisreset /restart
   ```

---

## **3. Instalación de Exchange Server 2019**

1. **Descarga el instalador de Exchange Server 2019** desde el sitio web oficial de Microsoft:  
   [https://www.microsoft.com/en-us/microsoft-365/exchange/download-exchange](https://www.microsoft.com/en-us/microsoft-365/exchange/download-exchange)
2. **Extrae los archivos** en `C:\ExchangeSetup`.
3. **Ejecuta el instalador de Exchange** (`Setup.exe`) desde `C:\ExchangeSetup`.
4. Selecciona **“Instalación de Servidor de Buzón”**.
5. Acepta los términos de licencia.
6. **Elige la ruta de instalación** (se recomienda dejar la predeterminada).
7. **Habilita la Protección contra Malware** (opcional).
8. **Verifica que el servidor está unido al dominio antes de continuar.**
9. **Espera a que se complete la instalación**.

---

## **Conclusión**
Esta guía ahora incluye la instalación de Active Directory, la promoción del servidor a controlador de dominio y la configuración del servidor DNS para garantizar el correcto funcionamiento de Exchange Server 2019 en un entorno de prueba.

