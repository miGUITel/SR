# Direcciones anycast

Una dirección **anycast** puede asignarse a varios nodos. Cuando un equipo envía un paquete a esa dirección, la red lo encamina hacia uno de ellos según la mejor ruta disponible.

Anycast puede utilizarse tanto con **IPv4** como con **IPv6**. No identifica un tipo de dirección distinto visible para el usuario: son los protocolos de encaminamiento los que anuncian la misma dirección desde varias ubicaciones.

## Funcionamiento

1. Varios nodos anuncian la misma dirección IP.
2. Los routers comparan las rutas disponibles según sus métricas y políticas.
3. El paquete llega al nodo correspondiente a la ruta seleccionada.

El nodo elegido no tiene por qué ser el más cercano geográficamente, el de menor latencia ni el menos congestionado. Es el alcanzado por la ruta que el sistema de encaminamiento considera mejor en ese momento.

## Usos habituales

- **DNS**: varias instancias de un servidor comparten una dirección para mejorar la disponibilidad.
- **CDN**: el tráfico se dirige a una de las ubicaciones que anuncian el servicio.
- **Servicios distribuidos**: si una ubicación deja de anunciar la ruta, el tráfico puede dirigirse a otra.

## Ventajas y límites

- Mejora la disponibilidad al ofrecer varias ubicaciones para un mismo servicio.
- Puede reducir la distancia de red, aunque no garantiza la menor latencia.
- Distribuye tráfico como consecuencia del encaminamiento, pero no sustituye a un balanceador que conozca la carga real de cada servidor.
