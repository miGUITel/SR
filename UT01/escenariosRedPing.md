Estos cinco escenarios permiten practicar el subnetting con subredes IPv4 privadas. La información proporcionada disminuye progresivamente para que el alumnado complete los datos necesarios.

Debes ser capaz de configurar los escenarios con Windows Server 2019, Ubuntu Desktop y Ubuntu Server.

Completa y configura únicamente los datos solicitados en cada escenario. Puedes calcular información adicional para verificar la solución, pero no es obligatorio entregarla.

---

### **Escenario 1: Subred IPv4 privada con prefijo /26**
- **Dirección de red:** `192.168.1.0/26`
- **Dirección de broadcast:** `192.168.1.63`
- **Rango de hosts:** `192.168.1.1 - 192.168.1.62`
- **Máscara de subred:** `255.255.255.192`
- **Equipos a configurar:**  
  - PC1: `192.168.1.10`  
  - PC2: `192.168.1.20`  
  - PC3: `192.168.1.30`  
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 2: Subred IPv4 privada con prefijo /27**
- **Dirección de red:** `192.168.2.32/27`
- **Dirección de broadcast:** `192.168.2.63`
- **Rango de hosts:** `192.168.2.33 - 192.168.2.62`
- **Máscara de subred:** `255.255.255.224`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 3: Subred IPv4 privada con prefijo /28**
- **Dirección de red:** `192.168.3.64/28`
- **Máscara de subred:** `255.255.255.240`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 4: Subred IPv4 privada con prefijo /29**
- **Dirección de red:** `192.168.4.128/29`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 5: Subred IPv4 privada con prefijo /30**
- **Equipos a configurar:**  
  - PC1: `192.168.5.201`
  - PC2: `elige ip adecuada`  
- **Prueba de conectividad:** `ping` entre los dos equipos.

---

### **Instrucciones para los alumnos**
1. Asignar manualmente las direcciones IP a cada equipo dentro del rango de hosts.
2. Configurar la máscara de subred correspondiente en cada equipo.
3. Comprobar conectividad mediante el comando `ping` entre los equipos configurados.
   - En redes internas o Host-Only sin router, dejar vacíos la puerta de enlace y el DNS.
   - No utilizar `ping 8.8.8.8` como comprobación de la comunicación entre las máquinas del escenario.
4. Si hay problemas de conectividad, verificar:
   - Que todos los equipos tengan la máscara de subred correcta.
   - Que las direcciones IP no estén repetidas.
   - Que los cortafuegos no estén bloqueando las pruebas de `ping`.
   - Que las máquinas clonadas no tengan direcciones MAC duplicadas. Si las tienen, apágalas, genera nuevas direcciones MAC en VirtualBox y vuelve a iniciarlas.
   - Que los equipos estén conectados a la misma red interna o Host-Only de VirtualBox.

Para seguir el procedimiento completo, consulta [Comprobar una red interna o Host-Only](./diagnostico_red_virtual.md).
