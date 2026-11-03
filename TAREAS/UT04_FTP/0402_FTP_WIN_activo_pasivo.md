# 0402 · FTP Windows: activo y pasivo

Las imágenes de ayuda proceden de los guiones anteriores. Utiliza los datos del escenario y de cada paso. Las capturas de tu entrega corresponderán a tus propios equipos. [Consultar la ayuda visual](#ayuda-visual).

[Situación de partida](#1-situación-de-partida) · [Conexión de control](#2-preparar-el-cliente-y-comprobar-la-conexión-de-control) · [Preparar el modo pasivo](#3-preparar-el-servidor-para-las-conexiones-pasivas) · [Probar pasivo](#4-probar-el-modo-pasivo-con-filezilla) · [Probar activo](#5-probar-el-modo-activo-con-el-mismo-usuario) · [Consola](#6-comprobar-también-desde-consola) · [Comparar](#7-comparar-y-razonar) · [Entrega](#8-qué-debes-entregar) · [Valoración](#9-cómo-se-valorará)

## 1. Situación de partida

El servidor de la empresa ya permite entrar con la cuenta `ftpbase`. Ahora debes comprobar dos formas de establecer la **conexión de datos**: activo y pasivo. Utilizarás el mismo usuario, la misma carpeta y los mismos permisos; cambiará la forma de conectar.

Continúa con el servidor de 0401 y prepara el cliente indicado a continuación. **No vuelvas a instalar FTP ni crees otro sitio.**

| Equipo o elemento | Configuración |
|---|---|
| Servidor `FTP-SRV` | Windows Server 2019 · `192.168.4.10/24` |
| Cliente `FTP-CLI` | Otra máquina Windows · `192.168.4.20/24` |
| VirtualBox | Los dos equipos en la red interna `SR-FTP` |
| Puerta de enlace y DNS | En blanco en esta red aislada; las conexiones se harán por IP |
| Sitio existente | `FTP_EMPRESA` · TCP 21 · sin nombre de host virtual |
| Carpeta del servidor | `C:\FTP\Empresa` |
| Cuenta de pruebas | `ftpbase`, con lectura y escritura |

Usa únicamente credenciales de laboratorio. En esta práctica se emplea FTP sin cifrado dentro de la red interna; no utilices contraseñas personales ni conectes el servicio a la red pública.

## 2. Preparar el cliente y comprobar la conexión de control

### 2.1. Preparar la VM cliente

1. Utiliza un clon de trabajo de Windows para `FTP-CLI`; puede ser otro Windows Server 2019. Si aún no tienes ese clon, créalo desde una máquina de partida apagada, genera nuevas direcciones MAC y conserva las máquinas y las instantáneas de DNS. Este equipo actuará como cliente y no necesita instalar el servicio FTP.
2. Comprueba que dispone de **FileZilla Client**, la aplicación cliente. Si falta, prepara su instalador desde la [web oficial de FileZilla](https://filezilla-project.org/) con el profesor y completa la instalación antes de las pruebas. Para una descarga, utiliza temporalmente NAT y configuración automática de red; al terminar, vuelve a la red interna y a la IP fija del paso siguiente.
3. Con la VM apagada, conecta su adaptador de trabajo a **Red interna → SR-FTP** y deshabilita los demás adaptadores durante las pruebas.
4. En Windows, cambia el nombre del equipo a `FTP-CLI` y reinicia si lo solicita. En **Panel de control → Centro de redes y recursos compartidos → Cambiar configuración del adaptador → Ethernet → Propiedades → IPv4**, establece `192.168.4.20`, máscara `255.255.255.0`, sin puerta de enlace ni DNS.
5. Comprueba que el servidor conserva `192.168.4.10/24` y que ambos equipos están en `SR-FTP`. En el Explorador del cliente, activa **Vista → Extensiones de nombre de archivo** para que los archivos de prueba no terminen en `.txt.txt`.

### 2.2. Permitir y comprobar el canal de control

1. Inicia servidor y cliente. Comprueba sus direcciones con `ipconfig`.
2. En **PowerShell como administrador de FTP-SRV**, permite el canal de control únicamente desde el cliente:

   ```powershell
   New-NetFirewallRule -Name "SR-FTP-Control" -DisplayName "SR FTP control" -Direction Inbound -Action Allow -Protocol TCP -LocalAddress 192.168.4.10 -LocalPort 21 -RemoteAddress 192.168.4.20 -Profile Any
   ```

   Si ya existe una regla con ese nombre, comprueba sus propiedades en **Firewall de Windows Defender con seguridad avanzada → Reglas de entrada**; no la dupliques. Debe permitir TCP 21 hacia `192.168.4.10` únicamente desde `192.168.4.20`.

3. En **PowerShell del cliente**, ejecuta:

   ```powershell
   Test-NetConnection 192.168.4.10 -Port 21
   ```

4. Debe aparecer `TcpTestSucceeded : True`. Si no aparece, revisa antes de continuar: nombre de red interna, IP de ambos equipos, sitio iniciado y regla de entrada al puerto 21 que acabas de configurar.

> Esta prueba comprueba el puerto de control. Todavía no demuestra que puedas listar carpetas o transferir archivos.

## 3. Preparar el servidor para las conexiones pasivas

### 3.1. Fijar el rango de puertos en IIS

**Ayuda:** [localizar la herramienta en IIS](#v1-localizar-compatibilidad-con-el-firewall-de-ftp) y [ayuda del rango pasivo](#v3-rango-pasivo-del-servidor).

En **FTP-SRV**:

1. Abre **Administrador del servidor → Herramientas → Administrador de Internet Information Services (IIS)**.
2. En **Conexiones**, selecciona el **nombre del servidor**, por encima de Sitios. Abre **Compatibilidad con el firewall de FTP**.
3. En **Intervalo de puertos del canal de datos**, escribe:

   ```text
   50000-50049
   ```

4. Deja en blanco la **dirección IP externa del firewall**: los dos equipos están en la misma red interna, sin un router NAT entre ellos. Pulsa **Aplicar**.
5. Reinicia **Microsoft FTP Service** desde Servicios, o ejecuta en PowerShell como administrador:

   ```powershell
   Restart-Service FTPSVC
   ```

> El rango pasivo se configura en el nivel del servidor. Reiniciar únicamente el sitio no sustituye al reinicio del servicio FTP después de cambiar este rango.

### 3.2. Permitir esos puertos en el firewall del servidor

**Ayuda:** [ayuda de la regla de puertos pasivos](#v8-reglas-del-firewall).

En **PowerShell como administrador de FTP-SRV**, crea una regla limitada al cliente del laboratorio:

```powershell
New-NetFirewallRule -Name "SR-FTP-Pasivo" -DisplayName "SR FTP datos pasivos" -Direction Inbound -Action Allow -Protocol TCP -LocalAddress 192.168.4.10 -LocalPort 50000-50049 -RemoteAddress 192.168.4.20 -Profile Any
```

Si ya existe una regla con ese nombre, comprueba sus propiedades en **Firewall de Windows Defender con seguridad avanzada → Reglas de entrada**; no la dupliques. Debe permitir TCP 50000–50049 hacia el servidor y únicamente desde `192.168.4.20`.

Mantén activado el firewall. No cambies las opciones globales de filtrado FTP ni abras rangos completos de puertos del sistema.

## 4. Probar el modo pasivo con FileZilla

En **FTP-CLI**, utiliza FileZilla Client, preparado para el laboratorio.

### 4.1. Guardar la conexión

**Ayuda:** [identificar los paneles local, remoto y de registro](#v2-paneles-de-filezilla) y [ayuda del Gestor de sitios](#v4-gestor-de-sitios-en-pasivo).

1. Abre **Archivo → Gestor de sitios → Nuevo sitio**. Llámalo `FTP_EMPRESA_PASIVO`.
2. Introduce los siguientes datos:

   | Campo | Valor |
   |---|---|
   | Protocolo | FTP |
   | Servidor | `192.168.4.10` |
   | Puerto | `21` |
   | Cifrado | Usar solamente FTP simple, dentro de este laboratorio aislado |
   | Modo de acceso | Preguntar la contraseña |
   | Usuario | `ftpbase` |

3. En **Configuración de transferencia**, selecciona expresamente **Pasivo**.
4. Conecta e introduce la contraseña de laboratorio.

### 4.2. Listar, subir y bajar

1. Crea en el cliente la carpeta `C:\FTP_CLIENTE` y, dentro, un archivo `0402_pasivo.txt`. Escribe una frase que identifique tu prueba.
2. En el panel local de FileZilla, entra en `C:\FTP_CLIENTE`. En el remoto, actualiza el listado de la raíz del sitio.
3. Sube `0402_pasivo.txt` arrastrándolo del panel local al remoto.
4. Crea en el cliente una subcarpeta `descargas`. Entra en ella en el panel local y descarga el archivo desde el panel remoto.
5. Abre el archivo descargado y comprueba su contenido. En el servidor también debe existir en `C:\FTP\Empresa`.
6. Revisa el registro superior de FileZilla. Localiza `PASV` o `EPSV`, la respuesta del servidor y una transferencia terminada correctamente. No basta con que aparezca «conectado».

**Debes poder demostrar:** listado remoto, subida y descarga. En el modo pasivo, el servidor comunica un puerto de datos y el cliente inicia la conexión hacia él. El rango configurado no sustituye al puerto 21: cada uno tiene una función.

## 5. Probar el modo activo con el mismo usuario

### 5.1. Preparar el cliente

**Ayuda:** [ayuda de la configuración activa](#v5-configuración-activa-del-cliente).

En **FTP-CLI**:

1. Desconecta FileZilla.
2. Abre **Edición → Opciones → Conexión → FTP → Modo activo**. Los nombres pueden variar ligeramente según el idioma y la versión.
3. Limita los puertos locales utilizados a **51000–51049**.
4. Configura como dirección para el modo activo la IP del cliente en esta red: **`192.168.4.20`**. No consultes una web para obtener una dirección pública.
5. En PowerShell como administrador del cliente, permite las conexiones de datos procedentes únicamente del servidor:

   ```powershell
   New-NetFirewallRule -Name "SR-FTP-Activo-FileZilla" -DisplayName "SR FTP datos activos FileZilla" -Direction Inbound -Action Allow -Protocol TCP -LocalAddress 192.168.4.20 -LocalPort 51000-51049 -RemoteAddress 192.168.4.10 -Profile Any
   ```

Si la regla ya existe, revisa sus propiedades en lugar de crear otra.

### 5.2. Repetir las operaciones en modo activo

1. En el **Gestor de sitios**, copia la conexión anterior y llámala `FTP_EMPRESA_ACTIVO`.
2. Conserva servidor, puerto y usuario. En **Configuración de transferencia**, selecciona **Activo**.
3. Conecta de nuevo. No reutilices una sesión que ya estaba abierta en modo pasivo.
4. Crea `C:\FTP_CLIENTE\0402_activo.txt` con un contenido diferente.
5. Actualiza el listado, sube ese archivo y descárgalo a `C:\FTP_CLIENTE\descargas`.
6. Comprueba el contenido y localiza en el registro `PORT` o `EPRT` y el resultado correcto de la transferencia.

> En activo, el cliente comunica dónde espera la conexión de datos y el servidor la inicia hacia el cliente. Esto se aplica tanto a una subida como a una descarga: **activo no significa subir y pasivo no significa bajar**.

## 6. Comprobar también desde consola

**Ayuda:** [ayuda de las pruebas de transferencia](#v6-resultados-activo-y-pasivo) y [comprobación desde consola](#v7-prueba-desde-consola).

Realiza la prueba en **FTP-CLI**, no desde la consola del servidor.

### Opción A. Cliente `curl.exe` con FTP disponible

En PowerShell, ejecuta:

```powershell
curl.exe --version
```

Si existe y su línea `Protocols` incluye `ftp`, crea `C:\FTP_CLIENTE\0402_consola.txt` y realiza estas tres operaciones. Cada comando pedirá la contraseña; no la escribas dentro de la URL ni la incluyas en la entrega.

```powershell
curl.exe --ftp-pasv --disable-epsv --max-time 20 --user ftpbase ftp://192.168.4.10/
curl.exe --ftp-pasv --disable-epsv --max-time 20 --user ftpbase --upload-file C:\FTP_CLIENTE\0402_consola.txt ftp://192.168.4.10/0402_consola.txt
curl.exe --ftp-pasv --disable-epsv --max-time 20 --user ftpbase --output C:\FTP_CLIENTE\descargas\0402_consola.txt ftp://192.168.4.10/0402_consola.txt
```

Se fuerza modo pasivo y la orden `PASV`. Abre el archivo descargado y comprueba que contiene lo que subiste. Si `curl.exe` no está disponible o no admite FTP, utiliza la opción B; no necesitas instalar otro programa para completar este apartado.

### Opción B. Cliente `ftp.exe` de Windows, en modo activo

Este cliente no admite modo pasivo. Como el puerto de datos que elige no coincide necesariamente con el rango fijado en FileZilla, necesita su propia regla.

1. En **PowerShell como administrador del cliente**, permite las conexiones hacia este programa, limitadas al servidor y a la IP del cliente:

   ```powershell
   New-NetFirewallRule -Name "SR-FTP-Activo-Consola" -DisplayName "SR FTP datos activos consola" -Direction Inbound -Action Allow -Protocol TCP -Program "$env:SystemRoot\System32\ftp.exe" -LocalAddress 192.168.4.20 -RemoteAddress 192.168.4.10 -Profile Any
   ```

2. Crea `C:\FTP_CLIENTE\0402_consola.txt`. Abre **CMD** y ejecuta:

   ```cmd
   ftp 192.168.4.10
   ```

3. Introduce `ftpbase` y su contraseña. En el indicador `ftp>`, ejecuta uno a uno:

   ```text
   lcd C:\FTP_CLIENTE
   binary
   dir
   put 0402_consola.txt
   get 0402_consola.txt 0402_consola_descarga.txt
   quit
   ```

4. Abre `C:\FTP_CLIENTE\0402_consola_descarga.txt` y verifica su contenido.

Completa **una** de las dos opciones e identifica cuál utilizaste y en qué modo trabajó.

## 7. Comparar y razonar

Completa esta tabla con tus observaciones, utilizando las IP y los puertos que hayas visto en los registros:

| Pregunta | Activo | Pasivo |
|---|---|---|
| ¿Quién inicia la conexión de control y hacia qué puerto? | | |
| ¿Quién escucha la conexión de datos y en qué equipo? | | |
| ¿Quién inicia la conexión de datos? | | |
| ¿Qué orden aparece: `PORT`/`EPRT` o `PASV`/`EPSV`? | | |
| ¿Has conseguido listar, subir y bajar? | | |

Responde brevemente:

1. ¿Por qué un inicio de sesión correcto no demuestra que el canal de datos funcione?
2. ¿En qué equipo revisarías primero el firewall si falla únicamente el modo activo? ¿Y si falla únicamente el pasivo? Justifica tu respuesta con la tabla.
3. ¿Ha cambiado el permiso de `ftpbase` al cambiar de modo? ¿Por qué?

Si una prueba ha fallado, conserva el mensaje, identifica en qué operación ocurrió y explica la comprobación que te permitió resolverlo. Antes de cambiar permisos, distingue un fallo de conexión de datos de una operación denegada al usuario.

## 8. Qué debes entregar

Entrega **un único PDF**, llamado `0402_Apellidos_Nombre.pdf`, de **un máximo de 5 páginas en total**, incluida la portada si la utilizas. No adjuntes contraseñas, exportaciones del Gestor de sitios ni archivos de configuración personales.

Incluye **como máximo 5 capturas**, legibles y acompañadas de una explicación breve:

1. Rango pasivo configurado en IIS.
2. Prueba pasiva desde FileZilla, con el registro que identifica el modo y el resultado de una transferencia.
3. Prueba activa desde FileZilla, con el registro que identifica el modo y el resultado de una transferencia.
4. Prueba desde consola y comprobación del archivo descargado.
5. Solo si aporta información: configuración del rango activo o una incidencia que hayas diagnosticado.

Añade la tabla comparativa y las tres respuestas razonadas. Las capturas deben corresponder a tus equipos. Selecciona los fragmentos que prueban los resultados; no copies todo el guion.

## 9. Cómo se valorará

| Calificación | Criterio |
|---|---|
| 0 | **No entregado.** |
| 3 | **La mayoría de las respuestas son incorrectas.** La tarea presenta errores importantes que muestran una comprensión insuficiente de los contenidos o dificultades para aplicar los procedimientos trabajados. |
| 6 | **La mayoría de las respuestas son correctas.** Se demuestra una comprensión general de los contenidos y una aplicación adecuada de los procedimientos, aunque quedan errores u omisiones por corregir. |
| 9 | **Todo correcto.** La tarea responde de forma completa y correcta a lo solicitado, con explicaciones claras y procedimientos bien aplicados. |
| 10 | **Todo correcto y con una aportación extra.** Además de resolver correctamente la tarea, el alumno incorpora una aportación propia y pertinente que enriquece el trabajo, como un ejemplo, una comprobación adicional o una reflexión razonada. |



---

## Ayuda visual

Las dos imágenes históricas siguientes ayudan a encontrar herramientas y a interpretar la interfaz. Sus nombres de equipo, IP y archivos pertenecen a otro laboratorio. Utiliza `FTP-SRV`, `FTP_EMPRESA`, `192.168.4.10`, `ftpbase` y las rutas indicadas en los pasos de esta práctica. Las ayudas de texto completan las imágenes con los valores que debes comprobar.

### V1. Localizar Compatibilidad con el firewall de FTP

![Herramientas FTP de IIS: icono Compatibilidad con el firewall](antiguas/SR0401_ftpWS/image-14.png)

*La imagen permite reconocer el icono **Compatibilidad con el firewall…**, situado junto a Autenticación FTP. El sitio `EJEMPLO_LOCAL` es histórico. En el apartado 3.1 selecciona el **nombre del servidor** en el árbol de IIS para fijar el rango global del canal de datos.*

[Volver a preparar el rango pasivo](#31-fijar-el-rango-de-puertos-en-iis).

### V2. Paneles de FileZilla

![FileZilla con registro de actividad y paneles de archivos local y remoto](antiguas/SR0402_ftp2/image-26.png)

*Arriba aparece el registro; a la izquierda, el equipo local; a la derecha, el servidor. El ejemplo muestra una subida terminada y el archivo en el panel remoto. La IP `172.16.0.9`, el usuario con prefijo de host virtual y las rutas antiguas pertenecen al ejemplo. En tu práctica conecta a `192.168.4.10` con `ftpbase`, sin ese prefijo. Esta imagen ayuda a leer los paneles; por sí sola no demuestra si la transferencia fue activa o pasiva.*

[Volver a la conexión con FileZilla](#41-guardar-la-conexión).

### V3. Rango pasivo del servidor

En **FTP-SRV**, selecciona el nombre del servidor → **Compatibilidad con el firewall de FTP**. Comprueba el intervalo `50000-50049` y la dirección IP externa en blanco.

Comprueba el nivel de IIS que estás configurando: el intervalo se fija en el nombre del servidor.

[Volver a fijar el rango pasivo](#31-fijar-el-rango-de-puertos-en-iis).

### V4. Gestor de sitios en pasivo

En **FileZilla → Archivo → Gestor de sitios → FTP_EMPRESA_PASIVO**, comprueba IP `192.168.4.10`, puerto `21`, FTP simple, preguntar contraseña y usuario `ftpbase`. En **Configuración de transferencia**, selecciona **Pasivo**.

[Volver a guardar la conexión](#41-guardar-la-conexión).

### V5. Configuración activa del cliente

En **FileZilla → Opciones → FTP → Modo activo**, comprueba el intervalo `51000-51049` y la dirección `192.168.4.20`. En el Gestor de sitios, `FTP_EMPRESA_ACTIVO` debe tener **Activo** seleccionado.

[Volver a preparar el cliente activo](#51-preparar-el-cliente).

### V6. Resultados activo y pasivo

En el registro de FileZilla, identifica `PASV` o `EPSV` para `0402_pasivo.txt` y `PORT` o `EPRT` para `0402_activo.txt`. Comprueba el listado y la transferencia terminada; conserva también el archivo descargado.

[Volver a la comparación de resultados](#7-comparar-y-razonar).

### V7. Prueba desde consola

Desde **FTP-CLI**, realiza la opción de consola elegida: `curl.exe` en pasivo o `ftp.exe` en activo. Comprueba listado, subida, descarga y contenido, con `192.168.4.10` y `ftpbase`.

[Volver a la prueba desde consola](#6-comprobar-también-desde-consola).

### V8. Reglas del firewall

En el servidor, revisa **SR-FTP-Control** y **SR-FTP-Pasivo**: entrada desde `192.168.4.20` a `192.168.4.10`, puertos `21` y `50000-50049`. En el cliente, **SR-FTP-Activo-FileZilla** permite entrada desde `192.168.4.10` a `192.168.4.20`, puertos `51000-51049`.

[Volver a permitir los puertos pasivos](#32-permitir-esos-puertos-en-el-firewall-del-servidor).

### Referencias visuales oficiales

- [Microsoft Learn: compatibilidad del servicio FTP con el firewall](https://learn.microsoft.com/es-es/iis/configuration/system.ftpserver/firewallsupport). Permite consultar la herramienta; la imagen de la documentación no utiliza el rango elegido para este laboratorio. Mantén `50000-50049` según el apartado 3.
- [FileZilla Pro: Gestor de sitios](https://filezillapro.com/docs/v3/getting-started/site-manager/). Referencia de la interfaz en inglés; utiliza los valores del escenario y las opciones indicadas en el guion.
