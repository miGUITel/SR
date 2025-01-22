SSH (Secure Shell) permite autenticarse en un sistema remoto de forma segura. Existen **dos métodos principales de autenticación**:

---

### 1. **Autenticación con usuario y contraseña**
Este es el método más común y básico, donde el cliente proporciona un nombre de usuario y su correspondiente contraseña al servidor.

#### **Flujo del proceso:**
1. El cliente inicia la conexión SSH al servidor utilizando su nombre de usuario.
2. El servidor solicita la contraseña del usuario.
3. Si la contraseña coincide con la almacenada en el servidor, se concede el acceso.

#### **Ventajas:**
- Sencillo de usar y configurar.
- No requiere configuración adicional en el cliente o servidor más allá del uso de contraseñas.

#### **Desventajas:**
- Menos seguro que el método de clave pública, ya que las contraseñas pueden ser vulnerables a ataques de fuerza bruta o intercepción si no se utilizan medidas adicionales como firewalls o autenticación de dos factores.
- Requiere recordar y gestionar contraseñas.

#### **Comando típico:**
```bash
ssh usuario@servidor
```
(Solicitará la contraseña tras ejecutarlo).

---

### 2. **Autenticación con clave pública**
En este método, el cliente utiliza un par de claves criptográficas (clave privada y clave pública). 

#### **Flujo del proceso:**
1. El cliente genera un par de claves (una **clave privada**, que permanece segura en el cliente, y una **clave pública**, que se copia al servidor).
2. Al conectarse, el cliente envía la clave pública al servidor para identificarse.
3. El servidor envía un desafío cifrado al cliente. Este desafío solo puede ser desencriptado con la clave privada del cliente.
4. El cliente desencripta el desafío con su clave privada y lo devuelve firmado al servidor.
5. El servidor verifica la firma utilizando la clave pública del cliente. Si coincide, se concede el acceso sin necesidad de contraseña.

#### **Ventajas:**
- Más seguro que usar contraseñas, ya que no se envía información sensible (como contraseñas) a través de la red.
- Las claves son mucho más difíciles de adivinar o forzar.
- Ideal para automatización y scripts (se puede configurar sin interacción del usuario).

#### **Desventajas:**
- Requiere configuración inicial (generar y copiar claves).
- Si se pierde la clave privada, no se puede acceder al servidor.

#### **Comandos típicos:**
1. **Generar un par de claves en el cliente**:
   ```bash
   ssh-keygen -t rsa
   ```
   Esto crea dos archivos en `~/.ssh/`: 
   - `id_rsa` (clave privada, no debe compartirse).
   - `id_rsa.pub` (clave pública, se copia al servidor).

2. **Copiar la clave pública al servidor**:
   ```bash
   ssh-copy-id usuario@servidor
   ```

3. **Conectarse al servidor**:
   ```bash
   ssh usuario@servidor
   ```
   Si está configurado correctamente, no pedirá contraseña.

---

### Comparación:
| Método                  | Seguridad | Facilidad de uso | Recomendado para  |
|-------------------------|-----------|------------------|--------------------|
| Usuario y contraseña    | Media     | Fácil            | Usuarios ocasionales o principiantes. |
| Clave pública           | Alta      | Requiere setup   | Administradores, automatización, y conexiones frecuentes. |

---

**Conclusión:**  
Usar **clave pública** es más seguro y adecuado para entornos críticos o automatización. Sin embargo, la autenticación con **usuario y contraseña** sigue siendo válida en escenarios simples o cuando no se necesita alta seguridad.

