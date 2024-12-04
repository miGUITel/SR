## Cabecera que acompaña a una petición http 

```
Host: www.iesinfantaelena.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
Cookie: __utma=258216968.1370078030.1566469139.1566469139.1566472892.2; __utmz=258216968.1566469139.1.1.utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=(not%20provided)
Upgrade-Insecure-Requests: 1
```

---

### Explicación detallada

1. **`Host: www.iesinfantaelena.com`**  
   - **Descripción**: Indica el dominio al que se está realizando la solicitud HTTP.  
   - **Función**: Especifica el servidor o dominio objetivo de la petición. Esto es útil en servidores que alojan múltiples sitios (Virtual Hosting).  

2. **`User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0`**  
   - **Descripción**: Detalla información sobre el navegador y sistema operativo del cliente que realiza la solicitud.  
   - **Detalles**:
     - `Mozilla/5.0`: Compatibilidad general del navegador.
     - `(X11; Ubuntu; Linux x86_64)`: El sistema operativo usado es Linux en una máquina de 64 bits.
     - `rv:68.0`: La versión del motor Gecko (68.0).
     - `Firefox/68.0`: El navegador es Firefox versión 68.  

3. **`Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8`**  
   - **Descripción**: Especifica los tipos de contenido que el cliente puede procesar.  
   - **Detalles**:
     - `text/html`: Prefiere contenido HTML.
     - `application/xhtml+xml`: Acepta XHTML.
     - `application/xml;q=0.9`: XML es aceptado con una prioridad (q) de 0.9.
     - `*/*;q=0.8`: Acepta cualquier otro tipo de contenido con una prioridad menor (0.8).

4. **`Accept-Language: es-ES,es;q=0.8,en-US;q=0.5,en;q=0.3`**  
   - **Descripción**: Indica los idiomas preferidos del cliente, ordenados por prioridad.  
   - **Detalles**:
     - `es-ES`: Español de España con prioridad máxima.
     - `es;q=0.8`: Español genérico con prioridad 0.8.
     - `en-US;q=0.5`: Inglés de Estados Unidos con prioridad 0.5.
     - `en;q=0.3`: Inglés genérico con menor prioridad.

5. **`Accept-Encoding: gzip, deflate, br`**  
   - **Descripción**: Especifica las codificaciones de contenido aceptadas por el cliente para comprimir la respuesta.  
   - **Detalles**:
     - `gzip`: Compresión gzip.
     - `deflate`: Compresión deflate.
     - `br`: Brotli, un algoritmo de compresión más eficiente.

6. **`Connection: keep-alive`**  
   - **Descripción**: Indica que el cliente desea mantener la conexión abierta para reutilizarla en múltiples solicitudes.  
   - **Ventaja**: Reduce el tiempo necesario para abrir nuevas conexiones, mejorando la eficiencia.

7. **`Cookie: __utma=258216968.1370078030.1566469139.1566469139.1566472892.2; __utmz=258216968.1566469139.1.1.utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=(not%20provided)`**  
   - **Descripción**: Incluye cookies que el cliente envía al servidor.  
   - **Detalles**:
     - `__utma`: Identifica usuarios únicos y realiza un seguimiento de las visitas.
     - `__utmz`: Proporciona información sobre la fuente de tráfico (`utmcsr=google`, `utmccn=(organic)`).
     - `utmctr=(not%20provided)`: Indica que no se proporcionaron datos de búsqueda específicos.

8. **`Upgrade-Insecure-Requests: 1`**  
   - **Descripción**: Indica que el cliente prefiere utilizar HTTPS en lugar de HTTP si el servidor lo permite.  
   - **Función**: Mejora la seguridad de la comunicación.
