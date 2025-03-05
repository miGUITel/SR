Aquí tienes cinco escenarios con dirección de red, dirección de broadcast y rango de hosts, aplicando subnetting en redes Clase C. Cada escenario requiere que los alumnos configuren tres equipos en la red y verifiquen la conectividad mediante **ping**.

---

### **Escenario 1: Red Clase C con máscara /26 (62 hosts)**
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

### **Escenario 2: Red Clase C con máscara /27 (30 hosts)**
- **Dirección de red:** `192.168.2.32/27`
- **Dirección de broadcast:** `192.168.2.63`
- **Rango de hosts:** `192.168.2.33 - 192.168.2.62`
- **Máscara de subred:** `255.255.255.224`
- **Equipos a configurar:**  
  - PC1: `192.168.2.35`  
  - PC2: `192.168.2.40`  
  - PC3: `192.168.2.50`  
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 3: Red Clase C con máscara /28 (14 hosts)**
- **Dirección de red:** `192.168.3.64/28`
- **Dirección de broadcast:** `192.168.3.79`
- **Rango de hosts:** `192.168.3.65 - 192.168.3.78`
- **Máscara de subred:** `255.255.255.240`
- **Equipos a configurar:**  
  - PC1: `192.168.3.66`  
  - PC2: `192.168.3.70`  
  - PC3: `192.168.3.75`  
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 4: Red Clase C con máscara /29 (6 hosts)**
- **Dirección de red:** `192.168.4.128/29`
- **Dirección de broadcast:** `192.168.4.135`
- **Rango de hosts:** `192.168.4.129 - 192.168.4.134`
- **Máscara de subred:** `255.255.255.248`
- **Equipos a configurar:**  
  - PC1: `192.168.4.130`  
  - PC2: `192.168.4.132`  
  - PC3: `192.168.4.134`  
- **Prueba de conectividad:** `ping` entre los tres equipos.

---

### **Escenario 5: Red Clase C con máscara /30 (2 hosts útiles, solo punto a punto)**
- **Dirección de red:** `192.168.5.200/30`
- **Dirección de broadcast:** `192.168.5.203`
- **Rango de hosts:** `192.168.5.201 - 192.168.5.202`
- **Máscara de subred:** `255.255.255.252`
- **Equipos a configurar:**  
  - PC1: `192.168.5.201`  
  - PC2: `192.168.5.202`  
  - PC3: **No puede usarse**, ya que solo hay dos hosts posibles. En este caso, podría configurarse un router para interconectar dos redes.  
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

Con estos cinco escenarios, los alumnos podrán practicar subnetting y la configuración de redes en un entorno controlado. 🚀