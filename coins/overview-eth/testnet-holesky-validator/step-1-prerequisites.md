# Primer paso: Prerequisitos

## :rocket: Como funcionan las pruebas de participación en Ethereum

1. Consigue un dispositivo(laptop, pc de escritorio, servidor) o renta un VPS (servidor en la nube): necesitas correr un nodo para hacer la prueba.
2. Sincroniza una capa de ejecución de cliente
3. Sincroniza un algoritmo de consenso de prueba de participación
4. Genera tus claves de validación e importalas a tu cliente de validación
5. Supervise y mantenga su nodo

Un nodo de ethereum consisten en la capa de ejecución + la capa de consenso.

Un nodo de prueba de participación de ethereum es el anterior + un cliente validador

<figure><img src="../../../.gitbook/assets/client-stack.png" alt=""><figcaption><p></p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/eth-validator-diagram.png" alt=""><figcaption><p>Foto: Ejecución / Consenso / Validador</p></figcaption></figure>

## :wave: Introducción

Esta guia esta escrita para aspirantes a manejo de nodos de prueba de participación que estén familiarizados con lineas de comando que hayan sido probadas con el cliente de Ubuntu 22.04.1 LTS. Querrás una nube dedicada VPS o una pc/servidor/laptop local corriendo una version de Ubuntu, limpia preferentemente.

### Terminología

Su nodo de ethereum puede ser

* **Local:** En una laptop/pc/ NUC a la que le puedas concetar fisicamente un teclado/mouse/monitor.
* **Remoto:** En la nube o en un VPS

Si su nodo de prueba de participación es **remoto**, entonces debería agregar una SSH.

Si esta usando un VPS o un nodo **remoto** , instale e inicie el cliente SSH para su sistema operativo:

