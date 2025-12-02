# 📌 **Archivos de configuración de Apache**

Apache organiza su configuración en varios archivos y carpetas, cada uno con un propósito claro. El conjunto forma un sistema modular, fácil de mantener.

---

## 🟦 **1. `apache2.conf` — Archivo principal (configuración global)**

```
/etc/apache2/apache2.conf
```

* Es el **archivo general del servidor**.
* Afecta **a todo Apache**, no a un sitio concreto.
* Define:

  * Política de seguridad global (`<Directory />`)
  * Parámetros generales del servidor
  * Inclusión de otros archivos
  * Límites globales

Los sitios web **no se configuran aquí**.

---

## 🟦 **2. `ports.conf` — Puertos que escucha Apache**

```
/etc/apache2/ports.conf
```

* Indica en qué puertos escucha Apache.
* Por defecto:

  ```
  Listen 80
  Listen 443
  ```

Los VirtualHosts heredan estos puertos.

---

## 🟦 **3. `sites-available/` — Sitios web disponibles**

```
/etc/apache2/sites-available/
```

* Contiene un archivo `.conf` **por cada sitio web**.
* Ejemplos:

  * `000-default.conf`
  * `sitio1.conf`
  * `sitio2.conf`

Aquí se define cada VirtualHost:

* `ServerName`
* `DocumentRoot`
* Reglas del sitio
* Autenticación
* SSL
* Directorios permitidos

No están activos hasta activarlos.

---

## 🟦 **4. `sites-enabled/` — Sitios web activados**

```
/etc/apache2/sites-enabled/
```

* Contiene enlaces simbólicos a los sitios activados.
* Se gestionan con:

  ```bash
  sudo a2ensite nombre.conf
  sudo a2dissite nombre.conf
  ```

Si aparece aquí, Apache lo carga.

---

## 🟦 **5. `mods-available/` y `mods-enabled/` — Módulos de Apache**

### 📁 `mods-available/`

* Todos los módulos instalados: SSL, autenticación, rewrite, etc.

### 📁 `mods-enabled/`

* Solo los activados.
* Se activan/desactivan con:

  ```bash
  sudo a2enmod nombre
  sudo a2dismod nombre
  ```

Ejemplos de módulos:

* `ssl`
* `rewrite`
* `auth_basic`
* `authnz_external`

---

## 🟦 **6. `conf-available/` y `conf-enabled/` — Configuraciones adicionales**

### 📁 `conf-available/`

* Pequeños ajustes globales opcionales (seguridad, charset, etc.).

### 📁 `conf-enabled/`

* Enlaces simbólicos a las configuraciones activas.

Se gestionan igual que los módulos:

```bash
sudo a2enconf nombre
sudo a2disconf nombre
```

---

## 🟦 **Resumen visual (ultrabreve)**

| Ubicación          | Contenido            | Función                      |
| ------------------ | -------------------- | ---------------------------- |
| `apache2.conf`     | Configuración global | Afecta a todo Apache         |
| `ports.conf`       | Puertos              | Define Listen 80/443         |
| `sites-available/` | Sitios web           | Donde se crean VirtualHosts  |
| `sites-enabled/`   | Sitios activos       | Apache solo carga estos      |
| `mods-available/`  | Módulos disponibles  | Todos los módulos instalados |
| `mods-enabled/`    | Módulos activos      | Los que Apache usa           |
| `conf-available/`  | Configs opcionales   | Ajustes extra                |
| `conf-enabled/`    | Configs activadas    | Apache las carga             |

