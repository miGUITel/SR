# Diagnóstico y evidencias

El diagnóstico debe seguir un orden. Cambiar varios parámetros a la vez dificulta saber cuál era la causa.

## Orden de diagnóstico

1. Estado del adaptador en VirtualBox y opción de cable conectado.
2. Nombre de la red interna o selección de la red Host-Only.
3. Correspondencia entre la MAC de VirtualBox y la interfaz del sistema.
4. Estado de la interfaz dentro del sistema operativo.
5. Dirección IP y prefijo.
6. Tabla de rutas.
7. Comunicación con otra máquina de la misma subred.
8. Estado y puerto del servicio.
9. Puerta de enlace y DNS, únicamente si el escenario los necesita.

## Errores frecuentes

| Síntoma | Posible causa | Primera comprobación |
|---|---|---|
| Las máquinas no responden entre sí | Están conectadas a redes virtuales distintas | Nombre y modo de red en VirtualBox |
| Dos clones se comportan de forma extraña | Comparten dirección MAC | MAC de cada adaptador |
| Una máquina alcanza unas direcciones pero no otras | Dirección o prefijo incorrectos | IP y máscara de todos los equipos |
| Aparecen dos salidas posibles | Hay más de una ruta predeterminada | Tabla de rutas y gateways configurados |
| `ping` funciona pero SSH no | Servicio detenido, puerto cerrado o cortafuegos | Estado de SSH y puerto TCP 22 |
| La IP funciona pero el nombre no | DNS ausente o incorrecto | Servidor DNS y ruta para alcanzarlo |
| La máquina tiene una dirección `169.254.x.x` | No ha recibido respuesta DHCP | Modo de red y servicio DHCP esperado |

Consulta también [Comprobar una red interna o Host-Only](../UT01/diagnostico_red_virtual.md).

## Evidencias suficientes

Selecciona únicamente las pruebas que permitan reconstruir y verificar la configuración:

- una tabla con máquina, interfaz, MAC, modo de VirtualBox, IP y prefijo;
- la tabla de rutas cuando sea relevante;
- una prueba de comunicación entre las máquinas;
- una prueba del servicio solicitado;
- una captura o salida que muestre el fallo cuando haya diagnóstico;
- una explicación breve de la causa y del cambio que lo resolvió.

No es necesario capturar cada clic. Una captura sin contexto tampoco demuestra qué se ha configurado.

## Explicación del diagnóstico

Una explicación breve puede seguir esta estructura:

1. **Resultado esperado**: qué comunicación o servicio debía funcionar.
2. **Síntoma**: qué prueba fallaba.
3. **Causa**: qué configuración era incorrecta.
4. **Corrección**: qué se cambió.
5. **Comprobación final**: qué prueba demuestra que funciona.

<!--
IMAGEN SUGERIDA, NO INCLUIDA
Descripción: dos capturas recortadas del mismo terminal, una con una prueba fallida y otra con la prueba correcta después de modificar un único parámetro.
Finalidad: mostrar que una evidencia útil debe relacionar síntoma, corrección y resultado.
Ubicación propuesta: antes del apartado "Explicación del diagnóstico".
Texto alternativo: "Comparación de una prueba de red fallida y la misma prueba correcta después de la corrección".
Fuente recomendable: capturas propias de un escenario preparado por el docente para evitar datos y licencias ajenos.
-->
