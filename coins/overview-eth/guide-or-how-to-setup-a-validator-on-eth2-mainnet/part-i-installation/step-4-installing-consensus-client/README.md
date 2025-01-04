# Step 4: Installing consensus client

Puede elegir entre Lighthouse, Lodestar, Teku, Nimbus o Prysm.

{% hint style="warning" %}
Solo se requiere un cliente de consenso por nodo.
{% endhint %}

## **Consensus Client Diversity**

* Para fortalecer la resiliencia de Ethereum contra posibles ataques o errores de consenso, se recomienda ejecutar un cliente minoritario para aumentar la diversidad de clientes.
* Encuentre la última distribución de clientes de consenso aquí: [https://clientdiversity.org](https://clientdiversity.org/)

<figure><img src="../../../../../.gitbook/assets/cd-c.png" alt=""><figcaption><p>Sept 2023 Client Diversity</p></figcaption></figure>

## Overview of Consensus Clients

{% hint style="info" %}
:shield: **Recomendación** :shield:: Teku, Nimbus, or Lodestar
{% endhint %}

### Lighthouse

* Lighthouse: Proyecto Ethereum 2.0 de código abierto de Sigma Prime, que sigue las especificaciones de investigación de la Fundación Ethereum.
* Características innovadoras: Implementa tecnologías avanzadas de cadena de bloques como consenso de prueba de participación, ejecución de transacciones paralelas y fragmentación (separación de estados).
* Gestión independiente: No está afiliado oficialmente a la Fundación Ethereum, se adhiere a sus pautas siempre que sean beneficiosas para el protocolo y la comunidad de Ethereum.
* Implementado en Rust: Prioriza la seguridad y la eficiencia a través de la elección del lenguaje.

### Lodestar

* Lodestar: Cliente de consenso de Ethereum de código abierto de ChainSafe Systems, conocido por su cadena de balizas lista para producción y su cliente validador.
* Producto estrella: Ideal para investigadores y desarrolladores debido a la rápida creación de prototipos y las capacidades de uso del navegador.
* Implementación de Typescript: Característica distintiva, se alinea con la familiaridad de millones de desarrolladores en todo el mundo.
* Experiencia en clientes ligeros: Investigación pionera, estandarización e implementación de clientes ligeros de Ethereum.
* Enfoque colaborativo: trabaja con otros implementadores, investigadores y desarrolladores para promover el uso de datos sin confianza de la cadena de bloques.

### Teku

* Teku (anteriormente Artemis): Cliente de consenso de Ethereum centrado en la empresa desarrollado por PegaSys, una división de ConsenSys.
* Escrito en Java: Lenguaje de programación maduro y ampliamente utilizado para un mayor atractivo institucional y requisitos de seguridad.
* Desarrolado por PegaSys: Una rama de ConsenSys dedicada a crear herramientas y clientes de Ethereum listos para la empresa.

### Nimbus

* Nimbus: Cliente de Ethereum de código abierto compatible con Ethereum 2.0 y Ethereum 1.0.
* Uso de recursos liviano: Diseñado para un rendimiento óptimo en sistemas integrados y dispositivos con recursos restringidos.
* Aplicación versátil: También es adecuado para ejecutarse junto con otras cargas de trabajo, beneficioso para los participantes que buscan minimizar los costos del servidor.
* Implementado en Nim: Escrito con el lenguaje de programación Nim.
* Mantenido por el equipo de Status.im.

### Prysm

* Prysm: Implementación completa de Ethereum 2.0 en el lenguaje de programación Go.
* Desarrollado por Prysmatic Labs.
* Se adhiere a la especificación oficial de Ethereum 2.0, evolucionando colectivamente a través de esfuerzos de investigación y desarrollo de varios equipos del ecosistema Ethereum, incluida la Fundación Ethereum.

## Comparison of Consensus Clients

<table><thead><tr><th>Client</th><th width="108">CPU Use</th><th width="111">RAM Use</th><th>Database Size</th><th>Time to sync head</th></tr></thead><tbody><tr><td><strong>Lighthouse</strong></td><td>Medium</td><td>6 GB</td><td>120 - 150 GB</td><td>Instant via checkpoint</td></tr><tr><td><strong>Lodestar</strong></td><td>Medium</td><td>8 GB</td><td>120 - 150 GB</td><td>Instant via checkpoint</td></tr><tr><td><strong>Teku</strong></td><td>Medium</td><td>10 GB</td><td>120 - 150 GB</td><td>Instant via checkpoint</td></tr><tr><td><strong>Nimbus</strong></td><td>Low</td><td>3 GB</td><td>120 - 150 GB</td><td>Instant via checkpoint</td></tr><tr><td><strong>Prysm</strong></td><td>Medium</td><td>6 GB</td><td>120 - 150 GB</td><td>Instant via checkpoint</td></tr></tbody></table>

#### Notes:

* A medida que las bases de datos se expanden más allá de los 300 GB de tamaño con el tiempo,lLa sincronización de puntos de control permite que los nodos se resincronicen de manera eficiente y reduzcan significativamente el tamaño de sus bases de datos mientras minimizan el tiempo de inactividad.
