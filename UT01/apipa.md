APIPA significa **Automatic Private IP Addressing**.

Es un mecanismo de Windows (y otros sistemas) que se activa cuando un equipo tiene configurado **obtener IP automáticamente** pero **no encuentra un servidor DHCP** que se la proporcione.

En ese caso, el sistema operativo se asigna a sí mismo una dirección en el rango **169.254.0.0/16**, con máscara **255.255.0.0**.

🔎 Consecuencias:

* Permite que varios equipos en la misma red local se comuniquen entre ellos **sin necesidad de DHCP**.
* Sin embargo, **no permite salir a Internet**, porque esas direcciones son solo para redes internas y no se enrutan.

En la práctica: si ves una IP 169.254.x.x en un equipo, significa que **no consiguió dirección del DHCP** y probablemente haya un problema de red.

