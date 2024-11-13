**Guion para Práctica: Instalación y Configuración de FTP en Windows Server 2019**

## Objetivo:
Instalar y configurar un servidor FTP en Windows Server 2019, usando PowerShell para la instalación y la interfaz gráfica para la configuración. Los alumnos aprenderán a gestionar usuarios, permisos y conexiones de red para garantizar un acceso controlado.

## Pasos a seguir:

> RECUERDA CONFIGURAR LA IP FIJA DEL SERVIDOR

## **1. Instalación del Servidor FTP usando PowerShell**
   - Abrir PowerShell como administrador.
   - Ejecutar el siguiente comando para instalar IIS y el servicio FTP:
     ```powershell
     Install-WindowsFeature -name Web-FTP-Server -IncludeManagementTools
     Install-WindowsFeature -name Web-Server -IncludeManagementTools
     ```
   - Verificar que la instalación se ha completado correctamente.
  
  ![alt text](image-5.png)

## **2. Crear Usuarios y Grupos Locales (modo gráfico)**
   > Usaremos estos usuarios para probar las conexiones ftp con distintos permisos.
   - Abrir "Administración de Equipos" y navegar hasta "Usuarios y grupos locales".

![alt text](image-6.png)

   - Crear los siguientes usuarios locales necesarios para el acceso al FTP:
     - **AdminFTP**
     - **Informático1**
     - **Informático2**
     - **Contable1**
     - **Contable2**
   - Recordar: **No seleccionar que el usuario deba cambiar la contraseña al iniciar sesión**.

> **Introduce contraseñas que no vayas a olvidar...**

![alt text](image-22.png)

![alt text](image-23.png)

## **3. Crear los Grupos Locales**
   - Crear los siguientes grupos para organizar los usuarios del FTP:
     - **Informáticos** (formado por Informático1 e Informático2).
     - **Contables** (formado por Contable1 y Contable2).
   - Asignar los usuarios correspondientes a cada grupo según sus roles.

![alt text](image-9.png)

![alt text](image-10.png)

## **4. Crear el Sitio FTP a través de IIS**
   - Crear una carpeta local llamada **INICIALES_LOCAL** (donde INICIALES representa las iniciales del nombre completo del alumno, por ejemplo, FJLM_LOCAL para Francisco Javier López Mota) que servirá de directorio raíz para los archivos.
     - Asegúrate de que los permisos de la carpeta sean compatibles tanto con el sistema operativo como con el servidor FTP.
   
   - **4.1.** Abrir el "Administrador de IIS" y agregar un nuevo sitio FTP.

![alt text](image-11.png)
---
![alt text](image-12.png)
  
   - **4.2.** Dar nombre al sitio FTP (p.e. iniciales.local)
   - **4.3.** Habilitar los nombres de host virtuales para permitir que diferentes dominios apunten al mismo servidor FTP.
   - **4.4.** Configurar sin SSL (Secure Sockets Layer) para simplificar la configuración en este entorno de prueba.

![alt text](image-13.png)

## **5. Configurar Accesos al Servidor FTP**

![alt text](image-14.png)

   - **5.1.** Seleccionar "Autenticación Básica" para permitir que los usuarios inicien sesión con nombre de usuario y contraseña.
   - **5.2.** Generar reglas de permisos para definir qué usuarios/grupos pueden leer, escribir o modificar archivos en el FTP:
       - ![alt text](image-15.png)
  
     > *Click derecho -> Agregar reglas de permiso*

     - **Lectura y escritura** para:
       - Usuario **AdminFTP**.
       - Grupo **Informáticos**.
     - **Lectura** para:
       - Grupo **Contables**.
       - ![alt text](image-24.png)

   - **5.3.** Configurar el número máximo de conexiones simultáneas a **50** para limitar la carga en el servidor.
   - ![alt text](image-17.png)
   - ![alt text](image-18.png)
   

## **6. Configurar Permisos de la Carpeta en el Sistema Operativo**
   - Navegar a la carpeta creada en el paso 4.1 (**INICIALES_LOCAL** (donde INICIALES representa las iniciales del nombre completo del alumno, por ejemplo, FJLM_LOCAL para Francisco Javier López Mota)).
   - ![alt text](image-19.png)
   - ![alt text](image-20.png)
   - Asignar permisos adecuados según los requisitos:
     - **Lectura y escritura** para el usuario **AdminFTP** y el grupo **Informáticos**.
     - **Lectura** para el grupo **Contables**.
   - Asegurarse de que los permisos están alineados con las reglas definidas en el IIS.

## **7. Conectar al Servidor FTP desde CMD**
   - Abrir el símbolo del sistema (CMD).
   - **7.1.** Crear un directorio de prueba para comprobar el acceso al servidor FTP:
     ```cmd
     ftp 172.16.X.9
     REM (donde X es el número de lista del alumno, por ejemplo, 43 si el alumno es el número 43)
     Usuario (ip:(none)): nombreFtpVirtual.local|usuario
     REM (donde nombreFtpVirtual.local es el nombre que le has dado a tu ftp virtual y usuario es el nombre de usuario con el que te quieres conectar)
     Contraseña:
     REM introduce la contraseña del usuario con el que te quieres conectar    
     mkdir prueba_directorio
     ```
   - ![alt text](image-21.png)
   - Verificar que el directorio ha sido creado exitosamente.

## Consideraciones Adicionales:
- **Seguridad**: Aunque se esté configurando sin SSL, en un entorno real se debería habilitar SSL para proteger las credenciales de los usuarios.
- **Documentación**: Anotar los usuarios, contraseñas y permisos configurados para futuras referencias.

## Objetivo de Evaluación:
- Verificar que los alumnos han comprendido la configuración completa del servidor FTP y la gestión de usuarios y permisos, así como la conexión al servidor usando herramientas de línea de comandos. Además, cada alumno deberá personalizar la configuración con sus iniciales y número de lista para el directorio compartido y la red, respectivamente.

