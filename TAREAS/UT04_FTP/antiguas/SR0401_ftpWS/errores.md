# - [🧾 **LISTA DE COMPROBACIÓN DE ERRORES FTP (IIS en Windows Server)**](#-lista-de-comprobación-de-errores-ftp-iis-en-windows-server)
- [- 🧾 **LISTA DE COMPROBACIÓN DE ERRORES FTP (IIS en Windows Server)**](#---lista-de-comprobación-de-errores-ftp-iis-en-windows-server)
  - [🔹 1. **Usuario y credenciales**](#-1-usuario-y-credenciales)
  - [🔹 2. **Configuración de autenticación FTP (IIS)**](#-2-configuración-de-autenticación-ftp-iis)
  - [🔹 3. **Reglas de autorización (FTP Authorization Rules)**](#-3-reglas-de-autorización-ftp-authorization-rules)
  - [🔹 4. **Permisos del sistema operativo (NTFS y seguridad)**](#-4-permisos-del-sistema-operativo-ntfs-y-seguridad)
  - [🔹 5. **Estado del servicio y del sitio FTP**](#-5-estado-del-servicio-y-del-sitio-ftp)
  - [🧠 CONSEJO FINAL (prueba rápida integral)](#-consejo-final-prueba-rápida-integral)

---

## 🔹 1. **Usuario y credenciales**

| Nº  | Posible error                                                | Cómo comprobar                                           | Solución                                                                                                                        |
| --- | ------------------------------------------------------------ | -------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| 1.1 | ❌ El usuario **no existe** en el sistema                     | `net user` y `net user <nombre>`                         | Crear el usuario local: `net user usuario contraseña /add`                                                                      |
| 1.2 | ❌ Contraseña **incorrecta**                                  | `runas /user:usuario cmd` → probar contraseña            | Restablecer contraseña o comprobar mayúsculas/teclado                                                                           |
| 1.3 | ❌ La cuenta está **deshabilitada**                           | `net user <usuario>` → “Cuenta activa: No”               | `net user <usuario> /active:yes`                                                                                                |
| 1.4 | ⚠️ La contraseña ha **expirado**                             | `net user <usuario>` → “La contraseña expira”            | `net user <usuario> /passwordchg:no /expires:never`                                                                             |
| 1.5 | ⚠️ El usuario **no tiene permiso de inicio de sesión local** | Directiva local → “Permitir inicio de sesión localmente” | Añadir el usuario o grupo en *gpedit.msc → Configuración de seguridad → Directivas locales → Asignación de derechos de usuario* |

---

## 🔹 2. **Configuración de autenticación FTP (IIS)**

| Nº  | Error                                                                   | Cómo comprobar                           | Solución                                                            |
| --- | ----------------------------------------------------------------------- | ---------------------------------------- | ------------------------------------------------------------------- |
| 2.1 | ❌ No está activada la **Autenticación FTP**                             | IIS → Sitio FTP → *FTP Authentication*   | Activar **Basic** y/o **Anonymous** según el caso                   |
| 2.2 | ⚠️ Solo está activa una (Basic o Anonymous) pero el cliente usa la otra | Revisa en IIS qué método está habilitado | Activar el tipo que necesites (p. ej., Basic si usas usuario local) |
| 2.3 | ❌ IIS está configurado para **requerir SSL**                            | IIS → *FTP SSL Settings*                 | Cambiar a **Allow SSL** o **No SSL** si usas `ftp.exe`              |
| 2.4 | ⚠️ IIS usa **hostnames virtuales** pero el cliente no envía `HOST`      | IIS → *Bindings…* → “Host name” no vacío | Dejar “Host name” vacío o usar cliente compatible (FileZilla)       |

---

## 🔹 3. **Reglas de autorización (FTP Authorization Rules)**

| Nº  | Error                                                                    | Cómo comprobar                        | Solución                                         |
| --- | ------------------------------------------------------------------------ | ------------------------------------- | ------------------------------------------------ |
| 3.1 | ❌ No hay **ninguna regla de acceso**                                     | IIS → *FTP Authorization Rules* vacío | Añadir **Allow → All Users → Read/Write**        |
| 3.2 | ⚠️ Solo hay reglas para “Anonymous users” pero usas autenticación básica | Revisa las reglas existentes          | Añadir regla específica para ese usuario o grupo |
| 3.3 | ⚠️ Regla permite “Read” pero intentas subir archivos                     | Revisa permisos “Write”               | Añadir permiso “Write” en la regla               |

---

## 🔹 4. **Permisos del sistema operativo (NTFS y seguridad)**

| Nº  | Error                                                            | Cómo comprobar                                | Solución                                                                                 |
| --- | ---------------------------------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------- |
| 4.1 | ❌ La carpeta raíz del sitio FTP **no permite acceso al usuario** | Propiedades → Seguridad → pestaña *Seguridad* | Agregar `IUSR`, `IIS_IUSRS` y el usuario del FTP con **Lectura** o **Lectura/Escritura** |
| 4.2 | ⚠️ La carpeta raíz no existe o fue movida                        | IIS → *Advanced Settings → Physical Path*     | Corregir la ruta o crear la carpeta                                                      |
| 4.3 | ⚠️ El usuario no pertenece al grupo adecuado                     | `net localgroup IIS_IUSRS`                    | Agregar el usuario al grupo si procede                                                   |

---

## 🔹 5. **Estado del servicio y del sitio FTP**

| Nº  | Error                                                    | Cómo comprobar                                  | Solución                                           |
| --- | -------------------------------------------------------- | ----------------------------------------------- | -------------------------------------------------- |
| 5.1 | ❌ El sitio FTP está **detenido**                         | IIS → columna “Estado”                          | Clic derecho → *Iniciar*                           |

                                                                                                                                |


## 🧠 CONSEJO FINAL (prueba rápida integral)

1. `net user` → ¿el usuario existe y está activo?
2. `runas /user:usuario cmd` → ¿la contraseña funciona?
3. IIS → ¿Autenticación FTP activa (Basic/Anonymous)?
4. IIS → ¿Reglas de autorización configuradas?
5. Carpeta física → ¿permisos NTFS para `IUSR` o el usuario?
6. IIS → ¿Sitio iniciado y puerto 21 libre?
7. `netstat -an | find ":21"` → ¿escucha el servicio?
8. `ftp localhost` → ¿responde con “220 Microsoft FTP Service”?

