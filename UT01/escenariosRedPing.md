Aquí tienes cinco escenarios con dirección de red, dirección de broadcast y rango de hosts, aplicando subnetting en redes Clase C. Cada escenario requiere que los alumnos configuren tres equipos en la red y verifiquen la conectividad mediante **ping**.

### Debes se capaz de configurar todos los escenarios con sistemas WS19, UBUNTU DESCKTOP y UBUNTU SERVER

---

### **Escenario 1: Red Clase C con máscara /26**
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

### **Escenario 2: Red Clase C con máscara /27**
- **Dirección de red:** `192.168.2.32/27`
- **Dirección de broadcast:** `192.168.2.63`
- **Rango de hosts:** `192.168.2.33 - 192.168.2.62`
- **Máscara de subred:** `255.255.255.224`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 3: Red Clase C con máscara /28**
- **Dirección de red:** `192.168.3.64/28`
- **Máscara de subred:** `255.255.255.240`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 4: Red Clase C con máscara /29**
- **Dirección de red:** `192.168.4.128/29`
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 5: Red Clase C con máscara /30**
- **Equipos a configurar:**  
  - PC1: `192.168.5.201`
  - PC2: `elige ip adecuada`  
- **Prueba de conectividad:** `ping` entre los dos equipos.

---

### **Instrucciones para los alumnos**
1. Asignar manualmente las direcciones IP a cada equipo dentro del rango de hosts.
2. Configurar la máscara de subred correspondiente en cada equipo.
3. Comprobar conectividad mediante el comando `ping` entre los equipos configurados.
4. Si hay problemas de conectividad, verificar:
   - Que todos los equipos tengan la máscara de subred correcta.
   - Que las direcciones IP no estén repetidas.
   - Que los cortafuegos no estén bloqueando las pruebas de `ping`.
   - Que la MAC no cree conflictos: apagar, refrescar en VBox y encender desde el SO.
   - Que los equipos estén en la misma red interna de VBox.
