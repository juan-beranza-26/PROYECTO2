---
description: >-
  Empowered, inspired, home staker. Free. Open source. Public goods for
  Ethereum.
---

# 🛡️ EthPillar: one-liner setup tool and node management TUI

{% hint style="success" %}
:heart: Support us on **Gitcoin** GR20: [https://explorer.gitcoin.co/#/round/42161/26/34](https://explorer.gitcoin.co/#/round/42161/26/34)
{% endhint %}

## :new: What is EthPillar?

:smile: **Friendly Node Installer**: ¿Aún no hay nodo? Te ayuda a instalar un nodo Ethereum (Nimbus+Nethermind) apilar en unos minutos. MEVboost incluido.

:floppy\_disk: **Ease of use**: Ya no es necesario recordar comandos CLI. Acceda a las operaciones habituales de los nodos a través de una sencilla interfaz de usuario de texto (TUI).

:owl: **Fast Updates**: Encuentre y descargue rápidamente la última versión de consenso/ejecución. ¡Menos tiempo de inactividad!

:tada:**Compatibility**: Entre bastidores, los comandos de nodo y la estructura de archivos son idénticos a los de las configuraciones de replanteo V2.&#x20;

{% hint style="warning" %}
Already a running a Validator? EthPillar is compatible with [a Coincashew V2 Staking Setup.](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet)&#x20;
{% endhint %}

## :sunglasses: Preview

<figure><img src="../../.gitbook/assets/preview02.png" alt=""><figcaption><p>Main Menu</p></figcaption></figure>

<div>

<figure><img src="../../.gitbook/assets/preview01.png" alt=""><figcaption><p>Execution Client</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview03.png" alt=""><figcaption><p>Consensus Client</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview04.png" alt=""><figcaption><p>Validator</p></figcaption></figure>

</div>

<div>

<figure><img src="../../.gitbook/assets/preview05.png" alt=""><figcaption><p>System Administration</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview06.png" alt=""><figcaption><p>Tools</p></figcaption></figure>

 

<figure><img src="../../.gitbook/assets/preview07.png" alt=""><figcaption><p>Mevboost</p></figcaption></figure>

</div>

## :whale: Prerequisites

* [Review how staking works and the hardware requirements](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/prerequisites.md)
* An [Ubuntu](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/prerequisites.md#setup-ubuntu) installation.&#x20;
  * Tested working with Ubuntu 22.04 LTS
  * Also appears compatible with Linux Mint 21.2, Debian 12

## :triangular\_ruler: Option 1: Automated One-Liner Install

Abra la terminal de windows desde cualquier lugar escribiendo `Ctrl+Alt+T`.&#x20;

Para instalarlo, pega lo siguiente:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/coincashew/EthPillar/main/install.sh)"
```

## :handshake: Option 2: Manual Install

**Install updates and packages:**

```bash
sudo apt-get update && sudo apt-get install git curl ccze bc tmux
```

**Clone the ethpillar repo and install:**

```bash
mkdir -p ~/git/ethpillar
git clone https://github.com/coincashew/ethpillar.git ~/git/ethpillar
sudo ln -s ~/git/ethpillar/ethpillar.sh /usr/local/bin/ethpillar
```

#### Run ethpillar:

```bash
ethpillar
```

## :tada:Next Steps

{% hint style="success" %}
Congrats on installing a EthPillar, making nodes and home staking easier!
{% endhint %}

<details>

<summary>Additional step for new Node operators, new Validators</summary>

**Step 1: Configure your network, port forwarding and firewall.**&#x20;

* With EthPillar, puede modificarse en:
  * **Tools > UFW Firewall > Enable firewall with default settings**
  * El reenvío de puertos es [manually configured](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-2-configuring-node.md#configure-port-forwarding), depending on your router.
  * Confirme que el reenvío de puertos funciona con **Tools** > **Port Checker**
* Alternativamente, configure manualmente según la guía del manual. [Click here for detailed network configuration.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-2-configuring-node.md#network-configuration)

**Step 2: Configure your BIOS to auto power on after power loss**

Los pasos reales varían en función de la BIOS de su ordenador. Idea general: [https://www.wintips.org/setup-computer-to-auto-power-on-after-power-outage/](https://www.wintips.org/setup-computer-to-auto-power-on-after-power-outage/)

**Step 3: Enable Monitoring and Alerts (Optional)**

Encontrar bajo:

* **Tools** > **Monitoring**

**Step 4: Benchmark your node (Optional)**

Asegúrese de que su nodo tiene suficiente CPU/disk/network performance.

* **Tools** > **Yet-Another-Bench-Script**

</details>

<details>

<summary>Additional steps for new Validators</summary>

**Step 1: Setup Validator Keys**

* Familiarícese con la sección de la guía principal sobre [setting up your validator keys.](guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-5-installing-validator/setting-up-validator-keys.md)
* Cuando esté listo para generar sus claves, vaya a **EthPillar > Validator Client > Generate / Import Validator Keys**

**Step 2: Upload deposit\_data.json to Launchpad**

* Para empezar a apostar en Ethereum como validador, debe enviar al Launchpad su a  deposit\_data.json file, wque incluye los datos esenciales de la dirección de retirada, y abonar el depósito exigido de 32ETH por validador.

**Step 3: Congrats!**&#x20;

* Ahora estás esperando en la cola de entrada [https://www.validatorqueue.com](https://www.validatorqueue.com/)

<!---->

* Comprobar la [next steps from the main guide](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-i-installation/step-5-installing-validator/next-steps) para ampliar conocimientos. Especialmente las FAQ's «Wen staking rewards?"

</details>

## :joy: POAP

¿Te gusta EthPillar? [Support this public good by purchasing a limited edition POAP!](https://checkout.poap.xyz/169495)

<figure><img src="../../.gitbook/assets/3adf69e9-fb1b-4665-8645-60d71dd01a7b.png" alt=""><figcaption><p>Your EthPillar Enjoyoor's POAP</p></figcaption></figure>

**Purchase link:** [https://checkout.poap.xyz/169495](https://checkout.poap.xyz/169495)

ETH accepted on Mainnet, Arbitrum, Base, Optimism. :pray:

## :telephone: Get in touch

¿Tiene preguntas? Chatea con otros home stakers en [Discord](https://discord.gg/dEpAVWgFNB) o abrir PRs/issues en [Github](https://github.com/coincashew/ethpillar).&#x20;

Código fuente abierto disponible aquí: [https://github.com/coincashew/EthPillar](https://github.com/coincashew/EthPillar)

## :heart: Donations

Si desea apoyar este proyecto de bien público, encuéntrenos en la próxima Gitcoin Grants.

nuestra direccion de donacion es [0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0](https://etherscan.io/address/0xCF83d0c22dd54475cC0C52721B0ef07d9756E8C0) or coincashew.eth

## :ballot\_box\_with\_check: How to Update

{% tabs %}
{% tab title="TUI Update" %}
Upon opening EthPillar,

* Navigate to **System Administration > Update EthPillar** and then quit and relaunch.
{% endtab %}

{% tab title="Manual Update" %}
From a terminal, pull the latest updates from git.

```bash
cd ~/git/ethpillar
git pull
```
{% endtab %}
{% endtabs %}

## :tada: Credits

Shout out to [accidental-green](https://github.com/accidental-green/validator-install) for their pioneering work in Python validator tools, which has unintentionally ignited the inspiration and direction for this project. We are building upon their innovative foundations by forking their validator-install code. A heartfelt thanks to accidental-green for their game-changing contributions to the open-source Ethereum ecosystem!
