---
descripcion: >-
 Conviértete en validador y ayuda a proteger eth2, una cadena de bloques de prueba de participación. Cualquiera con 32 ETH puede unirse.
---

# Guía | Cómo configurar un validador en la red principal de ETH2

{% hint style="success" %}
A partir del 5 de diciembre de 2020, esta guía se actualizó para la **mainnet.** 😁
{% endhint %}

#### ✨ Para obtener la guía de la red de prueba, [haga clic aquí](guide-or-how-to-setup-a-validator-on-eth2-testnet.md).

![](../../.gitbook/assets/gg.jpg)

\*\*\*\*🎊 **Actualización 2020-12 : Estamos en [Gitcoin](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew), donde puedes contribuir a través de [quadratic funding](https://vitalik.ca/general/2019/12/07/quadratic.html) y generar un gran impacto. Tu contribución de 1 DAI equivale a una contribución equivalente de **23 DAI**
[Visitanos](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew). Gracias!🙏

{% embed url="https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew" %}

## 🏁 0. Requisitos previos

### 👩💻Habilidades para operar un nodo de baliza y validador eth2

Como validador de eth2, normalmente tendrás las siguientes capacidades:

* Conocimiento operativo de cómo configurar, ejecutar y mantener un nodo de baliza eth2 y un validador de forma continua.
* Un compromiso a largo plazo para mantener su validador 24/7/365
* Conocimientos básicos del sistema operativo
* He aprendido lo esencial viendo ['Intro to Eth2 & Staking for Beginners' by Superphiz](https://www.youtube.com/watch?v=tpkpW031RCI)
* Haber aprobado o estar inscrito activamente en el [Eth2 Study Master course](https://ethereumstudymaster.com/)
* y he leído las [8 Things Every Eth2 validator should know.](https://medium.com/chainsafe-systems/8-things-every-eth2-validator-should-know-before-staking-94df41701487)

### 🎗 **Requisitos mínimos de configuración**

* **Sistema operativo:** Linux de 64 bits (es decir, Ubuntu 20.04 LTS Server o Desktop)
* **Procesador:** CPU de doble núcleo, Intel Core i5-760 o AMD FX-8100 o superior
* **Memoria:** 8GB RAM
* **Almacenamiento:** 20GB SSD
* **Internet:** Conexión a Internet de banda ancha con velocidades de al menos 1 Mbps.
* **Energía:** Energía eléctrica confiable.
* **Saldo de ETH:** al menos 32 ETH y algo de ETH para tarifas de transacción de depósito
* **Monedero:** Metamask instalado

### 🏋♂ Configuración de hardware recomendada

* **Sistema operativo:** Linux de 64 bits (es decir, Ubuntu 20.04 LTS Server o Desktop)
* **Procesador:** CPU de cuatro núcleos, Intel Core i7–4770 o AMD FX-8310 o superior
* **Memoria:** 16 GB de RAM o más
* **Almacenamiento:** SSD de 1 TB o más
* **Internet:** Conexiones a internet de banda ancha con velocidades mínimas de 10 Mbps sin límite de datos.
* **Energía:** Energía eléctrica confiable con sistema de alimentación ininterrumpida (UPS)
* **Saldo de ETH:** al menos 32 ETH y algo de ETH para tarifas de transacción de depósito
* **Monedero:** Metamask instalado

{% hint style="success" %}
✨ **Consejo de validación profesional**: te recomiendo que comiences con una instancia completamente nueva de un sistema operativo, una máquina virtual o una máquina. Evita dolores de cabeza al NO reutilizar claves, billeteras o bases de datos de la red de prueba para tu validador. {% endhint %}

### 🔓 Prácticas recomendadas de seguridad para el validador eth2

Si necesita ideas o un recordatorio sobre cómo proteger su validador, consulte

{% page-ref page="guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md" %}

### 🛠 Configurar Ubuntu

Si necesita instalar Ubuntu Server, consulte

{% embed url="https://ubuntu.com/tutorials/install-ubuntu-server\#1-overview" %}

O escritorio Ubuntu,

{% page-ref page="../overview-xtz/guide-how-to-setup-a-baker/install-ubuntu.md" %}

### 🎭 Configurar Metamask

Si necesita instalar Metamask, consulte

{% page-ref page="../../wallets/browser-wallets/metamask-ethereum.md" %}

## 🌱 1. Comprar/cambiar o consolidar ETH

{% hint style="info" %}
Cada 32 ETH que poseas te permitirán crear un validador. Puedes ejecutar miles de validadores con tu nodo de baliza.
{% endhint %}

Su ETH (o múltiplos de 32 ETH) deben consolidarse en una única dirección accesible con Metamask.

Si necesita comprar/intercambiar o recargar su ETH a un múltiplo de 32, consulte:

{% page-ref page="guide-how-to-buy-eth.md" %}

## 👩💻 2. Regístrate para ser validador en Launchpad

1. Instale dependencias, la herramienta de depósito de la fundación Ethereum y genere sus dos conjuntos de pares de claves.

{% hint style="info" %}
Cada validador tendrá dos conjuntos de pares de claves: una clave de firma y una clave de retiro. Estas claves se derivan de una sola frase mnemotécnica. [Learn more about keys.](https://blog.ethereum.org/2020/05/21/keys/)
{% endhint %}

Tienes la opción de descargar la [ethereum foundation deposit tool](https://github.com/ethereum/eth2.0-deposit-cli) previamente creada o crearla desde la fuente.

{% tabs %}
{% tab title="Construir desde el código fuente" %} 
Instalar dependencias.

```text
sudo apt update
sudo apt install python3-pip git -y
```

Descargue el código fuente e instálelo.

```text
cd $HOME
git clone https://github.com/ethereum/eth2.0-deposit-cli.git eth2deposit-cli
cd eth2deposit-cli
sudo ./deposit.sh install
```

Crea un nuevo mnemónico.

```text
./deposit.sh new-mnemonic --chain mainnet
```
{% endtab %}

{% tab title="Pre-built eth2deposit-cli" %}
Descargar eth2deposit-cli.

```bash
cd $HOME
wget https://github.com/ethereum/eth2.0-deposit-cli/releases/download/v1.1.0/eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
```

Verifique que la suma de comprobación SHA256 coincida con la suma de comprobación en la [releases page](https://github.com/ethereum/eth2.0-deposit-cli/releases/tag/v1.0.0).

```bash
sha256sum eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
# SHA256 should be
# 2107f26f954545f423530e3501ae616c222b6bf77774a4f2743effb8fe4bcbe7
```

Extraer el archivo.

```text
tar -xvf eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
mv eth2deposit-cli-ed5a6d3-linux-amd64 eth2deposit-cli
rm eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
cd eth2deposit-cli
```

Crea un nuevo mnemónico.

```text
./deposit new-mnemonic --chain mainnet
```
{% endtab %}

{% tab title="Advanced - Most Secure" %}
{% hint style="warning" %}
🔥**\[ Optional \] Consejo de seguridad profesional**: **Ejecute la herramienta eth2deposit-cli y genere su semilla mnemotécnica para sus claves de validación en una máquina fuera de línea con espacio de aire iniciada desde USB.**
{% endhint %}

Aprenderá cómo iniciar una PC con Windows en un [Tails operating system](https://tails.boum.org/index.en.html).

Tails OS es un sistema operativo *amnésico* , lo que significa que no guardará nada ni *dejará* rastros cada vez que lo inicie.

### Part 0 - Requisitos previos

Necesitas:

- 2 medios de almacenamiento (pueden ser memorias USB, tarjetas SD o discos duros externos)
- Uno de ellos debe tener > 8 GB
- Computadora Windows o Mac
- 30 minutos o más dependiendo de la velocidad de descarga

### Part 1 - Descargar Tails OS

Descarga la imagen oficial del [Tails website](https://tails.boum.org/install/index.en.html). Puede que tarde un poco, ve a tomar un café.

Asegúrese de seguir la guía en el sitio web de Tails para verificar su descarga de Tails.

### Part 2 - Descargue e instale el software para transferir su imagen de Tails a su memoria USB

Para Windows:
- [Etcher](https://tails.boum.org/etcher/Etcher-Portable.exe)
- [Win32 Disk Imager](https://win32diskimager.org/#download)
- [Rufus](https://rufus.ie/en_US/)

Para Mac descargar  [Etcher](https://tails.boum.org/etcher/Etcher.dmg)

### Part 3 - Creando tu memoria USB de arranque

Ejecute el software anterior. Este es un ejemplo de cómo se ve en Mac OS con Etcher. Pero otros programas deberían ser similares.

![](../../.gitbook/assets/etcher_in_mac.png)

Seleccione la imagen del sistema operativo Tails que descargó como imagen. Luego, seleccione la memoria USB (la más grande).

A continuación, grabe la imagen en la memoria USB más grande.

### Part 4 - Descargar y verificar eth2-deposit-cli

Puede consultar la otra pestaña de esta guía sobre cómo descargar y verificar eth2-deposit-cli.

Copia el archivo a la otra memoria USB.

### Part 5 - Reinicie su computadora y acceda a Tails OS

Una vez que hayas hecho todo lo anterior, puedes reiniciar. Si estás conectado a Internet mediante un cable LAN, puedes desconectarlo manualmente.

Conecte la memoria USB que tiene su sistema operativo Tails.

En Mac, mantenga presionada la tecla Opción inmediatamente después de escuchar el sonido de inicio. Suelte la tecla después de que aparezca el Administrador de inicio.

En Windows, depende del fabricante de su computadora. Por lo general, se hace presionando F1 o F12. Si no funciona, intente buscar en Google "Ingresar al menú de opciones de arranque en [Ingrese la marca de su computadora]"

Seleccione la memoria USB que cargó con Tails OS para iniciar Tails.

### Part 6 - Bienvenido a Tails OS

![](../../.gitbook/assets/grub.png)

Puede arrancar con todas las configuraciones predeterminadas.

### Part 7 - Ejecutar eth2-deposit-cli

Conecte su otra memoria USB con el `eth2-deposit-cli` archivo.

A continuación, puede abrir la línea de comandos y navegar hasta el directorio que contiene el archivo. Luego, puede continuar con la guía desde la otra pestaña.

Crea un nuevo mnemónico.

```text
./deposit.sh new-mnemonic --chain mainnet
```

Si ejecutaste este comando directamente desde tu memoria USB que no es de Tails, las claves del validador deberían permanecer allí. Si no es así, copia el directorio a tu memoria USB que no es de Tails.

{% hint style="warning" %}
🔥**Asegúrate de haber guardado el directorio de claves de validación en tu otra memoria USB (que no sea Tails OS) antes de apagar Tails. Tails eliminará todo lo guardado en ella después de que lo apagues.**.
{% endhint %}

{% hint style="success" %}
🎉Felicitaciones por aprender a usar Tails OS para crear un sistema con espacio de aire. Como beneficio adicional, ¡puede reiniciar en Tails OS nuevamente y conectarse a Internet para navegar en la red oscura o limpiar la red de manera segura!
{% endhint %}

{% endtab %}

{% tab title="Advanced - Most Secure" %}
{% hint style="warning" %}
🔥**\[ Optional \] Consejo de seguridad profesional**:Ejecute la **herramienta eth2deposit-cli** y genere su **semilla mnemotécnica** para sus claves de validación en una **máquina fuera de línea con espacio de aire iniciada desde USB.**
{% endhint %}

Siga esta exclusiva [ethstaker.cc](https://ethstaker.cc/) para obtener información detallada sobre cómo crear un USB de arranque.

### Part 1 - Crear una unidad USB de arranque de Ubuntu 20.04

{% embed url="https://www.youtube.com/watch?v=DTR3PzRRtYU" %}

### Part 2 - Instalar Ubuntu 20.04 desde la unidad USB

{% embed url="https://www.youtube.com/watch?v=C97\_6MrufCE" %}

Puede copiar mediante una memoria USB los binarios preconstruidos de eth2deposit-cli desde una máquina en línea a una máquina fuera de línea con espacio de aire iniciada desde una memoria USB. Asegúrese de desconectar el cable Ethernet o el WIFI.
{% endtab %}
{% endtabs %}

2. Sigue las indicaciones y elige una contraseña. Anota tu mnemotécnica y guárdala en un lugar seguro y **fuera de línea**.

3. Sigue los pasos que se indican en [https://launchpad.ethereum.org/](https://launchpad.ethereum.org/) y omite los pasos que ya has completado. Estudia el material de descripción general de la fase 0 de eth2. ¡Entender eth2 es la clave del éxito!

4. De regreso al sitio web de Launchpad, cargue el archivo `deposit_data-#########.json` que encontró en el `validator_keys` directorio.

5. Conéctese al launchpad con su billetera Metamask, revise y acepte los términos.

6. Confirme la(s) transacción(es). Hay una transacción de depósito de 32 ETH para cada validador.

{% hint style="info" %}
Su transacción está enviando y depositando su ETH a la [official ETH2 deposit contract address. ](https://blog.ethereum.org/2020/11/04/eth2-quick-update-no-19/)

**Verifique**, _vuelva a verificar_, _**vuelva a verificar**_ que la dirección oficial del contrato de depósito de Eth2 sea correcta.[`0x00000000219ab540356cBB839Cbe05303d7705Fa`](https://etherscan.io/address/0x00000000219ab540356cbb839cbe05303d7705fa)
{% endhint %}

{% hint style="danger" %}
\*\*\*\*🔥 **Recordatorio crítico sobre criptomonedas: mantén tu mnemotecnia, mantén tu ETH.** 🚀

* Anota tu semilla mnemotécnica fuera de línea . _No por correo electrónico_. _No en la nube_.
* Es mejor tener varias copias. _Se recomienda almacenarlas en una_ [_metal seed._](https://jlopp.github.io/metal-bitcoin-storage-reviews/)
* Las claves de retiro se generarán a partir de este mnemónico en el futuro.
* Realice **copias de seguridad sin conexión**, por ejemplo en una memoria USB, de su **`validator_keys`** directorio.
{% endhint %}

## 🛸 3. Instalar un nodo ETH1

{% hint style="info" %}
Ethereum 2.0 requiere una conexión a Ethereum 1.0 para poder monitorear los depósitos del validador de 32 ETH. Alojar su propio nodo Ethereum 1.0 es la mejor manera de maximizar la descentralización y minimizar la dependencia de terceros como Infura. 
{% endhint %}

{% hint style="warning" %}
Los pasos siguientes suponen que ha completado la  [best practices security guide. ](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md)

🛑 No ejecutes tus procesos como usuario **ROOT** . 😱
{% endhint %}

Tu elección entre [**OpenEthereum**](https://www.parity.io/ethereum/)**,** [**Geth**](https://geth.ethereum.org/)**,** [**Besu**](https://besu.hyperledger.org/)**,** [**Nethermind**](https://www.nethermind.io/), [**Infura**](https://infura.io/) **or** [**Chainstack**](https://chainstack.com/)**.**

{% tabs %}
{% tab title="OpenEthereum \(Parity\)" %}
{% hint style="info" %}
**OpenEthereum** : su objetivo es ser el cliente de Ethereum más rápido, liviano y seguro que utilice el lenguaje de **programación Rust** . OpenEthereum tiene licencia GPLv3 y se puede utilizar para todas sus necesidades de Ethereum.
{% endhint %}

#### ⚙ Instalar dependencias

```text
sudo apt-get update
sudo apt-get install curl jq unzip -y
```

#### 🤖 Instalar OpenEthereum

Revise la última versión en [https://github.com/openethereum/openethereum/releases](https://github.com/openethereum/openethereum/releases)

Descargue automáticamente la última versión de Linux, descomprima, agregue permisos de ejecución y limpie.

```bash
mkdir $HOME/openethereum
cd $HOME/openethereum
curl -s https://api.github.com/repos/openethereum/openethereum/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux | xargs wget -q --show-progress
unzip -o openethereum*.zip
chmod +x openethereum
rm openethereum*.zip
```

​ ⚙ **Configurar y configurar systemd**

Ejecute lo siguiente para crear un **archivo de unidad** para definir su `eth1.service` configuración.

Simplemente copie y pegue lo siguiente.

```bash
cat > $HOME/eth1.service << EOF
[Unit]
Description     = openethereum eth1 service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/openethereum/openethereum
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Configuración específica de Nimbus**: agregue el siguiente indicador a la línea **ExecStart**.

```bash
--ws-origins=all
```
{% endhint %}

Mueva el archivo de la unidad a `/etc/systemd/system` y dale permisos.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Iniciar OpenEthereum

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Geth" %}
{% hint style="info" %}
**Geth** - Go Ethereum es una de las tres implementaciones originales (junto con C++ y Python) del protocolo Ethereum. Está escrito en Go , es de código abierto y tiene licencia GNU LGPL v3. 
{% endhint %}

Revise las últimas notas de la versión en [https://github.com/ethereum/go-ethereum/releases](https://github.com/ethereum/go-ethereum/releases)

#### 🧬 Instalar desde el repositorio

```text
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt-get install ethereum -y
```

⚙ **Instalar y configurar systemd**

Ejecute lo siguiente para crear un archivo de unidad para definir su `eth1.service` configuración.

Simplemente copie y pegue lo siguiente.

```bash
cat > $HOME/eth1.service << EOF
[Unit]
Description     = geth eth1 service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/geth --http --metrics --pprof
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
**Configuración específica de Nimbus**: agregue el siguiente indicador a la línea **ExecStart**.
```bash
--ws
```
{% endhint %}

Mueva el archivo de la unidad a `/etc/systemd/system` y dale permisos.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Iniciar geth

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Besu" %}
{% hint style="info" %}
**Hyperledger Besu** es un cliente de Ethereum de código abierto diseñado para aplicaciones empresariales exigentes que requieren un procesamiento de transacciones seguro y de alto rendimiento en una red privada. Está desarrollado bajo la licencia Apache 2.0 y escrito en **Java**. 
{% endhint %}

#### 🧬 Instalar dependencia de Java

```text
sudo apt update
sudo apt install openjdk-18-jdk -y
```

#### 🌜 Descargar y descomprimir Besu

Revise la última versión en [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

El archivo se puede descargar desde [https://dl.bintray.com/hyperledger-org/besu-repo](https://dl.bintray.com/hyperledger-org/besu-repo)

```text
cd
wget -O besu.tar.gz https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz
tar -xvf besu.tar.gz
rm besu.tar.gz
mv besu* besu
```

⚙ **Instalar y configurar systemd**

Ejecute lo siguiente para crear un archivo de unidad para definir su `eth1.service` configuración.

Simplemente copie y pegue lo siguiente.

```bash
cat > $HOME/eth1.service << EOF
[Unit]
Description     = openethereum eth1 service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/besu/bin/besu --metrics-enabled --rpc-http-enabled --data-path="$HOME/.besu"
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Mueva el archivo de la unidad a `/etc/systemd/system` y dale permisos.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Iniciar besu

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Nethermind" %}
{% hint style="info" %}
**Nethermind** es un cliente insignia de Ethereum que se centra en el rendimiento y la flexibilidad. Desarrollado sobre .**NET** Core, una plataforma generalizada y apta para empresas, Nethermind simplifica la integración con infraestructuras existentes, sin perder de vista la estabilidad, la confiabilidad, la integridad de los datos y la seguridad.
{% endhint %}

#### ⚙ Instalar dependencias

```text
sudo apt-get update
sudo apt-get install curl libsnappy-dev libc6-dev jq libc6 unzip -y
```

#### 🌜 Descargar y descomprimir Nethermind

Revise la última versión en [https://github.com/NethermindEth/nethermind/releases](https://github.com/NethermindEth/nethermind/releases)

Descargue automáticamente la última versión de Linux, descomprima y limpie.

```bash
mkdir $HOME/nethermind
cd $HOME/nethermind
curl -s https://api.github.com/repos/NethermindEth/nethermind/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
unzip -o nethermind*.zip
rm nethermind*linux*.zip
```

⚙ **Instalar y configurar systemd**

Ejecute lo siguiente para crear un archivo de unidad para definir su `eth1.service` configuración.

Simplemente copie y pegue lo siguiente.

```bash
cat > $HOME/eth1.service << EOF
[Unit]
Description     = openethereum eth1 service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/nethermind/Nethermind.Runner --baseDbPath $HOME/.nethermind --Metrics.Enabled true --JsonRpc.Enabled true --Sync.DownloadBodiesInFastSync true --Sync.DownloadReceiptsInFastSync true --Sync.AncientBodiesBarrier 11052984 --Sync.AncientReceiptsBarrier 11052984
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Mueva el archivo de la unidad a `/etc/systemd/system` y dale permisos.

```bash
sudo mv $HOME/eth1.service /etc/systemd/system/eth1.service
```

```bash
sudo chmod 644 /etc/systemd/system/eth1.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```text
sudo systemctl daemon-reload
sudo systemctl enable eth1
```

#### ⛓ Iniciar Nethermind

```text
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Configuración mínima de hardware \(Infura\)" %}
{% hint style="info" %}
Infura es adecuado para configuraciones con espacio de disco limitado. Siempre que sea posible, ejecute su propio nodo eth1 completo.
{% endhint %}

Regístrese para obtener una clave de acceso API en [https://infura.io/](https://infura.io/)

1. Regístrese para obtener una cuenta gratuita.
2. Confirme su dirección de correo electrónico.
3. Visita tu panel de control [https://infura.io/dashboard](https://infura.io/dashboard)
4. Crea un proyecto y dale un nombre.
5. Seleccione **Mainnet** como ENDPOINT
6. Siga la configuración específica para su cliente eth2 que se encuentra a continuación.

{% hint style="success" %}
Alternativamente, utilice un nodo Ethereum gratuito en [https://ethereumnodes.com/](https://ethereumnodes.com/)
{% endhint %}

## Configuración específica de Nimbus

1. Al crear el archivo de unidad de su systemd , actualice el `--web-url` parámetro con este endpoint.
2. Copiar el punto final del websocket. Comienza con `wss://`
3. Guarde esto para el paso 4, configurando su nodo eth2.

```bash
#example
--web3-url=<your wss:// infura endpoint>
```

## Configuración específica de Teku

1. Después de crear el `teku.yaml` ubicado en `/etc/teku/teku.yaml`, actualice el  `--eth1-endpoint` parámetro con este endpoint.
2. Copiar el punto final http. Comienza con `http://`
3. Guarde esto para el paso 4, configurando su nodo eth2.

```bash
#example
eth1-endpoint: <your https:// infura endpoint>
```

## Configuración específica del faro

1. Al crear el archivo de unidad systemd de su cadena de balizas , agregue el parámetro con este punto final. `--eth1-endpoint`
2. Copia el punto final https . Comienza con `https://`
3. Guarde esto para el paso 4, configurando su nodo eth2.

```bash
#example
--eth1-endpoint=<your https:// infura endpoint>
```

## Configuración específica de Prysm

1. Al crear el archivo de unidad systemd de su cadena de balizas , actualice el  `--http-web3provider` parámetro con este punto final.
2. Copia el punto final https . Comienza con `https://`
3. Guarde esto para el paso 4, configurando su nodo eth2.

```bash
#example
--http-web3provider=<your https:// infura endpoint>
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
La sincronización de un nodo eth1 puede demorar hasta una semana. En máquinas de alta gama con Internet Gigabit, la sincronización demorará menos de un día.
{% endhint %}

{% hint style="success" %}
Su nodo eth1 está completamente sincronizado cuando ocurren estos eventos.

* **`OpenEthereum:`** `Imported #<block number>`
* **`Geth:`** `Imported new chain segment`
* **`Besu:`** `Imported #<block number>`
* **`Nethermind:`** `No longer syncing Old Headers`
{% endhint %}

#### 🛠 Comandos útiles de eth1.service

​​ 🗒 **Para ver y seguir los registros de eth1**

```text
journalctl -u eth1 -f
```

🗒 **Para detener el servicio eth1**

```text
sudo systemctl stop eth1
```

## 🌜 4. Configurar un nodo y un validador de la cadena de balizas ETH2

Tu elección entre Lighthouse, Nimbus, Teku, Prysm o Lodestar.

{% tabs %}
{% tab title="Lighthouse" %}
{% hint style="info" %}
[Lighthouse](https://github.com/sigp/lighthouse) es un cliente de Eth2.0 con un gran enfoque en la velocidad y la seguridad. El equipo detrás de él, [Sigma Prime](https://sigmaprime.io/), es una empresa de seguridad de la información e ingeniería de software que ha financiado Lighthouse junto con la Fundación Ethereum, Consensys y particulares. Lighthouse está construido en Rust y se ofrece bajo una licencia Apache 2.0.
{% endhint %}

## ⚙ 4.1. Instalar dependencia de rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Introduzca '1' para continuar con la instalación predeterminada.

Actualice sus variables de entorno.

```bash
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Instalar dependencias de rust.

```text
sudo apt-get update
sudo apt install -y git gcc g++ make cmake pkg-config libssl-dev
```

## 💡 4.2. Construye Lighthouse desde la fuente

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/sigp/lighthouse.git
cd lighthouse
git fetch --all && git checkout stable && git pull
make
```

{% hint style="info" %}
En caso de errores de compilación, ejecute la siguiente secuencia.

```text
rustup update
cargo clean
make
```
{% endhint %}

{% hint style="info" %}
Este proceso de compilación puede tardar unos minutos.
{% endhint %}

Verifique que Lighthouse se haya instalado correctamente verificando el número de versión.

```text
lighthouse --version
```

## 🎩 4.3. Importar clave de validación

{% hint style="info" %}
Cuando importa sus claves a Lighthouse, sus claves de firma del validador se almacenan en la `$HOME/.lighthouse/mainnet/validators` carpeta.
{% endhint %}

Ejecute el siguiente comando para importar sus claves de validación desde el directorio de herramientas eth2deposit-cli.

Ingrese la contraseña de su almacén de claves para importar cuentas.

```bash
lighthouse account validator import --network mainnet --directory=$HOME/eth2deposit-cli/validator_keys
```

Verifique que las cuentas se importaron correctamente.

```bash
lighthouse account_manager validator list --network mainnet
```

{% hint style="danger" %}
**ADVERTENCIA** : NO UTILICE LOS ALMACENES DE CLAVE ORIGINALES PARA VALIDAR CON OTRO CLIENTE, O SERÁ CORTADO.
{% endhint %}

## 🔥 4.4. Configurar el reenvío de puertos y/o el firewall

Dependiendo de su configuración de red o de la configuración de su proveedor de nube, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **La cadena de balizas Lighthouse** requiere el puerto 9000 para TCP y UDP
* **El nodo eth1** requiere el puerto 30303 para tcp y udp

{% hint style="info" %}
✨ **Consejo de reenvío de puertos: deberá reenviar y abrir puertos a su validador. Verifique que funcione con [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) o [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## ⛓ 4.5. Iniciar la cadena de balizas

#### 🍰 Beneficios de usar systemd para tu cadena de balizas <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Inicie automáticamente su cadena de balizas cuando la computadora se reinicie debido a mantenimiento, corte de energía, etc.
2. Reiniciar automáticamente los procesos de la cadena de balizas bloqueados.
3. Maximice el tiempo de funcionamiento y el rendimiento de su cadena de balizas.

#### 🛠  Instrucciones de configuración para Systemd

Ejecute lo siguiente para crear un archivo de unidad que defina su `beacon-chain.service` configuración. Simplemente copie y pegue.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(which lighthouse) bn --staking --metrics --network mainnet
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="info" %}
\*\*\*\*🔥 **Consejo profesional de Lighthouse**: en la línea **ExecStart** `--eth1-endpoints` agregar el indicador permite nodos eth1 redundantes. Separe con una coma. Asegúrese de que el punto final no termine con una barra diagonal final o `/` elimínelo.

```bash
# Example:
--eth1-endpoints http://localhost:8545,https://nodes.mewapi.io/rpc/eth,https://mainnet.eth.cloud.ava.do,https://mainnet.infura.io/v3/xxx
```
{% endhint %}

Mueva el archivo de unidad a `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Actualizar permisos de archivos.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque y luego inicie su servicio de nodo de baliza.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="info" %}
**Solución de problemas comunes**:

_¿La cadena de balizas no pudo conectarse al servicio :8545?_

* En el archivo de unidad de cadena de balizas en [Servicio], agregue " `ExecStartPre = /bin/sleep 30`" para que espere 30 segundos hasta que el nodo eth1 se inicie antes de conectarse.

_CRIT Id. de cadena eth1 no válida. Cambie a la Id. de cadena correcta._

* Permita que su nodo eth1 se sincronice completamente con la red principal.
{% endhint %}

{% hint style="success" %}
Buen trabajo. Tu cadena de balizas ahora está administrada por la confiabilidad y solidez de systemd. A continuación, se muestran algunos comandos para usar systemd.
{% endhint %}

### 🛠 Algunos comandos útiles de systemd

#### ✅ Comprueba si la cadena de balizas está activa

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 Ver el estado de la cadena de balizas

```text
sudo systemctl status beacon-chain
```

#### 🔄 Reinicio de la cadena de balizas

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Detener la cadena de balizas

```text
sudo systemctl stop beacon-chain
```

#### 🗄 Visualización y filtrado de registros

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

## 🧬 4.6. Iniciar el validador

#### 🚀 Configurar Graffiti y POAP

Configura tu `graffiti`, un mensaje personalizado incluido en los bloques que tu validador propone con éxito, y gana un token POAP. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Ejecute el siguiente comando para configurar la `MY_GRAFFITI` variable. Reemplace  `<my POAP string or message>` entre comillas simples.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Obtenga más información sobre  [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

#### 🍰 Beneficios de usar systemd para tu validador <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1.Inicie automáticamente su validador cuando la computadora se reinicie debido a mantenimiento, corte de energía, etc.
2. Reiniciar automáticamente los procesos de validación bloqueados.
3. Maximice el tiempo de funcionamiento y el rendimiento de su validador.

#### 🛠 Instrucciones de configuración para Systemd

Ejecute lo siguiente para crear un **archivo de unidad** que defina su`validator.service` configuración. Simplemente copie y pegue.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(which lighthouse) vc --network mainnet --graffiti "${MY_GRAFFITI}" --metrics
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Mueva el archivo de unidad a `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Actualizar permisos de archivos.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque y luego inicie su validador.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
Buen trabajo. Ahora tu validador está gestionado por la fiabilidad y robustez de systemd. A continuación se muestran algunos comandos para utilizar systemd.
{% endhint %}

### 🛠 Algunos comandos útiles de systemd

#### ✅ Comprueba si el validador está activo

```text
sudo systemctl is-active validator
```

#### 🔎 Ver el estado del validador

```text
sudo systemctl status validator
```

#### 🔄Reiniciando el validador

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Detener el validador

```text
sudo systemctl stop validator
```

#### 🗄 Visualización y filtrado de registros

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journalctl --unit=validator --since=yesterday
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```
{% endtab %}

{% tab title="Nimbus" %}
{% hint style="info" %}
[Nimbus](https://our.status.im/tag/nimbus/) es un proyecto de investigación y una implementación de cliente para Ethereum 2.0 diseñado para funcionar bien en sistemas integrados y dispositivos móviles personales, incluidos los teléfonos inteligentes más antiguos con hardware con recursos limitados. El equipo de Nimbus es de [Status](https://status.im/about/) la empresa más conocida por [their messaging app/wallet/Web3 browser](https://status.im/) con el mismo nombre. Nimbus (Apache 2) está escrito en Nim, un lenguaje con sintaxis similar a Python que se compila en C. 
{% endhint %}

## ⚙ 4.1. Construir Nimbus desde el código fuente

Instalar dependencias.

```text
sudo apt-get update
sudo apt-get install curl build-essential git -y
```

Instalar y construir Nimbus.

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/status-im/nimbus-eth2
cd nimbus-eth2
make NIMFLAGS="-d:insecure" nimbus_beacon_node
```

{% hint style="info" %}
El proceso de compilación puede tardar unos minutos.
{% endhint %}

Verifique que Nimbus se haya instalado correctamente mostrando la ayuda.

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --help
```

Copiar el archivo binario a `/usr/bin`

```bash
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
```

## 🎩 4.2. Importar clave de validación <a id="6-import-validator-key"></a>

Cree una estructura de directorio para almacenar datos de Nimbus.

```bash
sudo mkdir -p /var/lib/nimbus
```

Toma posesión de este directorio y establece el nivel de permiso correcto.

```bash
sudo chown $(whoami):$(whoami) /var/lib/nimbus
sudo chmod 700 /var/lib/nimbus
```

El siguiente comando importará sus claves de validación.

Ingrese la contraseña de su almacén de claves para importar cuentas.

```bash
cd $HOME/git/nimbus-eth2
build/nimbus_beacon_node deposits import --data-dir=/var/lib/nimbus $HOME/eth2deposit-cli/validator_keys
```

Ahora puedes verificar que las cuentas se importaron correctamente haciendo un listado de directorio.

```bash
ll /var/lib/nimbus/validators
```

Deberías ver una carpeta con el nombre de cada una de las claves públicas de tu validador.

{% hint style="info" %}
Cuando importa sus claves a Nimbus, sus claves de firma del validador se almacenan en la`/var/lib/nimbus` carpeta, bajo `secrets` y `validators.`

La `secrets` carpeta contiene el secreto común que le da acceso a todas sus claves de validación.

La `validators` carpeta contiene sus almacenes de claves de firma (claves cifradas). Los validadores utilizan los almacenes de claves como método para intercambiar claves.

Para obtener más información sobre claves y almacenes de claves, consulte [here](https://blog.ethereum.org/2020/05/21/keys/).
{% endhint %}

{% hint style="danger" %}
**ADVERTENCIA**: NO UTILICE LOS ALMACENES DE CLAVE ORIGINALES PARA VALIDAR CON OTRO CLIENTE, O SERÁ CORTADO.
{% endhint %}

## 🔥 4.3. Configurar el reenvío de puertos y/o el firewall

Dependiendo de su configuración de red o de la configuración de su proveedor de nube, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **El nodo de la cadena de balizas Nimbus** utilizará el puerto 9000 para TCP y UDP
* **El nodo eth1** requiere el puerto 30303 para tcp y udp

{% hint style="info" %}
✨ **Consejo de reenvío de puertos**: deberá reenviar y abrir puertos a su validador. Verifique que funcione con [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) o [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

##  🏂 4.4. Iniciar la cadena de balizas y el validador

{% hint style="info" %}
Nimbus combina la cadena de balizas y el validador en un solo proceso.
{% endhint %}

#### 🚀 Configurar Graffiti y POAP

Configura tu  `graffiti`, un mensaje personalizado incluido en los bloques que tu validador propone con éxito, y gana un token POAP. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Ejecute el siguiente comando para configurar la `MY_GRAFFITI` variable. Reemplace `<my POAP string or message>` entre comillas simples.
```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Obtenga más información sobre [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

#### 🍰 Beneficios de usar systemd para su cadena de balizas y validador <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Inicie automáticamente su cadena de balizas cuando la computadora se reinicie debido a mantenimiento, corte de energía, etc.
2. Reiniciar automáticamente los procesos de la cadena de balizas bloqueados.
3. Maximice el tiempo de funcionamiento y el rendimiento de su cadena de balizas.

#### 🛠 Instrucciones de configuración

Ejecute lo siguiente para crear un archivo de unidad que defina su `beacon-chain.service` configuración. Simplemente copie y pegue.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
Type            = simple
User            = $(whoami)
WorkingDirectory= /var/lib/nimbus
ExecStart       = /bin/bash -c '/usr/bin/nimbus_beacon_node --network=mainnet --graffiti="${MY_GRAFFITI}" --data-dir=/var/lib/nimbus --web3-url=ws://127.0.0.1:8546 --metrics --metrics-port=8008 --rpc --rpc-port=9091 --validators-dir=/var/lib/nimbus/validators --secrets-dir=/var/lib/nimbus/secrets --log-file=/var/lib/nimbus/beacon.log'
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="warning" %}
Nimbus only supports websocket connections \("ws://" and "wss://"\) for the ETH1 node. Geth, OpenEthereum, Infura and Chainstack ETH1 nodes are verified compatible.
{% endhint %}

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```
{% endtab %}

{% tab title="Teku" %}
{% hint style="info" %}
[PegaSys Teku](https://pegasys.tech/teku/) \(formerly known as Artemis\) is a Java-based Ethereum 2.0 client designed & built to meet institutional needs and security requirements. PegaSys is an arm of [ConsenSys](https://consensys.net/) dedicated to building enterprise-ready clients and tools for interacting with the core Ethereum platform. Teku is Apache 2 licensed and written in Java, a language notable for its materity & ubiquity.
{% endhint %}

## ⚙ 4.1 Build Teku from source

Install git.

```text
sudo apt-get install git -y
```

Install Java 18.

For **Ubuntu 20.x**, use the following

```
sudo apt update
sudo apt install openjdk-18-jdk -y
```

For **Ubuntu 18.x**, use the following

```text
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-set-default -y
```

Verify Java 18+ is installed.

```bash
java --version
```

Install and build Teku.

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/ConsenSys/teku.git
cd teku
./gradlew distTar installDist
```

{% hint style="info" %}
This build process may take a few minutes.
{% endhint %}

Verify Teku was installed properly by displaying the version.

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

Copy the teku binary file to `/usr/bin/teku`

```bash
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Teku beacon chain node** will use port 9000 for tcp and udp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
\*\*\*\*✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🏂 4.3. Configure the beacon chain and validator

{% hint style="info" %}
Teku combines both the beacon chain and validator into one process.
{% endhint %}

Setup a directory structure for Teku.

```bash
sudo mkdir -p /var/lib/teku
sudo mkdir -p /etc/teku
sudo chown $(whoami):$(whoami) /var/lib/teku
```

 Copy your `validator_files` directory to the data directory we created above and remove the extra deposit\_data file.

```bash
cp -r $HOME/eth2deposit-cli/validator_keys /var/lib/teku
rm /var/lib/teku/validator_keys/deposit_data*
```

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

Store your validator's password in a file.

Update your password between the quotation marks after `echo`.

```bash
echo 'my_password_goes_here' > $HOME/validators-password.txt
sudo mv $HOME/validators-password.txt /etc/teku/validators-password.txt
```

#### 🚀 Setup Graffiti and POAP

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

Generate your Teku Config file. Simply copy and paste.

```bash
cat > $HOME/teku.yaml << EOF
# network
network: "mainnet"

# p2p
p2p-enabled: true
p2p-port: 9000
# validators
validator-keys: "/var/lib/teku/validator_keys:/var/lib/teku/validator_keys"
validators-graffiti: "${MY_GRAFFITI}"

# Eth 1
eth1-endpoint: "http://localhost:8545"

# metrics
metrics-enabled: true
metrics-port: 8008

# database
data-path: "$(echo $HOME)/tekudata"
data-storage-mode: "archive"

# rest api
rest-api-port: 5051
rest-api-docs-enabled: true
rest-api-enabled: true

# logging
log-include-validator-duties-enabled: true
log-destination: CONSOLE
EOF
```

Move the config file to `/etc/teku`

```bash
sudo mv $HOME/teku.yaml /etc/teku/teku.yaml
```

## 🎩 4.4 Import validator key

{% hint style="info" %}
When specifying directories for your validator-keys, Teku expects to find identically named keystore and password files.

For example `keystore-m_12221_3600_1_0_0-11222333.json` and `keystore-m_12221_3600_1_0_0-11222333.txt`
{% endhint %}

Create a corresponding password file for every one of your validators.

```bash
for f in /var/lib/teku/validator_keys/keystore*.json; do cp /etc/teku/validators-password.txt /var/lib/teku/validator_keys/$(basename $f .json).txt; done
```

Verify that your validator's keystore and validator's passwords are present by checking the following directory.

```bash
ll /var/lib/teku/validator_keys
```

## 🏁 4.5. Start the beacon chain and validator

Use **systemd** to manage starting and stopping teku.

#### 🍰 Benefits of using systemd for your beacon chain and validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = /usr/bin/teku/bin/teku -c /etc/teku/teku.yaml
Restart         = on-failure
Environment     = JAVA_OPTS=-Xmx5g

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```
{% endtab %}

{% tab title="Prysm" %}
{% hint style="info" %}
[Prysm](https://github.com/prysmaticlabs/prysm) is a Go implementation of Ethereum 2.0 protocol with a focus on usability, security, and reliability. Prysm is developed by [Prysmatic Labs](https://prysmaticlabs.com/), a company with the sole focus on the development of their client. Prysm is written in Go and released under a GPL-3.0 license.
{% endhint %}

## ⚙ 4.1. Install Prysm

```bash
mkdir ~/prysm && cd ~/prysm
curl https://raw.githubusercontent.com/prysmaticlabs/prysm/master/prysm.sh --output prysm.sh && chmod +x prysm.sh
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Prysm beacon chain node** will use port 12000 for udp and port 13000 for tcp
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🎩 4.3. Import validator key

Accept terms of use, accept default wallet location, enter a new password to encrypt your wallet and enter the password for your imported accounts.

```bash
$HOME/prysm/prysm.sh validator accounts import --mainnet --keys-dir=$HOME/eth2deposit-cli/validator_keys
```

Verify your validators imported successfully.

```bash
$HOME/prysm/prysm.sh validator accounts list --mainnet
```

Confirm your validator's pubkeys are listed.

> \#Example output:
>
> Showing 1 validator account View the eth1 deposit transaction data for your accounts by running \`validator accounts list --show-deposit-data
>
> Account 0 \| pens-brother-heat
> \[validating public key\] 0x2374.....7121

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🏂 4.4. Start the beacon chain

#### 🍰 Benefits of using systemd for your beacon chain and validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration. Simply copy and paste.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
Type            = simple
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh beacon-chain --mainnet --p2p-max-peers=75 --http-web3provider=http://127.0.0.1:8545 --accept-terms-of-use
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

####  🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

####  🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

#### 🗒 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

## 🧬 4.5. Start the validator <a id="9-start-the-validator"></a>

Store your validator's password in a file and make it read-only.

```bash
echo 'my_password_goes_here' > $HOME/.eth2validators/validators-password.txt
sudo chmod 600 $HOME/.eth2validators/validators-password.txt
```

#### 🚀 Setup Graffiti and POAP

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

Your choice of running a validator manually from command line or automatically with systemd.

#### 🍰 Benefits of using systemd for your validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your validator when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed validator processes.
3. Maximize your validator up-time and performance.

#### 🛠 Setup Instructions for systemd

Run the following to create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh validator --mainnet --graffiti "${MY_GRAFFITI}" --accept-terms-of-use --wallet-password-file $(echo $HOME)/.eth2validators/validators-password.txt
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Run the following to enable auto-start at boot time and then start your validator.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

### 🛠 Some helpful systemd commands

#### ✅ Check whether the validator is active

```text
sudo systemctl is-active validator
```

#### 🔎 View the status of the validator

```text
sudo systemctl status validator
```

#### 🔄 Restarting the validator

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Stopping the validator

```text
sudo systemctl stop validator
```

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journalctl --unit=validator --since=yesterday
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

Verify that your **validator public key** appears in the logs.

```bash
journalctl --unit=validator --since=today
# Example below
# INFO Enabled validator       voting_pubkey: 0x2374.....7121
```
{% endtab %}

{% tab title="Lodestar" %}
{% hint style="info" %}
**Lodestar is a Typescript implementation** of the official [Ethereum 2.0 specification](https://github.com/ethereum/eth2.0-specs) by the [ChainSafe.io](https://lodestar.chainsafe.io/) team. In addition to the beacon chain client, the team is also working on 22 packages and libraries. A complete list can be found [here](https://hackmd.io/CcsWTnvRS_eiLUajr3gi9g). Finally, the Lodestar team is leading the Eth2 space in light client research and development and has received funding from the EF and Moloch DAO for this purpose.
{% endhint %}

{% hint style="danger" %}
Lodestar may not be fully functional and stable yet.
{% endhint %}

## ⚙ 4.1 Build Lodestar from source

Install curl and git.

```bash
sudo apt-get install git curl -y
```

Install yarn.

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn
```

Confirm yarn is installed properly.

```bash
yarn --version
# Should output version >= 1.22.4
```

Install nodejs.

```text
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Confirm nodejs is installed properly.

```bash
nodejs -v
# Should output version >= v12.18.3
```

Install and build Lodestar.

```bash
mkdir ~/git
cd ~/git
git clone https://github.com/chainsafe/lodestar.git
cd lodestar
yarn install
yarn run build
```

{% hint style="info" %}
This build process may take a few minutes.
{% endhint %}

Verify Lodestar was installed properly by displaying the help menu.

```text
yarn run cli --help
```

## 🔥 4.2. Configure port forwarding and/or firewall

Specific to your networking setup or cloud provider settings, [ensure your validator's firewall ports are open and reachable.](guide-or-security-best-practices-for-a-eth2-validator-beaconchain-node.md#configure-your-firewall)

* **Lodestar beacon chain node** will use port 30607 for tcp and port 9000 for udp peer discovery.
* **eth1** node requires port 30303 for tcp and udp

{% hint style="info" %}
\*\*\*\*✨ **Port Forwarding Tip:** You'll need to forward and open ports to your validator. Verify it's working with [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/) or [https://canyouseeme.org/](https://canyouseeme.org/) .
{% endhint %}

## 🎩 4.3. Import validator key

```bash
yarn run cli account validator import \
  --network mainnet \
  --directory $HOME/eth2deposit-cli/validator_keys
```

Enter your keystore's password to import accounts.

Confirm your keys were imported properly.

```text
yarn run cli account validator list --network mainnet
```

{% hint style="danger" %}
**WARNING**: DO NOT USE THE ORIGINAL KEYSTORES TO VALIDATE WITH ANOTHER CLIENT, OR YOU WILL GET SLASHED.
{% endhint %}

## 🏂 4.4. Start the beacon chain and validator

Run the beacon chain automatically with systemd.

#### 🍰 Benefits of using systemd for your beacon chain <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your beacon chain when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed beacon chain processes.
3. Maximize your beacon chain up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`beacon-chain.service` configuration. Simply copy and paste.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
User            = $(whoami)
WorkingDirectory= $(echo $HOME)/git/lodestar
ExecStart       = yarn run cli beacon --network mainnet --eth1.providerUrl http://localhost:8545 --metrics.serverPort 8008
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```

Run the following to enable auto-start at boot time and then start your beacon node service.

```text
sudo systemctl daemon-reload
sudo systemctl enable beacon-chain
sudo systemctl start beacon-chain
```

{% hint style="success" %}
Nice work. Your beacon chain is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### ✅ Check whether the beacon chain is active

```text
sudo systemctl is-active beacon-chain
```

#### 🔎 View the status of the beacon chain

```text
sudo systemctl status beacon-chain
```

#### 🔄 Restarting the beacon chain

```text
sudo systemctl reload-or-restart beacon-chain
```

#### 🛑 Stopping the beacon chain

```text
sudo systemctl stop beacon-chain
```

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=beacon-chain -f
#view log since yesterday
journalctl --unit=beacon-chain --since=yesterday
#view log since today
journalctl --unit=beacon-chain --since=today
#view log between a date
journalctl --unit=beacon-chain --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```

## 🧬 4.5. Start the validator

#### 🚀 Setup Graffiti and POAP

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn a POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>` between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

Run the validator automatically with systemd.

#### 🍰 Benefits of using systemd for your validator <a id="benefits-of-using-systemd-for-your-stake-pool"></a>

1. Auto-start your validator when the computer reboots due to maintenance, power outage, etc.
2. Automatically restart crashed validator processes.
3. Maximize your validator up-time and performance.

#### 🛠 Setup Instructions

Run the following to create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
WorkingDirectory= $(echo $HOME)/git/lodestar
ExecStart       = yarn run cli validator run --network mainnet --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```

Run the following to enable auto-start at boot time and then start your validator.

```text
sudo systemctl daemon-reload
sudo systemctl enable validator
sudo systemctl start validator
```

{% hint style="success" %}
Nice work. Your validator is now managed by the reliability and robustness of systemd. Below are some commands for using systemd.
{% endhint %}

### 🛠 Some helpful systemd commands

#### ✅ Check whether the validator is active

```text
sudo systemctl is-active validator
```

#### 🔎 View the status of the validator

```text
sudo systemctl status validator
```

#### 🔄 Restarting the validator

```text
sudo systemctl reload-or-restart validator
```

#### 🛑 Stopping the validator

```text
sudo systemctl stop validator
```

#### 🗄 Viewing and filtering logs

```bash
#view and follow the log
journalctl --unit=validator -f
#view log since yesterday
journalctl --unit=validator --since=yesterday
#view log since today
journalctl --unit=validator --since=today
#view log between a date
journalctl --unit=validator --since='2020-12-01 00:00:00' --until='2020-12-02 12:00:00'
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Validator client** - Responsible for producing new blocks and attestations in the beacon chain and shard chains.

**Beacon chain client** - Responsible for managing the state of the beacon chain, validator shuffling, and more.

Remember, Teku and Nimbus combines both clients into one process.
{% endhint %}

{% hint style="success" %}
Congratulations. Once your beacon chain is sync'd, validator up and running, you just wait for activation. This process can take 24+ hours. Only 900 new validators can join per day. When you're assigned, your validator will begin creating and voting on blocks while earning staking rewards.

Use [https://beaconcha.in/](https://beaconcha.in/) to create alerts and track your validator's performance.
{% endhint %}

## 🕒5. Time Synchronization

{% hint style="info" %}
Because beacon chain relies on accurate times to perform attestations and produce blocks, your computer's time must be accurate to real NTP or NTS time within 0.5 seconds.
{% endhint %}

Setup **Chrony** with the following guide.

{% page-ref page="../overview-ada/guide-how-to-build-a-haskell-stakepool-node/how-to-setup-chrony.md" %}

{% hint style="info" %}
chrony is an implementation of the Network Time Protocol and helps to keep your computer's time synchronized with NTP.
{% endhint %}

## 🔎6. Monitoring your validator with Grafana and Prometheus

Prometheus is a monitoring platform that collects metrics from monitored targets by scraping metrics HTTP endpoints on these targets. [Official documentation is available here.](https://prometheus.io/docs/introduction/overview/) Grafana is a dashboard used to visualize the collected data.

### 🐣 6.1 Installation

Install prometheus and prometheus node exporter.

```text
sudo apt-get install -y prometheus prometheus-node-exporter
```

Install grafana.

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://packages.grafana.com/oss/deb stable main" > grafana.list
sudo mv grafana.list /etc/apt/sources.list.d/grafana.list
sudo apt-get update && sudo apt-get install -y grafana
```

Enable services so they start automatically.

```bash
sudo systemctl enable grafana-server.service prometheus.service prometheus-node-exporter.service
```

Create the **prometheus.yml** config file. Choose the tab for your eth2 client. Simply copy and paste.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:5054']
   - job_name: 'validators'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:5064']
EOF
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}

{% tab title="Teku" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'nodes'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}

{% tab title="Prysm" %}
```bash
cat > $HOME/prometheus.yml << EOF
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']
  - job_name: 'validator'
    static_configs:
      - targets: ['localhost:8081']
  - job_name: 'beacon node'
    static_configs:
      - targets: ['localhost:8080']
  - job_name: 'slasher'
    static_configs:
      - targets: ['localhost:8082']
EOF
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
cat > $HOME/prometheus.yml << EOF
scrape_configs:
   - job_name: 'node_exporter'
     static_configs:
       - targets: ['localhost:9100']
   - job_name: 'Lodestar'
     metrics_path: /metrics
     static_configs:
       - targets: ['localhost:8008']
EOF
```
{% endtab %}
{% endtabs %}

Setup prometheus for your **eth1 node**. Start by editing **prometheus.yml**

```bash
nano $HOME/prometheus.yml
```

Append the applicable job snippet for your eth1 node to the end of **prometheus.yml**. Save the file.

{% tabs %}
{% tab title="Geth" %}
```bash
   - job_name: 'geth'
     scrape_interval: 15s
     scrape_timeout: 10s
     metrics_path: /debug/metrics/prometheus
     scheme: http
     static_configs:
     - targets: ['localhost:6060']
```
{% endtab %}

{% tab title="Besu" %}
```bash
  - job_name: 'besu'
    scrape_interval: 15s
    scrape_timeout: 10s
    metrics_path: /metrics
    scheme: http
    static_configs:
    - targets:
      - localhost:9545
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
   - job_name: 'nethermind'
    scrape_interval: 15s
    scrape_timeout: 10s
    honor_labels: true
    static_configs:
    - targets: ['localhost:9091']
```

Nethermind monitoring requires [Prometheus Pushgateway](https://github.com/prometheus/pushgateway). Install with the following command.

```bash
sudo apt-get install -y prometheus-pushgateway
```

{% hint style="info" %}
Pushgateway listens for data from Nethermind on port 9091.
{% endhint %}
{% endtab %}

{% tab title="OpenEthereum" %}
```bash
Work in progress
```
{% endtab %}
{% endtabs %}

Move it to `/etc/prometheus/prometheus.yml`

```bash
sudo mv $HOME/prometheus.yml /etc/prometheus/prometheus.yml
```

Finally, restart the services.

```bash
sudo systemctl restart grafana-server.service prometheus.service prometheus-node-exporter.service
```

Verify that the services are running properly:

```text
sudo systemctl status grafana-server.service prometheus.service prometheus-node-exporter.service
```

{% hint style="info" %}
💡 **Reminder**: Ensure port 3000 is open on the firewall and/or port forwarded if you intend to view monitoring info from a different machine.
{% endhint %}

### 📶 6.2 Setting up Grafana Dashboards

1. Open [http://localhost:3000](http://localhost:3000) or http://&lt;your validator's ip address&gt;:3000 in your local browser.
2. Login with **admin** / **admin**
3. Change password
4. Click the **configuration gear** icon, then **Add data Source**
5. Select **Prometheus**
6. Set **Name** to **"Prometheus**"
7. Set **URL** to [http://localhost:9090](http://localhost:9090)
8. Click **Save & Test**
9. **Download and save** your ETH2 Client's json file. \[ [Lighthouse BC ](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/Summary.json)\| [Lighthouse VC](https://raw.githubusercontent.com/sigp/lighthouse-metrics/master/dashboards/ValidatorClient.json) \| [Teku ](https://grafana.com/api/dashboards/13457/revisions/2/download)\| [Nimbus ](https://raw.githubusercontent.com/status-im/nimbus-eth2/master/grafana/beacon_nodes_Grafana_dashboard.json)\| [Prysm ](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/less_10_validators.json)\| [Prysm &gt; 10 Validators](https://raw.githubusercontent.com/GuillaumeMiralles/prysm-grafana-dashboard/master/more_10_validators.json) \| Lodestar \]
10. **Download and save** your ETH1 Client's json file \[ [Geth](https://gist.githubusercontent.com/karalabe/e7ca79abdec54755ceae09c08bd090cd/raw/3a400ab90f9402f2233280afd086cb9d6aac2111/dashboard.json) \| [Besu ](https://grafana.com/api/dashboards/10273/revisions/5/download)\| [Nethermind ](https://raw.githubusercontent.com/NethermindEth/metrics-infrastructure/master/grafana/dashboards/nethermind.json)\| OpenEthereum \]
11. Click **Create +** icon &gt; **Import**
12. Add the ETH2 client dashboard via **Upload JSON file**
13. If needed, select Prometheus as **Data Source**.
14. Click the **Import** button.
15. Repeat steps 11-14 for the ETH1 client dashboard.

{% hint style="info" %}
\*\*\*\*🔥 **Troubleshooting common Grafana issues**:

_The dashboards do not display eth1 node data._

* In the **eth1 unit file** under located at `/etc/systemd/system/eth1.service`,  make sure your eth1 node/geth is started with the correct parameters so that reporting metrics and pprof http server are enabled.
  * Example:`ExecStartPre = /usr/bin/geth --http --metrics --pprof`
{% endhint %}

#### Example of Grafana Dashboards for each ETH2 client.

{% tabs %}
{% tab title="Lighthouse" %}
![Beacon Chain dashboard by sigp](../../.gitbook/assets/lhm.png)

![Validator Client dashboard by sigp](../../.gitbook/assets/lighthouse-validator.png)

Credits: [https://github.com/sigp/lighthouse-metrics/](https://github.com/sigp/lighthouse-metrics/)
{% endtab %}

{% tab title="Nimbus" %}
![Dashboard by status-im](../../.gitbook/assets/nim_dashboard.png)

Credits: [https://github.com/status-im/nimbus-eth2/](https://github.com/status-im/nimbus-eth2/)
{% endtab %}

{% tab title="Teku" %}
![Teku by PegaSys Engineering](../../.gitbook/assets/teku.dash.png)

Credits: [https://grafana.com/grafana/dashboards/13457](https://grafana.com/grafana/dashboards/13457)
{% endtab %}

{% tab title="Prysm" %}
![Prysm dashboard by GuillaumeMiralles](../../.gitbook/assets/prysm_dash.png)

Credits: [https://github.com/GuillaumeMiralles/prysm-grafana-dashboard](https://github.com/GuillaumeMiralles/prysm-grafana-dashboard)
{% endtab %}

{% tab title="Lodestar" %}
Work in progress.
{% endtab %}
{% endtabs %}

#### Example of Grafana Dashboards for each ETH1 node.

{% tabs %}
{% tab title="Geth" %}
![Dashboard by karalabe](../../.gitbook/assets/geth-dash.png)

Credits: [https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd](https://gist.github.com/karalabe/e7ca79abdec54755ceae09c08bd090cd)
{% endtab %}

{% tab title="Besu" %}
![](../../.gitbook/assets/besu-dash.png)

Credits: [https://grafana.com/dashboards/10273](https://grafana.com/dashboards/10273)
{% endtab %}

{% tab title="Nethermind" %}
![](../../.gitbook/assets/nethermind-dash.png)

Credits: [https://github.com/NethermindEth/metrics-infrastructure](https://github.com/NethermindEth/metrics-infrastructure)
{% endtab %}

{% tab title="OpenEthereum" %}
Work in progress
{% endtab %}
{% endtabs %}

### ⚠ 6.3 Setup Alert Notifications

{% hint style="info" %}
Setup alerts to get notified if your validators go offline.
{% endhint %}

Get notified of problems with your validators. Choose between email, telegram, discord or slack.

{% tabs %}
{% tab title="Email Notifications" %}
1. Visit [https://beaconcha.in/](https://beaconcha.in/)
2. Sign up for an account.
3. Verify your **email**
4. Search for your **validator's public address**
5. Add validators to your watchlist by clicking the **bookmark symbol**.
{% endtab %}

{% tab title="Telegram Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.
2. Click on **Add channel**.
3. Give the notification channel a **name**.
4. Select **Telegram** from the Type list.
5. To complete the **Telegram API settings**, a **Telegram channel** and **bot** are required. For instructions on setting up a bot with `@Botfather`, see [this section](https://core.telegram.org/bots#6-botfather) of the Telegram documentation. You need to create a BOT API token.
6. Create a new telegram group.
7. Invite the bot to your new group.
8. Type at least 1 message into the group to initialize it.
9. Visit [`https://api.telegram.org/botXXX:YYY/getUpdates`](https://api.telegram.org/botXXX:YYY/getUpdates) where `XXX:YYY` is your BOT API Token.
10. In the JSON response, find and copy the **Chat ID**. Find it between **chat** and **title**. _Example of Chat ID_:  `-1123123123`

    ```text
    "chat":{"id":-123123123,"title":
    ```

11. Paste the **Chat ID** into the corresponding field in **Grafana**.
12. **Save and test** the notification channel for your alerts.
13. Now you can create custom alerts from your dashboards. [Visit here to learn how to create alerts.](https://grafana.com/docs/grafana/latest/alerting/create-alerts/)
{% endtab %}

{% tab title="Discord Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.
2. Click on **Add channel**.
3. Add a **name** to the notification channel.
4. Select **Discord** from the Type list.
5. To complete the set up, a Discord server \(and a text channel available\) as well as a Webhook URL are required. For instructions on setting up a Discord's Webhooks, see [this section](https://support.discord.com/hc/en-us/articles/228383668-Intro-to-Webhooks) of their documentation.
6. Enter the Webhook **URL** in the Discord notification settings panel.
7. Click **Send Test**, which will push a confirmation message to the Discord channel.
{% endtab %}

{% tab title="Slack Notifications" %}
1. On the menu of Grafana, select **Notification channels** under the bell icon.
2. Click on **Add channel**.
3. Add a **name** to the notification channel.
4. Select **Slack** from the Type list.
5. For instructions on setting up a Slack's Incoming Webhooks, see [this section](https://api.slack.com/messaging/webhooks) of their documentation.
6. Enter the Slack Incoming Webhook URL in the **URL** field.
7. Click **Send Test**, which will push a confirmation message to the Slack channel.
{% endtab %}
{% endtabs %}

### 🌊 6.4 Monitoring with Uptime Check by Google Cloud

{% hint style="info" %}
Who watches the watcher? With an external 3rd party tool like Uptime Check, you can have greater reassurance your validator is functioning in case of disasters such as power failure, hardware failure or internet outage. In these scenarios, the previously mentioned monitoring by Prometheus and Grafana would likely cease to function as well.

Credits to [Mohamed Mansour for inspiring this how-to guide](https://www.youtube.com/watch?v=txgOVDTemPQ).
{% endhint %}

Here's how to setup a no-cost monitoring service called Uptime Check by Google.

{% hint style="info" %}
For a video demo, watch [MohamedMansour's eth2 education videos](https://www.youtube.com/watch?v=txgOVDTemPQ). Please support his [GITCOIN grant](https://gitcoin.co/grants/1709/video-educational-grant). 🙏
{% endhint %}

1. Visit [cloud.google.com](https://cloud.google.com/)
2. Search for **Monitoring** in the search field.
3. Click **Select a Project to Start Monitoring**.
4. Click **New Project.**
5. **Name** your project and click **Create.**
6. From the notifications menu, select your new project.
7. On the right column, there's a Monitoring Card. Click **Go to Monitoring**.
8. On the left menu, click **Uptime checks** and then **CREATE UPTIME CHECK.**
9. Type in a title i.e. _**Geth node**_
10. Select protocol as _**TCP**_
11. Enter your public IP address and port number. i.e. ip=**7.55.6.3** and port=**30303**
12. Select your desired frequency to check i.e. **5 minutes.**
13. Choose the region closest to you to check from. Click Next.
14. Create a Notification Channel. Click **Manage Notification Channels.**
15. Choose your desired settings. Pick from any or all of Slack, Webhook, Email or SMS.
16. Go back to Create Uptime Check window.
17. Within the notifications field, click the refresh button to load your new notification channels.
18. Select desired notifications.
19. Click **TEST** to verify your notifications are setup correctly.
20. Click **CREATE** to finish.

{% hint style="success" %}
🎉Congrats on setting up your validator! You're good to go on eth2.0.

Did you find our guide useful? Let us know with a tip and we'll keep updating it.

Use [cointr.ee to find our donation ](https://cointr.ee/coincashew)addresses. 🙏

Any feedback and all pull requests much appreciated. 🌛

Hang out and chat with fellow stakers on Discord @

[https://discord.gg/w8Bx8W2HPW](https://discord.gg/w8Bx8W2HPW) 😃
{% endhint %}

## 🧙♂7. Update a ETH2 client

When a new release is cut, you will want to update to the latest stable release. The following shows you how to update your eth2 beacon chain and validator.

{% hint style="info" %}
Always review the **git logs with command`git log`** or **release notes** before updating. There may be changes requiring your attention.
{% endhint %}

Select your ETH2 client.

{% tabs %}
{% tab title="Lighthouse" %}
Review release notes and check for breaking changes/features.

[https://github.com/sigp/lighthouse/releases](https://github.com/sigp/lighthouse/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/lighthouse
git fetch --all && git checkout stable && git pull
make
```

Verify the build completed by checking the new version number.

```bash
lighthouse --version
```

Restart beacon chain and validator as per normal operating procedures.

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus" %}
Review release notes and check for breaking changes/features.

[https://github.com/status-im/nimbus-eth2/releases](https://github.com/status-im/nimbus-eth2/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/nimbus-eth2
git pull && make update
make NIMFLAGS="-d:insecure" nimbus_beacon_node
```

Verify the build completed by checking the new version number.

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --version
```

Stop, copy new binary, and restart beacon chain and validator as per normal operating procedures.

```bash
sudo systemctl stop beacon-chain
sudo rm /usr/bin/nimbus_beacon_node
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/bin
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Teku" %}
Review release notes and check for breaking changes/features.

[https://github.com/ConsenSys/teku/releases](https://github.com/ConsenSys/teku/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/teku
git pull
./gradlew distTar installDist
```

Verify the build completed by checking the new version number.

```bash
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

Restart beacon chain and validator as per normal operating procedures.

```bash
sudo systemctl stop beacon-chain
sudo rm -rf /usr/bin/teku
sudo cp -r $HOME/git/teku/build/install/teku /usr/bin/teku
sudo systemctl reload-or-restart beacon-chain
```
{% endtab %}

{% tab title="Prysm" %}
Review release notes and check for breaking changes/features. [https://github.com/prysmaticlabs/prysm/releases](https://github.com/prysmaticlabs/prysm/releases)

```bash
#Simply restart the processes
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}

{% tab title="Lodestar" %}
Review release notes and check for breaking changes/features.

[https://github.com/ChainSafe/lodestar/releases](https://github.com/ChainSafe/lodestar/releases)

Pull the latest source and build it.

```bash
cd $HOME/git/lodestar
git pull
yarn install
yarn run build
```

Verify the build completed by checking the new version number.

```bash
yarn run cli --version
```

Restart beacon chain and validator as per normal operating procedures.

```text
sudo systemctl reload-or-restart beacon-chain validator
```
{% endtab %}
{% endtabs %}

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl status beacon-chain
```
{% endtab %}
{% endtabs %}

## 🔥8. Additional Useful Tips

### 🛑 8.1 Voluntary exit a validator

{% hint style="info" %}
Use this command to signal your intentions to stop validating with your validator. This means you no longer want to stake with your validator and want to turn off your node.

* Voluntary exiting takes a minimum of 2048 epochs \(or ~9days\). There is a queue to exit and a delay before your validator is finally exited.
* Once a validator is exited in phase 0, this is non-reversible and you can no longer restart validating again.
* Your funds will not be available for withdrawal until phase 1.5 or later.
* After your validator leaves the exit queue and is truely exited, it is safe to turn off your beacon node and validator.
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator exit \
--keystore $HOME/.lighthouse/mainnet/validators \
--beacon-node http://localhost:5052
```
{% endtab %}

{% tab title="Teku" %}
```bash
teku voluntary-exit \
--epoch=<epoch number to exit> \
--beacon-node-api-endpoint=http://127.0.0.1:5051 \
--validator-keys=<path to keystore.json>:<path to password.txt file>
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
build/nimbus_beacon_node deposits exit --validator=<VALIDATOR_PUBLIC_KEY> --data-dir=/var/lib/nimbus
```
{% endtab %}

{% tab title="Prysm" %}
```bash
$HOME/prysm/prysm.sh validator accounts voluntary-exit
```
{% endtab %}

{% tab title="Lodestar" %}
```bash
#TO BE DETERMINED
```
{% endtab %}
{% endtabs %}

### 🗝 8.2 Verify your mnemonic phrase

Using the eth2deposit-cli tool, ensure you can regenerate the same eth2 key pairs by restoring your `validator_keys`

```bash
cd $HOME/eth2deposit-cli
./deposit.sh existing-mnemonic --chain mainnet
```

{% hint style="info" %}
When the **pubkey** in both **keystore files** are **identical,** this means your mnemonic phrase is veritably correct. Other fields will be different because of salting.
{% endhint %}

### 🤖8.3 Add additional validators

Using the eth2deposit-cli tool, you can add more validators by creating a new deposit data file and `validator_keys`

First, backup and move your existing `validator_key` directory and append the date to the end.

```bash
cd $HOME/eth2deposit-cli
mv validator_key validator_key_$(date +"%Y%d%m-%H%M%S")
```

For example, in case we originally created 3 validators but now wish to add 5 more validators, we could use the following command.

```bash
cd $HOME/eth2deposit-cli
./deposit.sh existing-mnemonic --validator_start_index 3 --num_validators 5 --chain mainnet
```

Complete the steps of uploading the `deposit_data-#########.json` to the launch pad site and making your corresponding 32 ETH deposit transactions.

Finish by stopping your validator, importing the new validator key\(s\), restarting your validator and verifying the logs ensuring everything still works without error.

### 💸 8.4 Switch / migrate Eth2 clients with slash protection

{% hint style="info" %}
The key takeaway in this process is to avoid running two eth2 clients simultaneously. You want to avoid being punished by a slashing penalty, which causes a loss of ether.
{% endhint %}

#### 🛑 8.4.1 Stop old beacon chain and old validator.

In order to export the slashing database, the validator needs to be stopped.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl stop beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl stop beacon-chain
```
{% endtab %}
{% endtabs %}

#### 💽 8.4.2 Export slashing database \(Optional\)

{% hint style="info" %}
[EIP-3076](https://eips.ethereum.org/EIPS/eip-3076) implements a standard to safety migrate validator keys between eth2 clients. This is the exported contents of the slashing database.
{% endhint %}

Update the export .json file location and name.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection export <lighthouse_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
To be implemented
{% endtab %}

{% tab title="Teku" %}
```bash
teku slashing-protection export --to=<FILE>
```
{% endtab %}

{% tab title="Prysm" %}
To be implemented
{% endtab %}

{% tab title="Lodestar" %}
To be implemented
{% endtab %}
{% endtabs %}

#### 🚧 8.4.3 Setup and install new validator / beacon chain

Now you need to setup/install your new validator **but do not start running the systemd processes**. Be sure to thoroughly follow your new validator's  [Section 4. Configure a ETH2 beacon chain and validator.](guide-or-how-to-setup-a-validator-on-eth2-testnet.md#4-configure-a-eth2-beacon-chain-node-and-validator) You will need to build/install the client, configure port forwarding/firewalls, and new systemd unit files.

{% hint style="warning" %}
\*\*\*\*✨ **Pro Tip**: During the process of re-importing validator keys, **wait at least 13 minutes** or two epochs to prevent slashing penalties. You must avoid running two eth2 clients with same validator keys at the same time.
{% endhint %}

{% hint style="danger" %}
🛑 **Critical Step**: Do not start any **systemd processes** until either you have **imported the slashing database** or you have **waited at least 13 minutes or two epochs**.
{% endhint %}

#### 📂 8.4.4 Import slashing database \(Optional\)

Using your new eth2 client, run the following command and update the relevant path to import your slashing database from 2 steps ago.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
lighthouse account validator slashing-protection import <my_interchange.json>
```
{% endtab %}

{% tab title="Nimbus" %}
To be implemented
{% endtab %}

{% tab title="Teku" %}
```bash
teku slashing-protection import --from=<FILE>
```
{% endtab %}

{% tab title="Prysm" %}
To be implemented
{% endtab %}

{% tab title="Lodestar" %}
To be implemented
{% endtab %}
{% endtabs %}

#### 🌠 8.4.5 Start new validator and new beacon chain

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl start beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl start beacon-chain
```
{% endtab %}
{% endtabs %}

#### 🔥 8.4.6 Verify functionality

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl status beacon-chain
```
{% endtab %}
{% endtabs %}

Finally, verify your validator's attestations are working with public block explorer such as

[https://beaconcha.in/](https://beaconcha.in/)

Enter your validator's pubkey to view its status.

#### 🧯 8.4.7 Update Monitoring with Prometheus and Grafana

[Review section 6](guide-or-how-to-setup-a-validator-on-eth2-testnet.md#6-monitoring-your-validator-with-grafana-and-prometheus) and change your `prometheus.yml`. Ensure prometheus is connected to your new eth2 client's metrics port. You will also want to import your new eth2 client's dashboard.

### 🖥 8.5 Use all available LVM disk space

During installation of Ubuntu Server, a common issue arises where your hard drive's space is not fully available for use.

```bash
# View your disk drives
df

# Change the logical volume filesystem path if required
lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv

exit
# Resize file system to use the new available space in the logical volume
resize2fs /dev/ubuntu-vg/ubuntu-lv

## Verify new available space
df -h
```

**Source reference**:

{% embed url="https://askubuntu.com/questions/1106795/ubuntu-server-18-04-lvm-out-of-space-with-improper-default-partitioning" %}

### 🚦 8.6 Reduce network bandwidth usage

{% hint style="info" %}
Hosting your own ETH1 node can consume hundreds of gigabytes of data per day. Because data plans can be limited or costly, you might desire to slow down data usage but still maintain good connectivity to the network.
{% endhint %}

Edit your eth1.service unit file.

```bash
sudo nano /etc/systemd/system/eth1.service
```

Add the following flag to limit the number of peers on the `ExecStart` line.

{% tabs %}
{% tab title="Geth" %}
```bash
--maxpeers 10
# Example
# ExecStart       = /usr/bin/geth --maxpeers 10 --http --ws
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
--max-peers 10
# Example
# ExecStart       = <home directory>/openethereum/openethereum --max-peers 10
```
{% endtab %}

{% tab title="Besu" %}
```bash
--max-peers 10
# Example
# ExecStart       = <home directory>/besu/bin/besu --max-peers 10 --rpc-http-enabled
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--ActivePeersMaxCount 10
# Example
# ExecStart       = <home directory>/nethermind/Nethermind.Runner --ActivePeersMaxCount 10 --JsonRpc.Enabled true
```
{% endtab %}
{% endtabs %}

Finally, reload the new unit file and restart the eth1 node.

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

### 📂 8.7 Important directory locations

{% hint style="info" %}
In case you need to locate your validator keys, database directories or other important files.
{% endhint %}

#### Eth2 Client files and locations

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# Validator Keys
~/.lighthouse/mainnet/validators

# Beacon Chain Data
~/.lighthouse/mainnet/beacon

# List of all validators and passwords
~/.lighthouse/mainnet/validators/validator_definitions.yml

#Slash protection db
~/.lighthouse/mainnet/validators/slashing_protection.sqlite
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# Validator Keys
/var/lib/nimbus/validators

# Beacon Chain Data
/var/lib/nimbus/db

#Slash protection db
/var/lib/nimbus/validators/slashing_protection.sqlite3

#Logs
/var/lib/nimbus/beacon.log
```
{% endtab %}

{% tab title="Teku" %}
```bash
# Validator Keys
/var/lib/teku

# Beacon Chain Data
~/tekudata/beacon

#Slash protection db
~/tekudata/validator/slashprotection
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# Validator Keys
~/.eth2validators/prysm-wallet-v2/direct

# Beacon Chain Data
~/.eth2/beaconchaindata
```
{% endtab %}

{% tab title="Lodestar" %}
TBD
{% endtab %}
{% endtabs %}

#### Eth1 node files and locations

{% tabs %}
{% tab title="Geth" %}
```bash
# database location
$HOME/.ethereum
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
# database location
$HOME/.local/share/openethereum
```
{% endtab %}

{% tab title="Besu" %}
```bash
# database location
$HOME/.besu/database
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
#database location
$HOME/.nethermind/nethermind_db/mainnet
```
{% endtab %}
{% endtabs %}

###  🌏 8.8 Hosting ETH1 node on a different machine

{% hint style="info" %}
Hosting your own ETH1 node on a different machine than where your beacon-chain and validator resides, can allow some extra modularity and flexibility.
{% endhint %}

On the eth1 node machine, edit your eth1.service unit file.

```bash
sudo nano /etc/systemd/system/eth1.service
```

Add the following flag to allow remote incoming http and or websocket api requests on the `ExecStart` line.

{% hint style="info" %}
If not using websockets, there's no need to include ws parameters. Only Nimbus requires websockets.
{% endhint %}

{% tabs %}
{% tab title="Geth" %}
```bash
--http.addr 0.0.0.0 --ws.addr 0.0.0.0
# Example
# ExecStart       = /usr/bin/geth --http.addr 0.0.0.0 --ws.addr 0.0.0.0 --http --ws
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
```bash
--jsonrpc-interface=all --ws-interface=all
# Example
# ExecStart       = <home directory>/openethereum/openethereum --jsonrpc-interface=all --ws-interface=all
```
{% endtab %}

{% tab title="Besu" %}
```bash
--rpc-http-host=0.0.0.0 --rpc-ws-enabled --rpc-ws-host=0.0.0.0
# Example
# ExecStart       = <home directory>/besu/bin/besu --rpc-http-host=0.0.0.0 --rpc-ws-enabled --rpc-ws-host=0.0.0.0 --rpc-http-enabled
```
{% endtab %}

{% tab title="Nethermind" %}
```bash
--JsonRpc.Host 0.0.0.0 --WebSocketsEnabled
# Example
# ExecStart       = <home directory>/nethermind/Nethermind.Runner --JsonRpc.Host 0.0.0.0 --WebSocketsEnabled --JsonRpc.Enabled true
```
{% endtab %}
{% endtabs %}

Reload the new unit file and restart the eth1 node.

```bash
sudo systemctl daemon-reload
sudo systemctl restart eth1
```

On the separate machine hosting the beacon-chain, update the beacon-chain unit file with the eth1 node's IP address.

{% tabs %}
{% tab title="Lighthouse" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --eth1-endpoint parameter
# example
# --eth1-endpoint=http://192.168.10.22
```
{% endtab %}

{% tab title="Nimbus" %}
```bash
# edit beacon chain unit file
nano /etc/systemd/system/beacon-chain.service
# modify the --web-url parameter
# example
# --web3-url=ws://192.168.10.22
```
{% endtab %}

{% tab title="Teku" %}
```bash
# edit teku.yaml
nano /etc/teku/teku.yaml
# change the eth1-endpoint
# example
# eth1-endpoint: "http://192.168.10.20:8545"
```
{% endtab %}

{% tab title="Prysm" %}
```bash
# edit beacon-chain unit file
nano /etc/systemd/system/beacon-chain.service
# add the --http-web3provider parameter
# example
# --http-web3provider=http://192.168.10.20:8545
```
{% endtab %}

{% tab title="Lodestar" %}
```
tbd.
```
{% endtab %}
{% endtabs %}

Reload the updated unit file and restart the beacon-chain.

```bash
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```

### 🎊 8.9 Add or change POAP graffiti flag

Setup your `graffiti`, a custom message included in blocks your validator successfully proposes, and earn an early beacon chain validator POAP token. [Generate your POAP string by supplying an Ethereum 1.0 address here.](https://beaconcha.in/poap)

Run the following command to set the `MY_GRAFFITI` variable. Replace `<my POAP string or message>`  between the single quotes.

```bash
MY_GRAFFITI='<my POAP string or message>'
# Examples
# MY_GRAFFITI='poapAAAAACGatUA1bLuDnL4FMD13BfoD'
# MY_GRAFFITI='eth2 rulez!'
```

{% hint style="info" %}
Learn more about [POAP - The Proof of Attendance token. ](https://www.poap.xyz/)
{% endhint %}

{% tabs %}
{% tab title="Lighthouse" %}
Run the following to re-create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(which lighthouse) vc --network mainnet --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Nimbus" %}
Run the following to re-create a **unit file** to define your`beacon-chain.service` configuration. Simply copy and paste.

```bash
cat > $HOME/beacon-chain.service << EOF
# The eth2 beacon chain service (part of systemd)
# file: /etc/systemd/system/beacon-chain.service

[Unit]
Description     = eth2 beacon chain service
Wants           = network-online.target
After           = network-online.target

[Service]
Type            = simple
User            = $(whoami)
WorkingDirectory= /var/lib/nimbus
Environment     = "ClientIP=\$(curl -s ident.me)"
ExecStart       = /bin/bash -c '/usr/bin/nimbus_beacon_node --network=mainnet --graffiti="${MY_GRAFFITI}" --data-dir=/var/lib/nimbus --nat=extip:\${ClientIP} --web3-url=ws://127.0.0.1:8546 --metrics --metrics-port=8008 --rpc --rpc-port=9091 --validators-dir=/var/lib/nimbus/validators --secrets-dir=/var/lib/nimbus/secrets --log-file=/var/lib/nimbus/beacon.log --max-peers=100'
Restart         = on-failure

[Install]
WantedBy    = multi-user.target
EOF
```

{% hint style="warning" %}
Nimbus only supports websocket connections \("ws://" and "wss://"\) for the ETH1 node. Geth, OpenEthereum, Infura and Chainstack ETH1 nodes are verified compatible.
{% endhint %}

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/beacon-chain.service /etc/systemd/system/beacon-chain.service
```

Update file permissions.

```bash
sudo chmod 644 /etc/systemd/system/beacon-chain.service
```
{% endtab %}

{% tab title="Teku" %}
Re-generate your Teku Config file. Simply copy and paste.

```bash
cat > $HOME/teku.yaml << EOF
# network
network: "mainnet"

# p2p
p2p-enabled: true
p2p-port: 9000
# validators
validator-keys: "/var/lib/teku/validator_keys:/var/lib/teku/validator_keys"
validators-graffiti: "${MY_GRAFFITI}"

# Eth 1
eth1-endpoint: "http://localhost:8545"

# metrics
metrics-enabled: true
metrics-categories: ["BEACON","LIBP2P","NETWORK"]
metrics-port: 8008

# database
data-path: "$(echo $HOME)/tekudata"
data-storage-mode: "archive"

# rest api
rest-api-port: 5051
rest-api-docs-enabled: true
rest-api-enabled: true

# logging
log-include-validator-duties-enabled: true
log-destination: CONSOLE
EOF
```

Move the config file to `/etc/teku`

```bash
sudo mv $HOME/teku.yaml /etc/teku/teku.yaml
```
{% endtab %}

{% tab title="Prysm" %}
Re-create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
ExecStart       = $(echo $HOME)/prysm/prysm.sh validator --mainnet --graffiti "${MY_GRAFFITI}" --accept-terms-of-use --wallet-password-file $(echo $HOME)/.eth2validators/validators-password.txt
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

 Update its permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}

{% tab title="Lodestar" %}
Run the following to re-create a **unit file** to define your`validator.service` configuration. Simply copy and paste.

```bash
cat > $HOME/validator.service << EOF
# The eth2 validator service (part of systemd)
# file: /etc/systemd/system/validator.service

[Unit]
Description     = eth2 validator service
Wants           = network-online.target beacon-chain.service
After           = network-online.target

[Service]
User            = $(whoami)
WorkingDirectory= $(echo $HOME)/git/lodestar
ExecStart       = yarn run cli validator run --network mainnet --graffiti "${MY_GRAFFITI}"
Restart         = on-failure

[Install]
WantedBy	= multi-user.target
EOF
```

Move the unit file to `/etc/systemd/system`

```bash
sudo mv $HOME/validator.service /etc/systemd/system/validator.service
```

 Update its permissions.

```bash
sudo chmod 644 /etc/systemd/system/validator.service
```
{% endtab %}
{% endtabs %}

Reload the updated unit file and restart the validator process for your graffiti to take effect.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl daemon-reload
sudo systemctl restart validator
```
{% endtab %}

{% tab title="Teku \| Nimbus" %}
```
sudo systemctl daemon-reload
sudo systemctl restart beacon-chain
```
{% endtab %}
{% endtabs %}

### 📦 8.10 Update a ETH1 node - Geth / OpenEthereum / Besu / Nethermind

{% hint style="info" %}
From time to time, be sure to update to the latest ETH1 releases to enjoy new improvements and features.
{% endhint %}

Stop your eth2 beacon chain, validator, and eth1 node processes.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
# This can take some time.
sudo systemctl stop validator beacon-chain eth1
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```bash
# This can take some time.
sudo systemctl stop beacon-chain eth1
```
{% endtab %}
{% endtabs %}

Update the eth1 node package or binaries.

{% tabs %}
{% tab title="Geth" %}
Review the latest release notes at [https://github.com/ethereum/go-ethereum/releases](https://github.com/ethereum/go-ethereum/releases)

```bash
sudo apt update
sudo apt upgrade -y
```
{% endtab %}

{% tab title="OpenEthereum \(Parity\)" %}
Review the latest release at [https://github.com/openethereum/openethereum/releases](https://github.com/openethereum/openethereum/releases)

Automatically download the latest linux release, un-zip, add execute permissions and cleanup.

```bash
cd $HOME
# backup previous openethereum version in case of rollback
mv openethereum openethereum_backup_$(date +"%Y%d%m-%H%M%S")
# store new version in openethreum directory
mkdir openethereum && cd openethereum
# download latest version
curl -s https://api.github.com/repos/openethereum/openethereum/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
# unzip
unzip openethereum*.zip
# add execute permission
chmod +x openethereum
# cleanup
rm openethereum*.zip
```
{% endtab %}

{% tab title="Besu" %}
Review the latest release at [https://github.com/hyperledger/besu/releases](https://github.com/hyperledger/besu/releases)

File can be downloaded from [https://dl.bintray.com/hyperledger-org/besu-repo](https://dl.bintray.com/hyperledger-org/besu-repo)

Manually find the desired file from above repo and modify the `wget` command with the URL.

> Example:
>
> wget -O besu.tar.gz [https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz](https://dl.bintray.com/hyperledger-org/besu-repo/besu-20.10.1.tar.gz)

```bash
cd $HOME
# backup previous besu version in case of rollback
mv besu besu_backup_$(date +"%Y%d%m-%H%M%S")
# download latest besu
wget -O besu.tar.gz <https URL to latest tax.gz linux file>
# untar
tar -xvf besu.tar.gz
# cleanup
rm besu.tar.gz
# rename besu to standard folder location
mv besu* besu
```
{% endtab %}

{% tab title="Nethermind" %}
Review the latest release at [https://github.com/NethermindEth/nethermind/releases](https://github.com/NethermindEth/nethermind/releases)

Automatically download the latest linux release, un-zip and cleanup.

```bash
cd $HOME
# backup previous nethermind version in case of rollback
mv nethermind nethermind_backup_$(date +"%Y%d%m-%H%M%S")
# store new version in nethermind directory
mkdir nethermind && cd nethermind
# download latest version
curl -s https://api.github.com/repos/NethermindEth/nethermind/releases/latest | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux  | xargs wget -q --show-progress
# unzip
unzip -o nethermind*.zip
# cleanup
rm nethermind*linux*.zip
```
{% endtab %}
{% endtabs %}

Start your eth2 beacon chain, validator, and eth1 node processes.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl start eth1 beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl start eth1 beacon-chain
```
{% endtab %}
{% endtabs %}

Check the logs to verify the services are working properly and ensure there are no errors.

{% tabs %}
{% tab title="Lighthouse \| Prysm \| Lodestar" %}
```bash
sudo systemctl status eth1 status beacon-chain validator
```
{% endtab %}

{% tab title="Nimbus \| Teku" %}
```
sudo systemctl status eth1 beacon-chain
```
{% endtab %}
{% endtabs %}

Finally, verify your validator's attestations are working with public block explorer such as

[https://beaconcha.in/](https://beaconcha.in/)

Enter your validator's pubkey to view its status.

### 🖴 8.13 Dealing with storage issues on the Eth1 node

{% hint style="info" %}
It is currently recommended to use a minimum 1TB hard disk.
{% endhint %}

After running the Eth1 node for a while, you will notice that it will start to fill up the hard disk.
The following steps might be helpful for you.

Firstly make sure you have a fallback Eth1 node: see [8.11 Strategy 2](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet#strategy-2-eth1-redundancy).

{% tabs %}
{% tab title="Manually Pruning Geth" %}

{% hint style="info" %}
Since Geth 1.10x version, the blockchain data can be regularly pruned to reduce it's size.
{% endhint %}

Reference: https://gist.github.com/yorickdowne/3323759b4cbf2022e191ab058a4276b2

You will need to upgrade Geth to at least 1.10x. Other prerequisites are a fully synced Eth1 node and that a snapshot has been created.

Stop your Eth1 node

```bash
sudo systemctl stop eth1
```

Prune the blockchain data

```bash
geth --datadir ~/.ethereum snapshot prune-state
```

Restart the Eth1 node

```bash
sudo systemctl start eth1
```
{% endtab %}

{% tab title="Adding new hard disks and changing the data directory" %}

After you have installed your hard disk, you will need to properly format it and automount it. Consult the ubuntu guides on this.

I will assume that the new disk has been mounted onto `/mnt/eth1-data`. (The name of the mount point is up to you)

Handling file permissions.

You need to change ownership of the folder to be accessible by your `eth1 service`. If your folder is a different name, please change the `/mnt/eth1-data` accordingly.

```bash
sudo chown $whoami:$whoami /mnt/eth1-data
```
```bash
sudo chmod 755 /mnt/eth1-data
```

Stop your Eth1 node.

```bash
sudo systemctl stop eth1
```

Edit the system service file to point to a new data directory.

```bash
sudo nano /etc/systemd/system/eth1.service
```

At the end of this command starting with `/usr/bin/geth --http --metrics .... ` add a space and the following flag `--datadir "/mnt/eth1-data"`.

`Ctrl-X` to save your settings.

Refresh the system service daemon to load the new configurations.

```bash
sudo systemctl daemon-reload
```

Restart the Eth1 node.

```bash
sudo systemctl start eth1
```

Make sure it is up and running by viewing the running logs.

```bash
journalctl -u eth1 -f
```

(Optional) Delete original data directory

```bash
rm -r ~/.ethereum
```

{% endtab %}
{% endtabs %}



## 🌇 9. Join the community on Discord and Reddit

### 📱 Discord

{% tabs %}
{% tab title="Lighthouse" %}
{% embed url="https://discord.gg/cyAszAh" %}
{% endtab %}

{% tab title="Nimbus" %}
{% embed url="https://discord.gg/XRxWahP" %}
{% endtab %}

{% tab title="Teku" %}
{% embed url="https://discord.gg/7hPv2T6" %}
{% endtab %}

{% tab title="Prysm" %}
{% embed url="https://discord.gg/XkyZSSk4My" %}
{% endtab %}

{% tab title="Lodestar" %}
{% embed url="https://discord.gg/aMxzVcr" %}
{% endtab %}

{% tab title="CoinCashew" %}
{% embed url="https://discord.gg/w8Bx8W2HPW" %}
{% endtab %}
{% endtabs %}

### 🌍 Reddit r/ethStaker

{% embed url="https://www.reddit.com/r/ethstaker/" %}

## 🧩10. Reference Material

Appreciate the hard work done by the fine folks at the following links which served as a foundation for creating this guide.

{% embed url="https://launchpad.ethereum.org/" %}

{% embed url="https://pegasys.tech/teku-ethereum-2-for-enterprise/" %}

{% embed url="https://docs.teku.pegasys.tech/en/latest/HowTo/Get-Started/Build-From-Source/" %}

{% embed url="https://lighthouse-book.sigmaprime.io/intro.html" caption="" %}

{% embed url="https://status-im.github.io/nimbus-eth2/intro.html" %}

{% embed url="https://prylabs.net/participate" %}

{% embed url="https://docs.prylabs.network/docs/getting-started/" %}

{% embed url="https://chainsafe.github.io/lodestar/installation/" %}

## 🎉11. Bonus links

### 🧱 ETH2 Block Explorers

{% embed url="https://beaconcha.in/" %}

{% embed url="https://beaconscan.com/" caption="" %}

### 🗒 Latest Eth2 Info

{% embed url="https://hackmd.io/@benjaminion/eth2\_news/" caption="" %}

{% embed url="https://www.reddit.com/r/ethstaker" caption="" %}

{% embed url="https://blog.ethereum.org/" caption="" %}

### 👨👩👧👦 Additional ETH2 Community Guides

{% embed url="https://someresat.medium.com/" %}

{% embed url="https://github.com/metanull-operator/eth2-ubuntu" %}

{% embed url="https://agstakingco.gitbook.io/eth-2-0-staking-guide-medalla/" %}

#### Hardware Staking Guide [https://www.reddit.com/r/ethstaker/comments/j3mlup/a\_slightly\_updated\_look\_at\_hardware\_for\_staking/](https://www.reddit.com/r/ethstaker/comments/j3mlup/a_slightly_updated_look_at_hardware_for_staking/)

{% embed url="https://medium.com/@RaymondDurk/how-to-stake-for-ethereum-2-0-with-dappnode-231fa7689c02" %}

{% embed url="https://kb.beaconcha.in/" %}



