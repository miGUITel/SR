# Comprobar una red interna o Host-Only

En una práctica de red conviene comprobar primero el escenario que se ha construido. La salida a Internet es una prueba distinta y solo tiene sentido cuando el escenario la incluye.

## Puerta de enlace

La puerta de enlace permite enviar tráfico hacia **otras redes**. Para que funcione debe existir un router en la dirección configurada y ese router debe poder reenviar los paquetes.

En una red interna o Host-Only formada únicamente por varias máquinas de la misma subred no hay ningún router. Por tanto, esa interfaz no necesita puerta de enlace. Escribir una dirección ficticia provoca que el equipo envíe hacia un dispositivo inexistente los paquetes destinados a otras redes y dificulta el diagnóstico.

Cuando una máquina virtual tiene dos adaptadores, se utilizará normalmente esta distribución:

- **NAT**: configuración automática, ruta predeterminada y DNS para acceder al exterior.
- **Red interna o Host-Only**: dirección estática para comunicarse con las máquinas del escenario, sin puerta de enlace ni DNS por defecto.

De este modo solo existe una ruta predeterminada, normalmente la proporcionada por NAT.

## DNS

Un servidor DNS traduce nombres, como `servidor.example`, en direcciones IP. No interviene cuando la prueba se realiza directamente contra una dirección IP.

Configurar `8.8.8.8` en una interfaz aislada no permite acceder a él. Para utilizarlo tendría que existir una ruta real hacia Internet. En un escenario sin salida exterior se dejan vacíos los campos de DNS, salvo que la práctica incluya un servidor DNS local.

## Por qué `ping 8.8.8.8` no es la primera prueba

Un `ping` a `8.8.8.8` intenta comprobar un trayecto hacia Internet. No demuestra que dos máquinas de la red interna o Host-Only puedan comunicarse entre sí. También puede fallar por causas externas al escenario.

La primera prueba de conectividad debe dirigirse a otra máquina de la misma subred. Así se comprueban el adaptador de VirtualBox, la dirección IP, el prefijo y la comunicación local.

## Orden de comprobación

1. Comprobar que la interfaz está activa y que su MAC coincide con el adaptador esperado.
2. Comprobar la dirección IP y el prefijo o máscara.
3. Comprobar las rutas y confirmar que la interfaz aislada no introduce una ruta predeterminada incorrecta.
4. Hacer `ping` a otra máquina del escenario.
5. Probar el servicio solicitado, por ejemplo SSH.
6. Si el escenario incluye salida exterior, probar la puerta de enlace.
7. Después, probar una dirección pública y finalmente la resolución de nombres.

## Comandos de referencia

### Linux

```bash
ip link show
ip addr show
ip route show
ping <IP-del-otro-equipo>
```

### Windows

```powershell
Get-NetAdapter
ipconfig /all
route print
ping <IP-del-otro-equipo>
```

Una respuesta a `ping` confirma conectividad IP básica. No confirma por sí sola que DNS, SSH u otro servicio estén configurados correctamente; cada servicio debe comprobarse de forma específica.
