---
descripcion: >-
Staker empoderado, inspirado, desde casa. Gratis. Código abierto. Bienes públicos para
Ethereum.
---

# 🛡️ EthPillar: herramienta de configuración de una sola línea y administración de nodos TUI

{% hint style="success" %}
:heart: Apóyanos en **Gitcoin** GR20: [https://explorer.gitcoin.co/#/round/42161/26/34](https://explorer.gitcoin.co/#/round/42161/26/34)
{% endhint %}

## :new: ¿Qué es EthPillar?

:smile: **Instalador de nodos amigable**: ¿Aún no hay nodo? Te ayuda a instalar un nodo Ethereum (Nimbus+Nethermind) apilar en unos minutos. MEVboost incluido.

:floppy\_disk: **Facilidad de uso**: Ya no es necesario recordar comandos CLI. Acceda a las operaciones habituales de los nodos a través de una sencilla interfaz de usuario de texto (TUI).

:owl: **Fast Updates**: busque y descargue rápidamente la última versión de consenso/ejecución. ¡Menos tiempo de inactividad!

:tada:**Compatibilidad**: Entre bastidores, los comandos de nodo y la estructura de archivos son idénticos a los de las configuraciones de replanteo V2.&#x20;

{% estilo sugerencia="advertencia" %}
¿Ya estás ejecutando un Validador? EthPillar es compatible con [una configuración de participación de Coincashew V2.](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet)&#x20;
{% endhint %}

## :sunglasses: Vista previa

<figure><img src="../../.gitbook/assets/preview02.png" alt=""><figcaption><p>Menú principal</p></figcaption></figure>

<div>

<figure><img src="../../.gitbook/assets/preview01.png" alt=""><figcaption><p>Cliente de ejecución</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/preview03.png" alt=""><figcaption><p>Cliente de consenso</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/preview04.png" alt=""><figcaption><p>Validador</p></figcaption></figure>

</div>

<div>

<figure><img src="../../.gitbook/assets/preview05.png" alt=""><figcaption><p>Administración del sistema</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/preview06.png" alt=""><figcaption><p>Herramientas</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/preview07.png" alt=""><figcaption><p>Mevboost</p></figcaption></figure>

</div>

## :whale: Requisitos previos

* [Revise cómo funciona el staking y el hardware requisitos](guia-o-como-configurar-un-validador-en-eth2-mainnet/parte-i-instalacion/prerrequisitos.md)
* Una instalación de [Ubuntu](guia-o-como-configurar-un-validador-en-eth2-mainnet/parte-i-instalacion/prerrequisitos.md#configuracion-ubuntu).&#x20;
* Probado con Ubuntu 22.04 LTS
* También parece compatible con Linux Mint 21.2, Debian 12

## :triangular\_ruler: Opción 1: Instalación automatizada de una línea

Abra la terminal de Windows desde cualquier lugar escribiendo `Ctrl+Alt+T`.&#x20;

Para instalarlo, pega lo siguiente:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/coincashew/EthPillar/main/install.sh)"
```

## :handshake: Opción 2: Instalación manual

**Instalar actualizaciones y paquetes:**

```bash
sudo apt-get update && sudo apt-get install git curl ccze bc tmux
```

**Clonar el repositorio de ethpillar e instalar:**

```bash
mkdir -p ~/git/ethpillar
git clone https://github.com/coincashew/ethpillar.git ~/git/ethpillar
sudo ln -s ~/git/ethpillar/ethpillar.sh /usr/local/bin/ethpillar
```

#### Ejecutar ethpillar:

```bash
ethpillar
```

## :tada:Próximos pasos

{% hint style="success" %}
Felicitaciones por instalar un EthPillar, lo que hace que los nodos y el staking en el hogar sean más fáciles.
{% endhint %}

<details>

<summary>Paso adicional para nuevos operadores de nodos, nuevos validadores</summary>

**Paso 1: configure su red, reenvío de puertos y firewall.**&#x20;

* Con EthPillar, puede modificarse en:
* **Herramientas > Firewall UFW > Habilitar firewall con configuración predeterminada**
* El reenvío de puertos se configura [manualmente](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-2-configuring-node.md#configure-port-forwarding), según su enrutador.
* Confirme que el reenvío de puertos funciona con **Tools** > **Port Checker**
* Alternativamente, configure manualmente según la guía del manual. [Haga clic aquí para obtener una configuración de red detallada.](guía-o-cómo-configurar-un-validador-en-eth2-mainnet/part-i-installation/step-2-configuring-node.md#network-configuration)

**Paso 2: Configure su BIOS para que se encienda automáticamente después de un corte de energía**

Los pasos reales varían en función de la BIOS de su ordenador. Idea general: [https://www.wintips.org/setup-computer-to-auto-power-on-after-power-outage/](https://www.wintips.org/setup-computer-to- encendido automático después de un corte de energía/)

**Paso 3: Habilite el monitoreo y las alertas (opcional)**

Encontrar bajo:

* **Herramientas** > **Monitoreo**

**Paso 4: compara tu nodo (opcional)**

Asegúrese de que su nodo tenga suficiente rendimiento de CPU/disco/red.
* **Herramientas** > **Otro-script-de-banco**

</detalles>

<detalles>

<summary>Pasos adicionales para nuevos validadores</summary>

**Paso 1: Configurar las claves del validador**

* Familiarícese con la sección de la guía principal sobre [configurar sus claves de validador.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-5-installing -validador/configuración-validator-keys.md)
* Cuando esté listo para generar sus claves, vaya a **EthPillar > Validator Client > Generate/Import Validator Keys**

**Paso 2: sube deposit\_data.json a Launchpad**

* Para empezar a apostar en Ethereum como validador, debe enviar al Launchpad su archivo deposit\_data.json, que incluye los datos esenciales de la dirección de retirada, y abonar el depósito exigido de 32ETH por validador.

**Paso 3: ¡Felicidades!**&#x20;

* Ahora estás esperando en la cola de entrada [https://www.validatorqueue.com](https://www.validatorqueue.com/)

<!---->

* Comprobar la [próximos pasos de la guía principal](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part- i-instalación/paso-5-instalación-validador/siguientes-pasos) para ampliar conocimientos. Especialmente las preguntas frecuentes «¿Dónde están las recompensas por staking?»

</details>

## :joy: POAP

¿Te gusta EthPillar? [¡Apoya este bien público comprando una edición limitada de POAP!](https://checkout.poap.xyz/169495) <figure><img src="../../.gitbook/assets/3adf69e9-fb1b-4665-8645-60d71dd01a7b.png" alt=""><figcaption><p>El POAP de tu Enjoyor de EthPillar</p> </figcaption></figure>

**Enlace de compra:** [https://checkout.poap.xyz/169495](https://checkout.poap.xyz/169495)

ETH aceptado en Mainnet, Arbitrum, Base, Optimismo. :orar:

## :teléfono: Ponte en contacto

¿Tienes preguntas? Chatea con otros home stakers en [Discord](https://discord.gg/dEpAVWgFNB) o abrir PRs/issues en [Github](https://github.com/coincashew/ethpillar).&#x20;

Código fuente abierto disponible aquí: [https://github.com/coincashew/EthPillar](https://github.com/coincashew/EthPillar)

## :corazón: Donaciones

Si deseas apoyar este proyecto de bien público, encuéntrenos en la próxima Gitcoin Grants.

nuestra direccion de donacion es [0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0](https://etherscan.io/address/0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0) o coincashew.eth

## :ballot\_box\_with\_check: Cómo actualizar

{% tabs %}
{% tab title="Actualización de TUI" %} Al abrir EthPillar,

* Vaya a **Administración del sistema > Actualizar EthPillar** y luego salga y vuelva a iniciar.
{% endtab %}

{% tab title="Actualización manual" %}
Desde una terminal, extraiga las últimas actualizaciones de Git.

```bash
cd ~/git/ethpillar
git pull
```
{% endtab %}
{% endtabs %}

## :tada: Créditos

Un agradecimiento especial a [accidental-green](https://github.com/accidental-green) green/validator-install) por su trabajo pionero en herramientas de validación de Python, que sin querer ha encendido la inspiración y la dirección de este proyecto. Estamos construyendo sobre sus cimientos innovadores al bifurcar su código de instalación de validador. Un sincero agradecimiento a accidental-green por ¡Sus contribuciones revolucionarias al ecosistema Ethereum de código abierto!
