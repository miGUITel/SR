El mensaje de que `gateway4` está **deprecado** significa que se recomienda usar una alternativa más moderna en la configuración de **Netplan**. Esto sucede especialmente en versiones recientes de Ubuntu, donde se fomenta usar la directiva `routes` para definir la puerta de enlace.

### Configuración actualizada con `routes`
Aquí tienes cómo ajustar tu configuración para evitar el uso de `gateway4`:

```yaml
network:
  version: 2
  ethernets:
    enp1s0:
      addresses: [192.168.1.100/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

### Desglose de la sección `routes`
- `routes`: Se utiliza para definir rutas en la red.
  - `to: default`: Indica que esta es la ruta predeterminada (equivalente a la puerta de enlace).
  - `via: 192.168.1.1`: Especifica la dirección IP del gateway (puerta de enlace).

### Pasos para aplicar la configuración
1. Guarda el archivo de configuración en `/etc/netplan/`.
2. Verifica la configuración:
   ```bash
   sudo netplan try
   ```
3. Aplica los cambios:
   ```bash
   sudo netplan apply
   ```

### Verificar conectividad
Asegúrate de probar:
1. La dirección IP asignada:
   ```bash
   ip a
   ```
2. La tabla de rutas:
   ```bash
   ip route
   ```
   Deberías ver una línea como:
   ```
   default via 192.168.1.1 dev enp1s0
   ```
