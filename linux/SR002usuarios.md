La **creación de usuarios y contraseñas** es una tarea fundamental en la administración de sistemas Linux. A continuación, detallo los conceptos clave, los comandos necesarios y algunas consideraciones de seguridad.

---

### **Creación de usuarios en Linux**
Los usuarios en Linux se gestionan mediante el archivo `/etc/passwd`, que almacena información básica de cada usuario, y `/etc/shadow`, donde se almacenan las contraseñas encriptadas.

#### Comando principal: `useradd`
- **`useradd`** es el comando para crear un usuario en el sistema.
- Sintaxis básica:
  ```bash
  sudo useradd nombre_usuario
  ```
- Esto crea al usuario pero **no establece una contraseña** ni configura el entorno.

#### Comando complementario: `passwd`
- Para asignar una contraseña al usuario recién creado:
  ```bash
  sudo passwd nombre_usuario
  ```

#### Ejemplo completo:
1. Crear un usuario llamado "alumno":
   ```bash
   sudo useradd alumno
   ```
2. Establecer su contraseña:
   ```bash
   sudo passwd alumno
   ```

---

### **Creación de usuarios con opciones avanzadas**
El comando `useradd` permite personalizar la configuración del usuario al crearlo:

#### Parámetros comunes:
1. **Establecer el directorio personal:**
   - Por defecto, el directorio personal se crea en `/home/nombre_usuario`. Si deseas especificar otro directorio:
     ```bash
     sudo useradd -m -d /ruta/personalizada nombre_usuario
     ```

2. **Asignar un shell específico:**
   - Puedes especificar el shell que usará el usuario (por ejemplo, `/bin/bash` o `/bin/false` si no quieres que pueda iniciar sesión):
     ```bash
     sudo useradd -s /bin/bash nombre_usuario
     ```

3. **Añadir un comentario:**
   - Útil para identificar al usuario:
     ```bash
     sudo useradd -c "Alumno del curso de informática" nombre_usuario
     ```

4. **Asignar un grupo específico:**
   - Por defecto, el sistema crea un grupo con el mismo nombre del usuario. Sin embargo, puedes añadirlo a un grupo existente:
     ```bash
     sudo useradd -g grupo_existente nombre_usuario
     ```

5. **Definir una fecha de expiración para la cuenta:**
   - Ideal para usuarios temporales:
     ```bash
     sudo useradd -e 2024-12-31 nombre_usuario
     ```

#### Ejemplo con múltiples opciones:
Crear un usuario "provisional" con un directorio personalizado, shell restringido y expiración:
```bash
sudo useradd -m -d /tmp/provisional -s /bin/false -e 2024-12-31 provisional
```

---

### **Modificación de usuarios existentes**
1. **Cambiar el nombre de usuario:**
   ```bash
   sudo usermod -l nuevo_nombre_usuario antiguo_nombre_usuario
   ```

2. **Cambiar el directorio personal:**
   - También puedes mover los archivos al nuevo directorio:
     ```bash
     sudo usermod -d /nuevo/directorio -m nombre_usuario
     ```

3. **Añadir un usuario a un grupo secundario:**
   - Por ejemplo, añadir a "alumno" al grupo "sudo":
     ```bash
     sudo usermod -aG sudo alumno
     ```

---

### **Eliminación de usuarios**
1. **Eliminar un usuario y su directorio personal:**
   ```bash
   sudo userdel -r nombre_usuario
   ```

2. **Eliminar un usuario sin eliminar su directorio personal:**
   ```bash
   sudo userdel nombre_usuario
   ```

---

### **Gestión de contraseñas**
1. **Cambiar la contraseña de un usuario existente:**
   - Como administrador:
     ```bash
     sudo passwd nombre_usuario
     ```

2. **Forzar el cambio de contraseña en el próximo inicio de sesión:**
   - Ideal para nuevas cuentas:
     ```bash
     sudo passwd -e nombre_usuario
     ```

3. **Bloquear una cuenta (sin eliminarla):**
   - Bloquear el inicio de sesión:
     ```bash
     sudo passwd -l nombre_usuario
     ```
   - Para desbloquear:
     ```bash
     sudo passwd -u nombre_usuario
     ```

4. **Definir políticas de contraseñas:**
   - Las políticas se gestionan mediante el archivo `/etc/login.defs` o herramientas como `chage`:
     - **Definir un mínimo de días entre cambios de contraseña:**
       ```bash
       sudo chage -m 7 nombre_usuario
       ```
     - **Definir un máximo de días antes de que expire la contraseña:**
       ```bash
       sudo chage -M 90 nombre_usuario
       ```
     - **Mostrar las configuraciones de la cuenta:**
       ```bash
       chage -l nombre_usuario
       ```

---

### **Consideraciones de seguridad**
1. **Políticas de contraseñas fuertes:**
   - Instalar y usar el módulo `pam_pwquality` para configurar reglas como:
     - Longitud mínima.
     - Complejidad (números, mayúsculas, etc.).
   - Configuración en `/etc/security/pwquality.conf`.

2. **Cuentas inactivas:**
   - Deshabilitar automáticamente cuentas que no se usen en un tiempo:
     ```bash
     sudo usermod -f 30 nombre_usuario
     ```

3. **Auditoría de accesos:**
   - Revisar el archivo `/var/log/auth.log` para detectar intentos fallidos de acceso.

---

Este enfoque permite administrar usuarios de manera eficiente, asegurando que el sistema sea funcional y seguro. Si necesitas ejemplos más específicos o ejercicios prácticos, ¡puedo ayudarte!