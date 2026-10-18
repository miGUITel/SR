[Volver al recorrido de DNS](./SR03.md#aea-xvi)

# Herramientas de consulta DNS: ampliación opcional

Este documento amplía las consultas con `dig` y `host`. La herramienta común de la unidad es `nslookup`, explicada en los [apuntes del cliente DNS](./SR0310.md#1031-nslookup).

Estas herramientas realizan consultas DNS. No comprueban el archivo `hosts` ni garantizan que se utilice la caché del sistema. Se pueden usar herramientas de cliente sin instalar el servidor BIND9.

## dig

```text
dig [@servidor] nombre [tipo] [opciones]
```

Los corchetes indican elementos opcionales. `@servidor` selecciona el DNS consultado. Si falta, se usa la configuración de resolución que utilice la herramienta.

```bash
dig google.com MX
dig www.example.com A
dig -x 192.0.2.10
dig -h
```

El ejemplo inverso usa una IP de documentación. No presupone que exista un PTR público para ella. Para consultar un servidor concreto, escribe `@` seguido de la IP real del DNS.

En una respuesta, distingue el servidor consultado, el estado de la consulta, el tipo de registro y la sección de respuestas. `NOERROR` puede acompañar una respuesta sin registros del tipo solicitado. `NXDOMAIN` indica nombre inexistente. Un tiempo de espera agotado indica ausencia de respuesta a tiempo.

## host

```text
host [-t TIPO] nombre [servidor]
```

```bash
host www.example.com
host -t MX google.com
host 192.0.2.10
```

En `host`, el servidor opcional se escribe sin `@`. Una IP como argumento permite solicitar la resolución inversa.

Opciones de consulta adicional:

| Opción | Función |
|---|---|
| `-t TIPO` | Solicita un tipo concreto de registro. |
| `-v` | Muestra una salida más detallada. |
| `-a` | Consulta ANY con salida detallada. No garantiza obtener todos los registros. |
| `-l` | Solicita una transferencia AXFR de una zona, si el servidor la permite. No descubre todos los equipos de una red. |
| `-T` | Utiliza TCP para la consulta. |

Una transferencia de zona requiere permiso del servidor. No equivale a una consulta ordinaria de un nombre.

Referencia: [manuales oficiales de dig y host](https://bind9.readthedocs.io/en/stable/manpages.html).
