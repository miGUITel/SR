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
- **Unified Communications Managed API (UCMA) 4.0**.
- **Módulo de reescritura de URL de IIS**.

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
8. **Cuando se te solicite el nombre de la organización de Exchange, puedes elegir cualquier nombre.** No es obligatorio que sea el mismo que el dominio de Active Directory, pero se recomienda que sea algo descriptivo y fácil de identificar, ya que este nombre **no podrá cambiarse después de la instalación**.
9. **Verifica que el servidor está unido al dominio antes de continuar.**

11. **Espera a que se complete la instalación**.

---

## **Conclusión**
Esta guía ahora incluye la instalación de Active Directory, la promoción del servidor a controlador de dominio y la configuración del servidor DNS, además de la instalación de todos los requisitos previos necesarios para Exchange Server 2019 en un entorno de prueba.

