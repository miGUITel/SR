La configuración de red con **Netplan** es un método moderno y flexible para gestionar redes en sistemas basados en Ubuntu y otras distribuciones derivadas. Netplan utiliza archivos de configuración YAML que describen las interfaces de red y su comportamiento. A continuación, se detalla cómo configurar redes con Netplan y aprovechar sus características.

---

### **¿Qué es Netplan?**
Netplan es una herramienta que actúa como intermediario entre el sistema y los backends de gestión de red, como:
- **NetworkManager**: Para entornos de escritorio.
- **systemd-networkd**: Para servidores.

Netplan permite configurar redes estáticas, dinámicas, VLANs, enlaces (bonding) y más, de forma sencilla y declarativa.

---

### **Ubicación de los archivos de configuración**
Los archivos de configuración de Netplan están en el directorio:
```bash
/etc/netplan/
```
Por defecto, encontrarás archivos con extensión `.yaml`, como:
```bash
/etc/netplan/01-netcfg.yaml
```

---

### **Estructura básica de un archivo YAML en Netplan**
Un archivo de configuración típico tiene esta estructura:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
```

#### Componentes clave:
1. **`version`**: Define la versión de Netplan (siempre debe ser `2`).
2. **`renderer`**: Especifica el backend (opcional):
   - `networkd`: Servidor o entorno sin GUI.
   - `NetworkManager`: Estaciones de trabajo o entornos de escritorio.
3. **`ethernets`**: Configuración de interfaces Ethernet.
4. **`dhcp4` y `dhcp6`**: Configura DHCP para IPv4 e IPv6.

---

### **Configuración de red básica**
#### 1. Configuración con DHCP (automática)
Permite obtener una dirección IP automáticamente desde un servidor DHCP:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
```

#### 2. Configuración con IP estática
Asignar manualmente una dirección IP, máscara de red, puerta de enlace y DNS:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4
```

#### Desglose:
- **`addresses`**: Especifica las direcciones IP (incluye la máscara con `/24`).
- **`gateway4`**: Define la puerta de enlace para IPv4.
- **`nameservers`**: Especifica los servidores DNS.

---

### **Configuración avanzada de red**
#### 1. Configuración de múltiples interfaces
Para configurar más de una interfaz:
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      addresses:
        - 10.0.0.100/24
      gateway4: 10.0.0.1
      nameservers:
        addresses:
          - 1.1.1.1
```

#### 2. Configuración de una red inalámbrica (Wi-Fi)
Si estás usando una conexión inalámbrica, especifica los detalles:
```yaml
network:
  version: 2
  renderer: NetworkManager
  wifis:
    wlan0:
      access-points:
        "NombreRed":
          password: "contraseña"
      dhcp4: true
```

#### 3. Configuración de VLAN
Para crear una VLAN etiquetada:
```yaml
network:
  version: 2
  renderer: networkd
  vlans:
    vlan10:
      id: 10
      link: enp0s3
      addresses:
        - 192.168.10.100/24
      gateway4: 192.168.10.1
```

#### 4. Configuración de bonding (enlace de interfaces)
Para unir dos interfaces físicas en una sola lógica:
```yaml
network:
  version: 2
  renderer: networkd
  bonds:
    bond0:
      interfaces:
        - enp0s3
        - enp0s8
      addresses:
        - 192.168.1.200/24
      gateway4: 192.168.1.1
      parameters:
        mode: active-backup
        primary: enp0s3
```

---

### **Aplicar y probar la configuración**
1. **Guardar el archivo YAML:**
   - Asegúrate de que el archivo esté bien formateado (respeta la indentación de 2 espacios).

2. **Validar la configuración:**
   - Comprueba errores en el archivo:
     ```bash
     sudo netplan try
     ```
   - Esto aplicará la configuración temporalmente para verificar que funciona.

3. **Aplicar cambios permanentemente:**
   - Si todo está correcto, aplica la configuración:
     ```bash
     sudo netplan apply
     ```

---

### **Solución de problemas**
1. **Comprobar los registros del sistema:**
   ```bash
   journalctl -u systemd-networkd
   ```

2. **Verificar el estado de las interfaces:**
   ```bash
   ip a
   ```

3. **Revisar errores en Netplan:**
   - Asegúrate de que el archivo YAML esté correctamente indentado y que las claves sean válidas.

4. **Reiniciar el servicio de red manualmente:**
   ```bash
   sudo systemctl restart systemd-networkd
   ```

---

### **Ventajas de usar Netplan**
- **Facilidad de uso:** Su formato YAML es fácil de leer y escribir.
- **Flexibilidad:** Compatible con configuraciones simples y avanzadas.
- **Modularidad:** Centraliza la configuración de red en un solo archivo.
- **Compatibilidad:** Trabaja con `NetworkManager` y `systemd-networkd`.
