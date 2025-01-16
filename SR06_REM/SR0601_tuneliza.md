La **tunnelización SSH** (o **SSH tunneling**) es una técnica que permite encapsular tráfico de red a través de una conexión SSH segura. Es útil para acceder a recursos remotos de forma segura, saltar firewalls o enrutar tráfico a través de un servidor intermedio.

Se pueden crear **túneles SSH** en dos formas principales: **local** y **remota**.
---

## ¿Qué es la túnelización SSH?

Cuando se establece una conexión SSH, el tráfico entre el cliente y el servidor viaja de forma cifrada. Con la túnelización, este túnel puede usarse no solo para enviar comandos a un servidor, sino también para redirigir tráfico de red de otras aplicaciones.

---

## Tipos de túneles SSH

### 1. **Túnel SSH local**

- **Función**: Redirige el tráfico desde un puerto local del cliente hacia un destino remoto a través del servidor SSH.
- **Propósito**: Acceder a servicios o recursos que están disponibles únicamente desde el servidor remoto.
  
#### Ejemplo típico:
Supongamos que tienes un servidor remoto (servidor.com) con una base de datos escuchando en el puerto `5432` de su red interna. Si quieres acceder desde tu máquina local, pero la base de datos no es accesible directamente, puedes crear un túnel SSH.

```bash
ssh -L 1234:localhost:5432 usuario@servidor.com
```

- **`-L`**: Especifica el túnel local.
- **`1234`**: Puerto en tu máquina local.
- **`localhost:5432`**: Dirección y puerto en el servidor remoto.

Luego, puedes conectarte desde tu máquina local como si la base de datos estuviera corriendo localmente:

```bash
psql -h localhost -p 1234
```

---

### 2. **Túnel SSH remoto**

- **Función**: Permite redirigir tráfico desde un puerto en el servidor SSH hacia una máquina o puerto específico en tu máquina local.
- **Propósito**: Exponer servicios en tu máquina local hacia el servidor remoto (y, opcionalmente, hacia otras máquinas conectadas a él).

#### Ejemplo típico:
Supongamos que tienes una aplicación local en tu máquina escuchando en el puerto `8080` y deseas que sea accesible desde el servidor remoto (servidor.com).

```bash
ssh -R 1234:localhost:8080 usuario@servidor.com
```

- **`-R`**: Especifica el túnel remoto.
- **`1234`**: Puerto en el servidor remoto.
- **`localhost:8080`**: Dirección y puerto en tu máquina local.

Cualquier conexión al puerto `1234` del servidor remoto será redirigida a tu máquina local en el puerto `8080`.

---

## Diferencias entre túneles local y remoto

| **Aspecto**           | **Túnel Local**                                      | **Túnel Remoto**                                     |
|------------------------|-----------------------------------------------------|-----------------------------------------------------|
| **Dirección del tráfico** | De la máquina local al servidor remoto             | Del servidor remoto hacia la máquina local          |
| **Uso típico**         | Acceso a servicios remotos desde tu máquina local    | Exponer servicios locales al servidor remoto        |
| **Opciones SSH**       | `-L [puerto_local]:[destino_remoto]:[puerto_remoto]` | `-R [puerto_remoto]:[destino_local]:[puerto_local]` |
| **Ejemplo de uso**     | Acceso a una base de datos remota                    | Exposición de una app local a un servidor público   |

---
