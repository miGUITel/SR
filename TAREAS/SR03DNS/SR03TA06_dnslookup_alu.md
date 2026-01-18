# PENDIENTE DE REVISIÓN, DEBEN UTILIZAR DOMINIOS COMO GOOGLE.COM, deben hacerla en casa, PUES EN EL CENTRO LOS DNS DE LA CONSEJERÍA pueden INTERFERIR

## **Uso de `nslookup` para Consultar el Funcionamiento del DNS**

### **Objetivo:**
Aprenderán a utilizar la herramienta `nslookup` para realizar consultas DNS y obtener diferentes tipos de registros, como registros A, AAAA, MX, NS, CNAME, y PTR. Además, se reforzarán los conceptos de servidores DNS autoritativos y recursivos, así como las resoluciones directa e inversa.

---

### **1. Realización de consultas básicas DNS (registro A)**

1. **Inicia el terminal** en tu sistema operativo (Linux, macOS o Windows).
2. Usa el comando `nslookup` para realizar una consulta del registro **A** (que devuelve la dirección IPv4) para un dominio:

   ```bash
   nslookup ejemplo.com
   ```

   - **Pregunta:**
     - ¿Qué dirección IP devuelve la consulta? ¿Por qué crees que se devuelve esta IP?
  

 ### **2. Consulta de un servidor DNS específico**


1. Realiza la misma consulta pero a un servidor DNS específico (por ejemplo, el de Google: `8.8.8.8`):

   ```bash
   nslookup ejemplo.com 8.8.8.8
   ```

   - **Pregunta:**
     - ¿Es la misma IP que en la consulta anterior? ¿Por qué es importante poder consultar diferentes servidores DNS?

### **3. Consulta de un registro AAAA (IPv6)**

4. Ahora, realiza una consulta de un registro **AAAA**, que proporciona la dirección IPv6 del dominio. Usa el siguiente comando:

   ```bash
   nslookup -query=AAAA ejemplo.com
   ```

   - **Pregunta:**
     - ¿El dominio tiene un registro IPv6 (AAAA)? Si es así, ¿por qué es importante tener direcciones IPv6?

### **4. Consulta de registros MX (Mail Exchange)**

5. Realiza una consulta para obtener los registros **MX**, que indican los servidores de correo electrónico para un dominio:

   ```bash
   nslookup -query=MX ejemplo.com
   ```

   - **Pregunta:**
     - ¿Qué servidores de correo se muestran? ¿Qué es la **prioridad** en un registro MX?

### **5. Consulta de registros NS (Name Server)**

6. Realiza una consulta para obtener los servidores de nombres (registros **NS**) que son autoritativos para el dominio:

   ```bash
   nslookup -query=NS ejemplo.com
   ```

   - **Pregunta:**
     - ¿Qué servidores DNS autoritativos se listan para el dominio?

### **6. Consulta de un alias o CNAME (Canonical Name)**

7. Ahora, realiza una consulta para obtener registros **CNAME**, que muestran un alias para un nombre de dominio:

   ```bash
   nslookup -query=CNAME www.ejemplo.com
   ```

   - **Pregunta:**
     - ¿El dominio tiene un registro CNAME? ¿Por qué se utilizan alias en los dominios?

### **7. Realización de una consulta inversa (registro PTR)**

8. Para realizar una consulta inversa (de IP a nombre de dominio), puedes utilizar el siguiente comando, pero debes reemplazar la IP con una dirección válida (en este caso, una IP de ejemplo):

   ```bash
   nslookup 192.0.2.1
   ```

   - **Pregunta:**
     - ¿Devuelve un nombre de dominio para la IP? ¿Cómo funciona la resolución inversa en el DNS?

---

### **8. Resolución con reenvío a otro servidor DNS**

9. Si tienes acceso a un servidor DNS interno o privado, puedes hacer consultas y reenviarlas a un servidor DNS local:

   ```bash
   nslookup ejemplo.local 192.168.1.1
   ```

   - **Pregunta:**
     - ¿Qué diferencias observas al consultar con un servidor DNS interno?

---

### **AMPLIACIÓN:** para tener un 10, realiza dos de los siguientes puntos

1. OPCIONAL repite las consultas para otro dominio distinto, como **www.google.com EN TU CASA**
2. OPCIONAL hazlo también para otros dos dominios reales **EN TU CASA**
3. OPCIONAL utiliza dig y/o host para repetir la práctica

---


