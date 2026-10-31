# NS y autoridad en DNS

[Volver al recorrido de DNS](./SR03.md#aea-xii) · [Tipos de servidores DNS](./SR0305.md)

Un registro NS anuncia un servidor autoritativo. La delegación, el reenvío y la transferencia de zona utilizan relaciones distintas entre servidores: no basta con ver que dos DNS se comunican para identificar cuál de ellas se está utilizando.

## 1. Servir una zona y anunciar sus servidores

Un DNS responde de forma **autoritativa para una zona** cuando la tiene configurada y cargada como fuente de sus datos, por ejemplo, como primaria o secundaria. El mismo DNS puede responder sin autoridad sobre otros nombres.

Los **registros NS de la propia zona** identifican mediante sus nombres DNS a los servidores autoritativos que se anuncian para ella. Sus direcciones se obtienen mediante registros A o AAAA. Publicar un NS que apunte a un servidor no configura ese servidor ni le crea la zona: también debe estar preparado para servirla. [Microsoft: zonas primarias y secundarias](https://learn.microsoft.com/en-us/windows-server/networking/dns/zone-types).

Por ejemplo, una zona `aula.test` puede anunciar a `ns1.aula.test` y `ns3.aula.test`. Uno mantiene la primaria y otro su copia secundaria; ambos son autoritativos para esa misma zona.

## 2. Delegación: el padre publica NS para una subzona

Supongamos que un DNS administra `centro.test` y se decide administrar `ventas.centro.test` como una zona separada.

La zona padre publica **NS cuyo nombre es el subdominio delegado**, indicando los servidores que atienden esa subzona. Estos servidores deben tener configurada la zona hija. El resolvedor puede seguir la referencia y preguntarles por nombres como `pc1.ventas.centro.test`.

La delegación necesita esos NS en el padre. Si los servidores tienen nombres dentro de la propia subzona, el padre también proporciona las direcciones de apoyo necesarias para localizarlos, llamadas **glue**. [RFC 9499: delegación y registros glue](https://datatracker.ietf.org/doc/html/rfc9499#section-7).

La relación es entre **zonas diferentes**: padre e hija. No es una transferencia de una copia de la misma zona.

## 3. Reenvío: configurar a qué DNS preguntar

En un reenvío, DNS1 recibe una consulta y, cuando necesita utilizar el reenviador, envía la consulta a DNS2 para obtener la respuesta.

En Windows se configura la **IP de DNS2 como reenviador**. No es necesario añadir un NS de DNS2 a la zona propia de DNS1. El reenvío general —también llamado absoluto en estos apuntes— se configura para la resolución general; el condicional selecciona destinos para determinados dominios. Las zonas propias y la caché siguen teniendo sus funciones. [Microsoft: reenvío DNS](https://learn.microsoft.com/en-us/windows-server/networking/dns/forwarding).

Por ejemplo, DNS1 administra `aula.test` y DNS2 administra `externo.test`. DNS1 puede obtener de DNS2 una respuesta sobre `recurso.externo.test` sin mantener una copia secundaria de `externo.test`.

## 4. Secundario: copiar la misma zona

Para que DNS3 obtenga una copia de `aula.test`, se configura esa zona como **secundaria**, con DNS1 como maestro. DNS1 debe permitirle la transferencia.

En Windows hay que distinguir dos opciones:

- **Autorizar la IP de DNS3 explícitamente:** la transferencia puede funcionar sin que DNS3 figure en los NS de la zona.
- **Autorizar solo a los servidores de la pestaña Servidores de nombres:** entonces DNS3 debe figurar en esa lista de NS.

Estas opciones controlan el permiso de transferencia. Para **anunciar DNS3 como otro servidor autoritativo de la zona**, se incorpora su NS a la lista correspondiente. Configurar un reenviador, autorizar una transferencia y publicar un NS son acciones diferentes. [Microsoft: permisos de transferencia](https://learn.microsoft.com/en-us/windows-server/networking/dns/manage-dns-zones#configure-zone-transfer-settings).

Primario y secundario mantienen la **misma zona**. No existe entre ellos una relación de padre y subdominio por el hecho de que uno sea secundario.

## 5. Comparación rápida

| Mecanismo | Qué se establece | Papel del NS |
|---|---|---|
| Delegación | Otra zona administra un subdominio. | El padre publica NS para la zona hija. |
| Reenvío | Un DNS encarga a otro resolver consultas. | El reenviador se configura por su IP; no necesita un NS en la zona propia. |
| Transferencia | Un secundario mantiene una copia de la misma zona. | Puede autorizarse por IP; los NS anuncian los servidores y algunas opciones de permiso utilizan esa lista. |

**Matiz de ampliación:** un servidor puede ser autoritativo sin estar anunciado en los NS publicados, como ocurre con un primario oculto que proporciona la zona a otros servidores. Por eso, la presencia de un NS y la capacidad real de servir la zona son comprobaciones relacionadas, pero distintas. [RFC 9499: servidores ocultos](https://datatracker.ietf.org/doc/html/rfc9499#section-6).
