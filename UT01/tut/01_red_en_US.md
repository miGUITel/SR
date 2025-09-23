# 🌐 Configurar la tarjeta de red en **Ubuntu Server** (consola, netplan)

### 1. Ver el nombre de la tarjeta de red

Entra en la consola y escribe:

```bash
ip a
```

Verás algo como `ens33`, `enp0s3` o `eth0` → ese es el **nombre de la interfaz**.

---

### 2. Editar la configuración de red

Los archivos de **netplan** están en `/etc/netplan/`.
Normalmente se llama algo como `00-installer-config.yaml`.

Abrimos el archivo:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

---

### 3. Ejemplo de configuración

#### 🔹 Para IP automática (DHCP)

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: true
```

#### 🔹 Para IP fija (manual)

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: false
      addresses:
        - 192.168.1.50/24
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

⚠️⚠️⚠️⚠️⚠️⚠️ Ojo con la indentación (espacios), en YAML es muy importante. ⚠️⚠️⚠️⚠️⚠️⚠️

---

### 4. Guardar y aplicar cambios

* Guardamos en nano con: `Ctrl + O` → Enter → `Ctrl + X`.
* Aplicamos la configuración:

  ```bash
  sudo netplan apply
  ```

---

### 5. Comprobar

```bash
ip a          # Ver IP asignada
ping 8.8.8.8  # Probar conexión
```


