# Comprobar conectividad y SSH

Realiza las pruebas en orden. Si una comprobación falla, corrige ese nivel antes de continuar.

## 1. Interfaces y direcciones

En Linux:

```bash
ip link show
ip addr show
ip route show
```

En PowerShell:

```powershell
Get-NetAdapter
ipconfig /all
route print
```

Comprueba que la interfaz está activa, la MAC es la esperada y la dirección pertenece a la subred del escenario.

## 2. Comunicación local

Haz `ping` a otra máquina de la misma red:

```text
ping <IP-del-otro-equipo>
```

Una red interna o Host-Only sin router no necesita una puerta de enlace ni un DNS. `ping 8.8.8.8` no comprueba la comunicación entre las máquinas del escenario.

## 3. Comprobar SSH

En Ubuntu Server, comprueba el servicio:

```bash
sudo systemctl status ssh
ss -lnt | grep ':22'
```

Desde otro equipo Linux o desde PowerShell:

```text
ssh <usuario>@<IP-del-servidor>
```

La primera conexión puede solicitar que confirmes la identidad del servidor. Después pedirá las credenciales del usuario remoto.

Si `ping` funciona pero SSH no, revisa el servicio, el puerto TCP 22 y el cortafuegos. Son comprobaciones distintas.

## 4. Salida exterior, solo si se solicita

Cuando el escenario incluya NAT:

1. comprueba la puerta de enlace recibida por la interfaz NAT;
2. prueba una dirección exterior;
3. comprueba finalmente la resolución de nombres.

Consulta la explicación completa en [Comprobar una red interna o Host-Only](../UT01/diagnostico_red_virtual.md).
