Te propongo una **secuenciación realista y pensada para SMR**, con **9 sesiones de 2–3 horas**, donde la teoría **siempre justifica una acción práctica**. No entro en guiones ni comandos concretos; es un **mapa de avance** coherente con tu presentación y con el RA 2.

---

## Enfoque general de la UT

* **Primera semana**: comprender *por qué existe DNS* y *cómo funciona*.
* **Segunda semana**: *instalar y configurar* un DNS funcional en red local.
* **Tercera semana**: *verificar, ampliar y trabajar en escenarios reales* (zonas, transferencias, pruebas).

La teoría **no se agota nunca en una sesión**: se introduce lo justo para poder hacer algo.

---

## SEMANA 1 — SENTIDO Y FUNCIONAMIENTO DEL DNS

### **Sesión 1 — Necesidad y concepto de resolución de nombres**

**Teoría**

* Problema de acceso a recursos por IP.
* Sistemas de nombres planos vs jerárquicos.
* NetBIOS, WINS y por qué hoy manda DNS.
* Primer contacto con el concepto de dominio.

**Práctica** [+](./S0301_hosts_OP.md)

* Resolución sin DNS (hosts).
* Observación del comportamiento del sistema cuando no hay resolución.
* Primeras comprobaciones simples de nombres ↔ IP.

👉 Objetivo didáctico: *que entiendan que DNS no es “opcional” en una red medianamente seria*.

---

### **Sesión 2 — Funcionamiento básico del DNS**

**Teoría**

* Cliente resolver y servidor DNS. [+](./S0302_nslookup_OP.md)
* Flujo general de una consulta.
* Caché DNS.
* Consultas recursivas e iterativas (sin exceso de detalle).

**Práctica**

* Uso básico de herramientas de consulta desde cliente.
* Diferenciar respuestas autoritativas y no autoritativas.
* Observación de la caché.

👉 Objetivo: *visualizar qué pasa cuando escriben un nombre*.

---

### **Sesión 3 — Estructura jerárquica del DNS** 

**Teoría**

* Árbol DNS: raíz, TLD, dominios, subdominios.
* FQDN.
* Zonas y por qué existen.
* Introducción a delegaciones (conceptual).

**Práctica** [+](./S0303_FQDN.md)

* Análisis de nombres reales.
* Descomposición de FQDN.
* Seguimiento conceptual de una resolución “desde la raíz”.

👉 Objetivo: *que entiendan la jerarquía antes de tocar un servidor*.

---

## SEMANA 2 — INSTALACIÓN Y CONFIGURACIÓN BÁSICA

### **Sesión 4 — Instalación de un servidor DNS**

**Teoría**

* Tipos de servidores: primario, secundario, caché.
* Qué significa “ser autoritativo”.

**Práctica** [+](./S0304_DNSWS19.md)

* Instalación del servicio DNS.
* Arranque, parada y verificación.
* Comprobación del servicio desde clientes.

👉 Objetivo: *perder el miedo a instalar y tocar el servicio*.

---

### **Sesión 5 — Zonas y resolución directa** [+](./S0305zonas.md)

**Teoría**

* Zona directa.
* Archivos de zona (concepto).
* Registros básicos: SOA, NS, A, CNAME.

**Práctica**

* Creación de una zona directa.
* Alta de registros A y CNAME.
* Comprobación desde cliente.

👉 Objetivo: *ver que “crear DNS” es crear datos coherentes*.

---

### **Sesión 6 — Resolución inversa y registros de servicio** [+](./s0306_inversa.md)

**Teoría**

* Zona inversa y su sentido real.
* Registro PTR.
* Introducción a MX y su papel en correo.

**Práctica**

* Creación de zona inversa.
* Asociación directa ↔ inversa.
* Pruebas de resolución inversa.
* Alta de registros MX simples.

👉 Objetivo: *entender que DNS afecta a otros servicios*.

---

## SEMANA 3 — ESCENARIOS REALES Y VERIFICACIÓN

### **Sesión 7 — Reenvíos, caché y resolución externa**

**Teoría**

* Reenviadores absolutos y condicionales.
* Sugerencias de raíz.
* DNS como intermediario.

**Práctica**

* Configuración de reenviadores.
* Resolución de dominios externos.
* Comparación de tiempos / comportamiento.

👉 Objetivo: *simular una red corporativa real*.

---

### **Sesión 8 — Transferencias de zona (trabajo en grupo)**

**Teoría**

* Necesidad de redundancia.
* Transferencias completas e incrementales.
* NOTIFY.

**Práctica**

* Configuración de servidor primario y secundario.
* Transferencia de zona entre equipos.
* Comprobación de sincronización.

👉 Objetivo: *trabajo cooperativo y comprensión de tolerancia a fallos*.

---

### **Sesión 9 — Verificación, diagnóstico y cierre**

**Teoría**

* Repaso funcional (no teórico).
* Qué mirar cuando “DNS no funciona”.
* Introducción conceptual a DDNS y DNSSEC.

**Práctica**

* Uso sistemático de herramientas de diagnóstico.
* Detección de errores provocados.
* Resolución guiada de fallos típicos.

👉 Objetivo: *pensar como técnico, no como alumno*.

---

## Cierre didáctico

Con esta secuencia:

* Cumples **todos los criterios del RA 2**.
* El alumnado **ve, toca y comprueba**.
* La teoría siempre tiene un **para qué inmediato**.
* El nivel es **exigente pero realista** para SMR.

Si quieres, el siguiente paso natural sería:

* Ajustar esta secuencia a **evidencias evaluables por sesión**,
* o decidir **qué sesiones son calificables y cuáles formativas**.
