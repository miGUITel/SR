**Tarea: Configuración de Servicios de Acceso Remoto RDP en Windows Server 2019 y Ubuntu Server 20.04**

### Introducción

El objetivo de esta guia es aprender a configurar y utilizar el servicio de acceso remoto mediante el protocolo RDP (Remote Desktop Protocol) tanto en un entorno Windows Server 2019 como en Ubuntu Server 20.04.

Al finalizar, serás capaz de establecer conexiones remotas de manera segura y eficiente, probando configuraciones gráficas y en línea de comandos.

---

### Parte 1: Configuración de RDP en Windows Server 2019

1. **Activación del servicio RDP**

   - Habilita el servicio de Escritorio Remoto en Windows Server 2019 desde:
     - Interfaz gráfica: "Propiedades del sistema" → "Acceso remoto" → Activar "Permitir conexiones remotas a este equipo".
     - Línea de comandos (PowerShell):
       ```powershell
       Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -Name "fDenyTSConnections" -Value 0
       Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
       ```

2. **Prueba de conexión desde un cliente Windows**

   - Desde un cliente Windows, abre la herramienta "Conexión a Escritorio Remoto" (mstsc).
   - Introduce la dirección IP.
   - Accede utilizando las credenciales del servidor.

3. **Verificación**

   - Comprueba que puedes conectarte al servidor y navega por su interfaz gráfica.


---

### Parte 2: Configuración de RDP en Ubuntu Server 20.04

#### 2.1. Preparación del sistema

1. **Actualización de los repositorios**:

   ```bash
   sudo apt-get update -y
   ```

2. **Instalación del servidor RDP (xrdp)**:

   ```bash
   sudo apt-get install xrdp -y
   ```

#### 2.2. Configuración del entorno gráfico

1. **Instalación de LXDE como entorno gráfico ligero**:

   ```bash
   sudo apt-get install xorg lxde-core lxde-icon-theme -y
   ```

2. **Configurar inicio en modo gráfico**:

   - Crea el archivo de configuración para iniciar sesión gráfica:
     ```bash
     echo lxsession > ~/.xsession
     sudo cp ~/.xsession /etc/skel/.xsession
     ```

#### 2.3. Configuración del firewall

1. **Permitir conexiones RDP en el firewall**:
   ```bash
   sudo ufw allow 3389
   ```

#### 2.4. Prueba de conexión

1. **Iniciar sesión gráfica en Ubuntu Server**:

   - Conéctate desde otro dispositivo utilizando un cliente RDP como "Conexión a Escritorio Remoto" (Windows) o VINAGRE (Ubuntu).

2. **Uso de VINAGRE**:

   - Abre la aplicación VINAGRE en un cliente Ubuntu y conecta al servidor.
   - Introduce la dirección IP y las credenciales del servidor.

3. **Verificación**

   - Realiza una captura de pantalla mostrando la sesión gráfica activa.

---

### Parte 3: Informe y entrega

1. **Informe escrito**:
   - Describe los pasos que seguiste y las configuraciones realizadas.
   - Incluye capturas de pantalla que demuestren:
     - Activación y conexión en Windows Server 2019.
     - Activación y conexión en Ubuntu Server 20.04.

2. **Preguntas reflexivas** (responde en el informe):
   - ¿Qué diferencias observas entre las configuraciones de RDP en Windows y Ubuntu?
   - ¿Qué retos enfrentaste durante la configuración y cómo los resolviste?
   - ¿En qué casos podría ser más útil un entorno gráfico frente a uno de texto para la administración remota?

3. **Entrega**:
   - Sube tu informe y capturas de pantalla al aula virtual antes de la fecha límite.

---

### Objetivos de Aprendizaje

- Configurar y utilizar servicios de acceso remoto en entornos Windows y Linux.
- Comparar y contrastar diferentes configuraciones de acceso remoto.
- Identificar problemas de conectividad y solucionarlos.
- Documentar procesos técnicos de forma clara y precisa.

---

