# Tarea 0107. Escenarios de red con máquinas virtuales

Configura cuatro escenarios por parejas para practicar los modos de red de VirtualBox, las subredes IPv4 y la comunicación mediante `ping`. Ya tienes preparadas Ubuntu Desktop (UD), Ubuntu Server (US), Windows Server 2019 (WS19) y Linux Mint (Mint).

## Preparación y comprobación común

Apaga las MV para ajustar sus adaptadores. Activa solo los necesarios, marca **Cable conectado** y relaciona cada adaptador con su interfaz mediante la MAC. Usa MAC distintas y elimina la configuración del escenario anterior; evita IP duplicadas en una red compartida.

Consulta los [modos de red](../UT01B/08_modos_red.md) y las [guías de configuración](../UT01B/04_configurar_interfaces.md). En Mint, selecciona IPv4 automático o manual desde los ajustes de la conexión.

**En todos los escenarios debes mostrar:**

1. **Hostname, IP y máscara o prefijo de MV1.**
2. **Hostname, IP y máscara o prefijo de MV2.**
3. **El comando `ping` desde MV1 a la IP de MV2 y su resultado completo, incluido el resumen.**

| Sistema | Identificación y configuración | Cuatro peticiones de ping desde MV1 |
|---|---|---|
| Linux: Mint, UD y US | `hostname` e `ip -4 addr show` | `ping -c 4 IP_MV2` |
| Windows Server 2019 | `hostname` e `ipconfig` | `ping -n 4 IP_MV2` |

Sustituye `IP_MV2` por la dirección real de destino. Con dos adaptadores, identifica la interfaz utilizada. Puedes usar una captura con ambas MV visibles o varias relacionadas, siempre legibles.

Las respuestas desde el destino demuestran conectividad IP básica. Un ping a tu propia IP, a `127.0.0.1`, a la puerta de enlace o a Internet **no demuestra comunicación entre las MV**. En NAT deberás interpretar un caso especial.

## 1. Red interna: Mint → UD

Una red aislada del aula y del anfitrión. Configura **un adaptador en Red interna** por MV, con el mismo nombre `SR0107-interna` y estos datos manuales:

| Dato | Valor |
|---|---|
| Red y máscara | `192.168.10.0/26` — `255.255.255.192` |
| Broadcast | `192.168.10.63` |
| Rango de hosts | `192.168.10.1` a `192.168.10.62` |
| MV1: Mint | `192.168.10.10` |
| MV2: UD | `192.168.10.20` |
| Puerta de enlace y DNS | Vacíos en ambas MV |

Desde Mint ejecuta `ping -c 4 192.168.10.20`: debe recibir respuestas. Explica por qué ambas IP pertenecen a la misma subred y por qué pueden comunicarse sin puerta de enlace. No hay un router en esta red.

## 2. Puente: UD → WS19

| Parámetro | Configuración en ambas MV |
|---|---|
| VirtualBox | Un adaptador en **Adaptador puente**, asociado a la tarjeta del anfitrión conectada al aula |
| IPv4 y DNS | Automáticos, mediante DHCP del aula |
| IP, máscara y puerta de enlace | Conservar las recibidas |

No asignes las IP de otros escenarios ni actives servidores DHCP en las MV. La máscara del aula puede coincidir con otra de la práctica; no debes cambiarla para hacerla diferente.

Anota IP y máscara de ambas MV, **calcula sus direcciones de red** e indica si pertenecen a la misma subred. Desde UD ejecuta `ping -c 4 IP_MV2`, usando la IP de WS19. Explica quién asignó las direcciones y cómo están conectadas las MV a la red física.

Se esperan respuestas si el aula permite la comunicación y Windows admite ICMP. Si Windows lo bloquea, habilita la regla de entrada de **solicitud de eco ICMPv4** para el perfil activo y el origen de la práctica, sin desactivar todo el cortafuegos. Si la infraestructura impide la comunicación, consulta al docente y documenta la limitación.

## 3. NAT: WS19 → US

Configura **un adaptador en NAT** por MV, con IPv4 y DNS automáticos, sin reenvío de puertos. Selecciona NAT, no «Red NAT».

Anota las IP y máscaras reales y **calcula sus direcciones de red**. Es habitual que ambas reciban `10.0.2.15/24`, máscara `255.255.255.0`.

Desde WS19 ejecuta `ping -n 4 IP_MV2`, usando la dirección que muestra US:

- **Si las IP coinciden**, estarás haciendo ping a la propia WS19: las respuestas no demuestran comunicación con US. Indícalo en el PDF.
- **Si son distintas**, conserva el intento y su resultado: NAT individual no proporciona comunicación directa entre estas MV con esta configuración.

Cada MV está detrás de su propio NAT. Explica por qué tener salida al exterior o direcciones del mismo rango no significa compartir una red virtual. **Aquí se evalúa la explicación del aislamiento, no un ping exitoso entre MV.**

## 4. NAT + Host-Only: US → Mint

Configura dos adaptadores por MV:

| Parámetro | Configuración |
|---|---|
| Adaptador 1 | **NAT**, IPv4 y DNS automáticos |
| Adaptador 2 | **Solo-anfitrión (Host-Only)**, misma red virtual en ambas MV |
| Red Host-Only y máscara | `192.168.20.0/28` — `255.255.255.240` |
| Interfaz Host-Only del anfitrión | `192.168.20.1` con esa máscara |
| DHCP de Host-Only | Desactivado |
| IP Host-Only de US y Mint | Elige dos direcciones manuales válidas y distintas |
| Puerta de enlace y DNS de Host-Only | Vacíos; se obtienen por NAT |

**Calcula el broadcast y el rango de hosts.** No asignes la dirección de red, el broadcast ni la IP reservada para el anfitrión.

Desde US ejecuta `ping -c 4 IP_MV2` hacia la **IP Host-Only de Mint**: debe responder. Muestra también `ip route` en US e identifica la ruta predeterminada por NAT. Explica la función de cada adaptador.

### Fallo de subred y corrección

1. Cambia solo la IP Host-Only de Mint a `192.168.20.20/28` y muestra su configuración.
2. Desde US ejecuta `ping -c 4 192.168.20.20` y conserva el resultado.
3. Calcula la nueva dirección de red de Mint. Explica por qué ya no hay comunicación directa por Host-Only: comparten red virtual, pero están en subredes distintas y no hay un router configurado entre ellas.
4. Restaura la IP inicial de Mint y conserva la configuración y el ping correcto final.

## Diagnóstico y entrega

Si una prueba falla, revisa: modo de VirtualBox y red compartida → cable e interfaz/MAC → IP y máscara → destino del ping → rutas → cortafuegos ICMPv4. Un fallo de ping no identifica por sí solo la causa. Cambia una cosa cada vez; consulta la [guía de diagnóstico](./diagnostico_red_virtual.md) si lo necesitas.

Entrega en **Aula Virtual un único PDF**, con tu nombre y cuatro apartados. Cada escenario incluirá:

- Capturas de los adaptadores de VirtualBox que identifiquen la pareja y el modo utilizado.
- Las tres evidencias comunes: **configuración de MV1, configuración de MV2 y comando ping con su resultado**.
- Los cálculos solicitados y una explicación breve de la comunicación o el aislamiento observado.

En el escenario 4 incluye además la ruta predeterminada y las evidencias del fallo y su corrección. No es necesario documentar cada clic ni repetir la creación o clonación de las MV.

Se valorarán la configuración, los cálculos, la interpretación y la claridad de las capturas. El aislamiento de NAT y el fallo provocado forman parte de la práctica.
