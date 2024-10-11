Puedes seguir estos pasos para diagnosticar y solucionar el problema:

1. **Revisar el Log de Errores**: El comando `systemctl status isc-dhcp-server` (o `isc-dhcp-server.service` dependiendo de tu distribución) te mostrará detalles sobre el estado del servicio, incluidos errores. Además, revisa los logs específicos del servicio:
   ```bash
   sudo journalctl -u isc-dhcp-server
   ```
   Esto te dará más contexto sobre por qué falla.

2. **Verificar la Configuración**: A menudo, el problema está en el archivo de configuración. Verifica que no haya errores de sintaxis en `/etc/dhcp/dhcpd.conf`. Puedes hacer una validación rápida ejecutando:
   ```bash
   sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
   ```
   Esto te indicará si hay algún problema en el archivo de configuración.

3. **Comprobar Interfaz de Red**: Asegúrate de que el servicio DHCP esté configurado para escuchar en la interfaz correcta. En la configuración, revisa el archivo `/etc/default/isc-dhcp-server` y confirma que la interfaz asignada en `INTERFACESv4` sea `enp1s0` o la interfaz que corresponda.

4. **Verificar los Permisos y Propietarios de Archivos**: A veces, el problema puede deberse a permisos o al usuario que está intentando ejecutar el servicio. Asegúrate de que los archivos tengan los permisos adecuados.

5. **Revisar Conflictos de Puerto**: El DHCP utiliza el puerto 67. Asegúrate de que no haya otro servicio ocupando ese puerto:
   ```bash
   sudo lsof -i :67
   ```

Si tras estas verificaciones el problema persiste, comparte el mensaje específico de error que aparece al ejecutar `systemctl status isc-dhcp-server` para poder ofrecerte una orientación más precisa.