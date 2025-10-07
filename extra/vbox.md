# 🖥️ VirtualBox — Atajos de teclado y consejos de uso
![alt text](Virtualbox_logo.png)
Guía rápida para moverse con soltura en VirtualBox durante las prácticas de clase.

---

## ⌨️ Atajos de teclado más útiles

> 💡 La tecla **Host** suele ser **Ctrl derecha**, pero puedes cambiarla en  
> *Archivo → Configuración → Entrada → Tecla del Host*.

| **Acción** | **Atajo (Host = Ctrl derecha)** | **Descripción breve** |
|-------------|---------------------------------|------------------------|
| Capturar/soltar ratón y teclado | Host + I | Libera el control del ratón si queda “atrapado” dentro de la VM. |
| Pausar / Reanudar VM | Host + P | Detiene temporalmente la ejecución de la máquina. |
| Reiniciar VM | Host + R | Reinicia sin apagar completamente el sistema invitado. |
| Apagar (botón ACPI) | Host + H | Simula pulsar el botón físico de apagado. |
| Pantalla completa | Host + F | Cambia entre modo ventana y pantalla completa. |
| Escalar ventana | Host + C | Ajusta el tamaño de la ventana a la resolución del invitado. |
| Modo sin bordes (Seamless) | Host + L | Muestra las ventanas del invitado junto a las del host (requiere Guest Additions). |
| Captura de pantalla | Host + E | Guarda una imagen de la pantalla de la VM. |
| Mostrar menú de dispositivos | Host + D | Acceso rápido a discos, USB, etc. |
| Insertar imagen ISO | Host + I | Monta una ISO en la unidad de CD/DVD. |
| Cambiar entre VMs abiertas | Host + Tab | Cambia rápidamente entre máquinas activas. |

---

## ⚙️ Consejos prácticos de uso

| **Consejo** | **Explicación breve** |
|--------------|-----------------------|
| **1. Usa clonación enlazada** | Permiten volver a un punto anterior si algo sale mal. |
| **2. NAT para internet, puente para LAN** | NAT da acceso a internet; puente permite comunicación con otros equipos. |
| **3. Discos VDI dinámicos** | Ocupan solo el espacio que realmente se usa. |
| **4. Compacta los discos** | Menú: *Archivo → Herramientas → Compactar disco* para liberar espacio. |
| **5. Instala Guest Additions** | Mejora el rendimiento, resolución y el portapapeles compartido. |
| **6. Clona máquinas en lugar de reinstalar** | Usa “Clonar” y marca “Generar nueva MAC”. |
| **7. Guardar estado vs Apagar** | Guardar estado ahorra tiempo, pero no es ideal para mover la VM. |
| **8. Cambia la tecla del Host** | Si interfiere con otros atajos, cámbiala (por ejemplo, Alt derecha). |
| **9. Ajusta la resolución** | Menú *Ver → Escalado* o usa 125%-150% si las letras se ven pequeñas. |
| **10. Carpetas compartidas** | *Dispositivos → Carpetas compartidas → Añadir nueva carpeta* para intercambiar archivos fácilmente. |

---

## 📁 Configuración recomendada para prácticas de clase

| **Elemento** | **Valor recomendado** | **Motivo** |
|---------------|----------------------|-------------|
| **Memoria RAM** | 1 GB mínimo (Linux) / 2 GB (Windows Server) | Equilibrio entre rendimiento y recursos disponibles. |
| **Procesadores** | 1–2 | Evita sobrecargar el host. |
| **Disco duro** | VDI dinámico de 50 GB | Ahorra espacio en disco. |
| **Red** | Adaptador puente o NAT según la práctica | Flexibilidad según el tipo de servicio. |
| **Carpeta compartida** | Activada (solo lectura si es necesario) | Facilita el intercambio de ficheros profesor↔alumno. |



