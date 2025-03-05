# Guía General para la Configuración de Red en Linux

## 1. Configuración en Modo Gráfico (Ubuntu y Linux Mint)
### **Ajustes de Red Avanzados**
1. Abre la configuración de red:
   - En Ubuntu: **Ajustes > Red**.
   - En Linux Mint: **Menú > Preferencias > Configuración de Red**.
2. Selecciona la conexión que quieres modificar (Ethernet o Wi-Fi).
3. Haz clic en **Editar** o **Configurar**.
4. Ve a la pestaña **IPv4** y elige:
   - **DHCP** si quieres que la dirección IP sea asignada automáticamente.
   - **Manual** si quieres establecer una IP fija.
5. Si eliges "Manual", introduce:
   - **Dirección IP** (ejemplo: `192.168.1.100`).
   - **Máscara de red** (ejemplo: `24` para `255.255.255.0`).
   - **Puerta de enlace** (ejemplo: `192.168.1.1`).
   - **Servidores DNS** (ejemplo: `8.8.8.8`).
6. Guarda los cambios y desconecta/reconecta la interfaz de red:
   - Desactiva y vuelve a activar la conexión desde el icono de red.
   - Si no funciona, reinicia **NetworkManager** desde la terminal:
     ```bash
     sudo systemctl restart NetworkManager
     ```

## 2. Configuración en Modo Consola con **NetworkManager**
### **Ver las conexiones disponibles**
```bash
nmcli connection show
```
### **Configurar DHCP**
```bash
nmcli connection modify "nombre_conexion" ipv4.method auto
nmcli connection up "nombre_conexion"
```
### **Configurar IP Estática**
```bash
nmcli connection modify "nombre_conexion" ipv4.addresses 192.168.1.100/24 ipv4.gateway 192.168.1.1 ipv4.dns 8.8.8.8 ipv4.method manual
nmcli connection up "nombre_conexion"
```
Si la conexión no se aplica correctamente, reinicia NetworkManager:
```bash
sudo systemctl restart NetworkManager
```

### **Configurar NetworkManager editando archivos**
#### **1. Ubicar el archivo de la conexión**
```bash
ls /etc/NetworkManager/system-connections/
```
#### **2. Editar la configuración manualmente**
Para establecer una **IP estática**, edita el archivo correspondiente:
```bash
sudo nano /etc/NetworkManager/system-connections/"nombre_conexion.nmconnection"
```
Modifica la sección `[ipv4]`:
```ini
[ipv4]
method=manual
addresses1=192.168.1.100/24,192.168.1.1
dns=8.8.8.8;
```
Para **DHCP**, cambia `method=manual` por:
```ini
method=auto
```
#### **3. Aplicar los cambios**
```bash
sudo systemctl restart NetworkManager
```
o recargar la configuración:
```bash
nmcli connection reload
nmcli connection up "nombre_conexion"
```

## 3. Configuración en Modo Consola con **Netplan** [ampliación netplan](./SR005redNetPlan.md)
### **Ubicación del archivo de configuración**
Los archivos de configuración de Netplan están en `/etc/netplan/`. Para listar los archivos existentes:
```bash
ls /etc/netplan/
```
Normalmente el archivo se llama `00-installer-config.yaml` o `01-netcfg.yaml`. Edita el archivo con:
```bash
sudo nano /etc/netplan/00-installer-config.yaml
```
### **Configurar DHCP con Netplan**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      dhcp4: true
```
### **Configurar IP Estática con Netplan**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp1s0:
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```
### **Aplicar los cambios**
Ejecuta:
```bash
sudo netplan apply
```
Si hay errores en la sintaxis, prueba:
```bash
sudo netplan generate
sudo netplan apply
```

## 4. Cómo Elegir entre **NetworkManager** y **Netplan**
En Linux, generalmente **no se usan ambos a la vez** en una misma interfaz. Para saber cuál está en uso:

### **Ver si NetworkManager está activo**
```bash
systemctl is-active NetworkManager
```
Si está activo, NetworkManager está gestionando la red.

### **Ver si Netplan está en uso**
```bash
ls /etc/netplan/
```
Si hay archivos `.yaml`, Netplan podría estar gestionando la red.

### **Configurar el gestor de red**
- **Usar NetworkManager:** Modifica `/etc/netplan/*.yaml` para incluir:
  ```yaml
  network:
    version: 2
    renderer: NetworkManager
  ```
  Luego aplica los cambios con:
  ```bash
  sudo netplan apply
  ```

- **Usar Netplan en lugar de NetworkManager:** Modifica `/etc/netplan/*.yaml` para que use `networkd`:
  ```yaml
  network:
    version: 2
    renderer: networkd
  ```
  Luego aplica los cambios:
  ```bash
  sudo netplan apply
  ```
  Para deshabilitar NetworkManager:
  ```bash
  sudo systemctl stop NetworkManager
  sudo systemctl disable NetworkManager
  ```

---
Esta guía cubre las principales formas de configurar la red en Linux y cómo elegir entre **NetworkManager** y **Netplan**. Si necesitas más opciones avanzadas, dime y lo ampliamos. 🚀