**Windows**: [PuTTY](https://www.puttygen.com/download-putty)

**MacOS y Linux**: Para la terminal, use el código nativo:

```
ssh <Tunombredeusuario>@<LaIPdetuservidor>
```

Aquí hay una [guía de Makeuseof](https://www.makeuseof.com/tag/beginners-guide-setting-ssh-linux-testing-setup/) para conectar por SSH tu **nodo remoto.**

## :hammer\_pick: Cómo correr comandos

* Los comandos corren en una ventana terminal o un ssh terminal.
* Los comandos que inician con `sudo` pedirán su contraseña al inicio, y periodicamente despues.

## :woman\_technologist: Habilidades para operar un nodo de prueba de participación

Como un validador de Ethereum, deberías de poseer las siguientes habilidades:

* Conocimiento operacional de como establecer, correr y mantener un cliente de consenso de ETH, cliente de ejecución y un validador constantemente
* Un compromiso a largo plazo para mantener tu validador 24/7/365
* Habilidades básicas de operación de sistemas

## :man\_technologist: Experiencia requerida para ser un apstador de ETH exitoso

* Haber examinado los vastos tomos de ["Conocimiento base de apostadores de ETH"](https://docs.ethstaker.cc/ethstaker-knowledge-base/)
* Haber aprendido lo escencial viendo ["Introducción a ETH2 y apuestas para principiantes" de Superphiz](https://www.youtube.com/watch?v=tpkpW031RCI)
* Haber cursado o participar activamente el [Curso de estudio para dominar ETH2](https://ethereumstudymaster.com)
* Haber leido [8 cosas que cada validador de ETH2 debería saber.](https://medium.com/chainsafe-systems/8-things-every-eth2-validator-should-know-before-staking-94df41701487)

## :reminder\_ribbon: **Requisitos mínimos para establecer nodos**

* **Sisstema operativo:** Linux de 64 bits (i.e. Ubuntu 22.04.1 LTS Serviodor o PC)
* **Procesador:** CPU de 2 nucleos Intel Core i5–760 o AMD FX-8100 o alguno más potente
* **RAM:** 16GB RAM
* **Almacenamiento:** 1TB SSD para redes de prueba
* **Internet:** Conección de banda ancha estable con una velocidad de almenos 5 Mbps para carga y descarga.
* **Plan de datos**: Al menos 2 TB mensuales.
* **Fuente eléctrica:** Confiablel. Mitigar con un [Suministro Eléctrico ininterrumpido (UPS)](https://www.lifewire.com/best-uninterrupted-power-supplies-4142625).
* **Balance de ETH:** Al menos 32 ETH y algunos ETH fondo para tarifas por transacción de depósitos
* * **Cartera virtual**: [Rabby](https://rabby.io/) Cartera instalada

## :man\_lifting\_weights: Requisitos recomendados para establecer nodos

{% Tipo de pista="información" %}
Una vez acabado el testeo de redes de replanteo, esta configuración de hardware sera la ideal para establecer los nodos.
{% Fin de la pista %}

* **Sistema operativo:** Linux de 64-bit (i.e. Ubuntu 22.04.1 LTS Servidor o pc)
* **Processor:** Una CPU de 4 núcleos, Intel Core i7–4770 o AMD FX-8310 o más potente
* **Memoria:** 32GB RAM
* **Almacenamiento:** 4TB NVME
* **Internet:** Red de banda ancha estable con una velocidad al menos de 10mbps sin límite de uso.
* **Plan de datos**: Al menos 2 TB por mes. Lo ideal seria que no hubiera límite de datos
* **Suministro eléctrico:** Una fuente eléctrica confiable con un[Flujo eléctrico ininterrumpido (UPS)](https://www.lifewire.com/best-uninterrupted-power-supplies-4142625).
* **EBalance de ETH:** Almenos 32 ETH y algunos ETH for tarifas de transacción de depósitos
* **Cartera virtual**: [Rabby](https://rabby.io/) Una cartera instalada
{% Tipo de pista="información" %}
:desktop: **Construcciones de Hardware**: Para ejemplificar construcciones de hardware de replanteo, checa [Guía de hardware de rocketpool(https://github.com/rocket-pool/docs.rocketpool.net/blob/main/docs/guides/node/local/hardware.md#example-setups) y [Ejemplos de hardware de apostadores de ETH](https://docs.ethstaker.cc/ethstaker-knowledge-base/hardware/hardware-examples).
{% Fin de la pista %}

{% Tipo de pista="información" %}
:cd: **Sugerencias de almacenamiento**: Checa lo siguiente para encontrar tu NUVME o SSD ideal.

* [**Consejos de almacenamiento de Yorick**](https://gist.github.com/yorickdowne/f3a3e79a573bf35767cd002cc977b038): Consulte las más y menos grandes recomendaciones de Yorick de SSDs para nodos de ethereum
* [**Lista de las mejores SSD**](https://docs.google.com/spreadsheets/d/1B27\_j9NDPU3cNlj2HKcrfpJKHkOf-Oi1DbuuQva2gT4/edit)**:** Las SSD más apropiadas están en un rango medio o superior
{% Fin de la pista %}

<figure><img src="../../../.gitbook/assets/ethereum-inside.png" alt=""><figcaption><p>Ethereum Staking Node</p></figcaption></figure>

{% Estilo de psita="Exito" %}
:sparkles: **Tip para validadores pro**: Es altamente recomendado que inicies con una instancia completamente nueva de un sistema operativo, máquina virtual y/o máquina. Ahorra dolores de cabeza evitando reusar llaves de redes de prueba, wallets o bases de datos para tu validador.
{% fin de la pista %}

## Nodo Local vs Nodo remoto

**Decisión**: Correré mi nodo de prueba de participación localmente o rentaré un servidor en la nube de VPS remotamente? A continuación hay una lista de criterios que lo ayudarán a decidir

|       Criterio       | Nodo Local                                                                                                                                         | Nodo remoto                                                                                                                                                                |
| :------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   Costos continuos |Bueno - Sin comisiones además de las facturas de internet y luz                                                                                             |Malo - Tarifas de alquiler mensuales o anuales recurrentes                                                                                                                         |
| Mantenimiento de hardware | Malo - Resuelve usted mismo los problemas con el hardware.                                                                                                         | Bueno - Incluido y cubierto con el proveedor de hoasting.                                                                                                                               |
|       Internet       | Malo - Puede alentar el internet y acabarse el plan de datos (si no es ilimitado). Presupuesto para planes de 2TB mensuales.                           | Bueno - Existen planes bastante generosos y que son más que suficiente para nodos de Ethereum                                                                                                   |
|      Fiabilidad    | Malo - Se encarga usted mismo de UPS, conecciones de internet redundantes, problemas técnicos                                                                  | Bueno - Suelen alojarse en centros de información con multiples conecciones a energía/a internet.                                                                                                       |
|  Barrera para entrar   | <p>Bueno - Puedes reutilizar hardware ya existente<br><br>Malo - El costo inicial de una computadora es alto</p>                            | Bueno - Rentar un VPS puede requerrir una inversion inicial menor que además se puede pagar en meses.                                                                                       |
|   Decentralization   | Bueno - El reeplanteamiento en casa es el**Estandart dorado** para la descentralización de Ethereum!                                                       | Malo - Host en la nube de VPS como [Netcup](https://www.netcup.eu/bestellen/produkt.php?produkt=3026) o los dispositivos en la web de amazon son, por naturaleza, centralizados.                        |
|     Customización    | Bueno - Mayor control sobre las configuraciones del hardware                                                                                               | Malo - Pueden ser opciones limitadas y el hardware podría compartirse. Por ejemplo, un error común es falta de espacio en el servidor de velocidad I/O (IOPS).                                      |
|       Seguridad       | Bueno - Tan seguro como tu casa y tu [OSPEC](https://en.wikipedia.org/wiki/Operations\_security)                                             | <p>Bueno - Centros de datos profesionales de nivel empresarial.</p><p>Malo - No es ni tu hardware ni tu nodo. Es posible que tu proveedor de hoasting pueda acceder a tu nodo.</p> |
|        Libertad      | <p>Bueno - Haz lo que desees. Planea tus propias mejoras.<br><br>Malo - Con una gran libertad y poder, te vuelves el único responsable de tu nodo.</p> | <p>Bueno - Manejado profesionalmente.</p><p><br>Malo - A merced de las acciones del anfitrión, es posible que se produzcan interrupciones en el centro de datos.</p>                                               |

## :tools: Configurando Ubuntu

Con tu nodo local o remoto ya listo, necesitas descargar el sistema operativo. Esta guía habla de como instalar Ubuntu 22.04.1 LTS.

* Para instalar **el servidor o escritorio de UBUNTU**, sigue esta [guía](https://docs.ethstaker.cc/ethstaker-knowledge-base/tutorials/installing-linux).

{% Tipo de pista="información" %}
**Recommendacióbn**: A headless (no monitor) install del **Servidor de Ubuntu** en **una** NUC/laptop/pc/VPS mejora la fiabilidad y seguridad. :fire: no uses este sistema para email/navegar en linea/videojuegos/redes sociales. :fire:
{% Fin de la pista %}

{% Tipo de pista="advertencia" %}
**Tip**: Cuando instales el servidor de Ubuntu, Asegurate de haber seleccionado “**Usar un disco entero**” en la pantalla **Configuración de almacenamiento guiada**. La siguiente pantalla será la de **Configuración de akmacenamiento** , Asegurese de que su configuración este usando todo el disco. Un [error común](../guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-iii-tips/using-all-available-lvm-disk-space.md) es que la configuración estandart de Ubuntu solo usa 200GB.
{% Fin de la pista %}

## :El arte de\_la maestría: Configurar Rabby

Cuando llegue el momento de validar el deposito de tus 32ETH, necesitaras una cartera digital para transferir los fondos al contrato de depósito de la cadena

* Para instalar rabby, visita su [sitio oficial.](https://rabby.io/)

## :jigsaw: Descripción general del nodo validador de alto nivel

{% Estilo de pista="información" %}
At the end of this guide, you will build a staking validator node that hosts three main components in two layers: consensus layer consists of a consensus client, also known as a validator client with a beacon chain client. The execution layer consists of a execution client, formerly a eth1 node.

**Cliente del validador** - Responsable de producir nuevos bloques y en los atestados de la cadena de tocino y cadena de cristales.

**Cliente de consenso** - Responsable de manejar el estado de la cadena de tocino, barajado del validador, y más.

**Cliente de ejecución** - Regula los depositos del validador de la red principal de ETH a la cadena de tocino del cliente.
{% Fin de la pista %}

![Cómo los nodos de ethereum encajan juntos con Leslie the Rhino, la mascota que lleva el nombre de la científica informática estadounidense Leslie Lamport](../../../.gitbook/assets/eth2network.png)
