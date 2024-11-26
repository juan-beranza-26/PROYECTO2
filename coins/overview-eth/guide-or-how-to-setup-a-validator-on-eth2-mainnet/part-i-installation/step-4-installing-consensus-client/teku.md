# Teku

## Vision General

{% hint style="info" %}
[PegaSys Teku](https://consensys.net/knowledge-base/ethereum-2/teku/) (anteriormente conocido como Artemis) es un cliente de Ethereum basado en Java, diseñado y construido para satisfacer las necesidades institucionales y los requisitos de seguridad. PegaSys es una rama de [ConsenSys](https://consensys.net) dedicada a la creación de clientes y herramientas listos para la empresa para interactuar con la plataforma central de Ethereum. Teku tiene licencia Apache 2 y está escrito en Java, un lenguaje notable por su madurez y ubicuidad.
{% endhint %}

{% hint style="info" %}
**Nota**: Teku está configurado para ejecutar tanto el **validator client** como **beacon chain client** en un solo proceso.
{% endhint %}

#### Enlaces Oficiales

| Asunto       | Enlaces                                                                                                         |
| ------------- | ------------------------------------------------------------------------------------------------------------- |
| Lanzamientos      | [https://github.com/ConsenSys/teku/releases](https://github.com/ConsenSys/teku/releases)                      |
| Documentación | [https://docs.teku.consensys.net/introduction](https://docs.teku.consensys.net/introduction)                  |
| Sitio web       | [https://consensys.net/knowledge-base/ethereum-2/teku](https://consensys.net/knowledge-base/ethereum-2/teku/) |

### 1. Configuración Inicial

Cree un usuario de servicio para el servicio de consenso, cree un directorio de datos y asigne la propiedad.

```bash
sudo adduser --system --no-create-home --group consensus
sudo mkdir -p /var/lib/teku
sudo chown -R consensus:consensus /var/lib/teku
```

Instale dependencias. .

```bash
sudo apt install curl ccze openjdk-17-jdk libsnappy-dev libc6-dev jq git libc6 unzip -y
```

### 2. Instalar Binarios

* La descarga de archivos binarios suele ser más rápida y cómoda.
* La construcción a partir del código fuente puede ofrecer una mejor compatibilidad y está más alineada con el espíritu del software libre y abierto FOSS (software libre de código abierto).

<details>

<summary>Opción 1 - Descargar archivos binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, un-tar y cleanup.

```bash
RELEASE_URL="https://api.github.com/repos/ConsenSys/teku/releases/latest"
LATEST_TAG="$(curl -s $RELEASE_URL | jq -r ".tag_name")"
BINARIES_URL="https://artifacts.consensys.net/public/teku/raw/names/teku.tar.gz/versions/${LATEST_TAG}/teku-${LATEST_TAG}.tar.gz"
echo Downloading URL: $BINARIES_URL

cd $HOME
# Download
wget -O teku.tar.gz $BINARIES_URL
# Untar
tar -xzvf teku.tar.gz -C $HOME
# Rename folder
mv teku-* teku
# Cleanup
rm teku.tar.gz
```

Instale los archivos binarios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo mv $HOME/teku /usr/local/bin/teku
</strong></code></pre>

</details>

<details>

<summary>Opción 2 - Compilar a partir del código fuente</summary>

Compile los archivos binarios.

```bash
mkdir -p ~/git
cd ~/git
git clone https://github.com/ConsenSys/teku.git
cd teku
# Get new tags
git fetch --tags
RELEASETAG=$(curl -s https://api.github.com/repos/ConsenSys/teku/releases/latest | jq -r .tag_name)
git checkout tags/$RELEASETAG
./gradlew distTar installDist
```

Verifica que Teku se haya construido correctamente mostrando la versión.

```shell
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

Instale los archivos binarios.

```shell
sudo cp -a $HOME/git/teku/build/install/teku /usr/local/bin/teku
```

</details>

### **3. Instalar y Configurar systemd**

Cree un **archivo de unidad systemd** para definir su `consensus.service` configuración.

```bash
sudo nano /etc/systemd/system/consensus.service
```

Pegue la siguiente configuración en el archivo.

{% tabs %}
{% tab title="Standalone Beacon Node (Recommended)" %}
```shell
[Unit]
Description=Teku Beacon Node Consensus Client service for Mainnet
Wants=network-online.target
After=network-online.target
Documentation=https://www.coincashew.com

[Service]
Type=simple
User=consensus
Group=consensus
Restart=on-failure
RestartSec=3
KillSignal=SIGINT
TimeoutStopSec=900
Environment=JAVA_OPTS=-Xmx6g
Environment=TEKU_OPTS=-XX:-HeapDumpOnOutOfMemoryError
ExecStart=/usr/local/bin/teku/bin/teku \
  --network=mainnet \
  --data-path=/var/lib/teku/ \
  --data-storage-mode="minimal" \
  --initial-state="https://beaconstate.info" \
  --ee-endpoint=http://127.0.0.1:8551 \
  --ee-jwt-secret-file=/secrets/jwtsecret \
  --rest-api-enabled=true \
  --rest-api-port=5052 \
  --p2p-port=9000 \
  --p2p-peer-upper-bound=100 \
  --p2p-peer-lower-bound=60 \
  --metrics-enabled=true \
  --metrics-port=8008 \
  --validators-proposer-default-fee-recipient=<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>

[Install]
WantedBy=multi-user.target
```

* Reemplace**`<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>`** con su propia dirección de Ethereum que controle. Los tips se envían a esta dirección y se pueden gastar de inmediato.
* **No haces staking?** Si solo desea un nodo completo, elimine la línea que comienza con 

```
--validators-proposer-default-fee-recipient
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, y luego `Enter`.
{% endtab %}

{% tab title="Combined BN+VC" %}
{% hint style="info" %}
Esta configuración combina la cadena de balizas y el validador en un servicio en ejecución. Si bien es más simple de administrar y ejecutar, esta configuración es menos flexible cuando se trata de ejecutar nodos de conmutación por error EL+CL o en momentos en que desee volver a sincronizar su cliente de ejecución y usar temporalmente el [Rocket Pool's Rescue Node](https://rescuenode.com/docs/how-to-connect/solo).
{% endhint %}

```shell
[Unit]
Description=Teku Beacon Node + Validator Consensus Layer Client service for Mainnet
Wants=network-online.target
After=network-online.target
Documentation=https://www.coincashew.com

[Service]
Type=simple
User=consensus
Group=consensus
Restart=on-failure
RestartSec=3
KillSignal=SIGINT
TimeoutStopSec=900
Environment=JAVA_OPTS=-Xmx6g
Environment=TEKU_OPTS=-XX:-HeapDumpOnOutOfMemoryError
ExecStart=/usr/local/bin/teku/bin/teku \
  --network=mainnet \
  --data-path=/var/lib/teku/ \
  --data-storage-mode="minimal" \
  --initial-state="https://beaconstate.info" \
  --ee-endpoint=http://127.0.0.1:8551 \
  --ee-jwt-secret-file=/secrets/jwtsecret \
  --rest-api-enabled=true \
  --rest-api-port=5052 \
  --p2p-port=9000 \
  --p2p-peer-upper-bound=100 \
  --p2p-peer-lower-bound=60 \
  --metrics-enabled=true \
  --metrics-port=8008 \
  --validator-keys=/var/lib/teku/validator_keys:/var/lib/teku/validator_keys \
  --validators-graffiti="🏠🥩🪙🛡️" \
  --validators-proposer-default-fee-recipient=<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>

[Install]
WantedBy=multi-user.target
```

* Reemplace**`<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>`** con su propia dirección de Ethereum que controle. Los tips se envían a esta dirección y se pueden gastar de inmediato.
* **No haces staking?** Si solo desea un nodo completo, elimine las tres líneas completas que comiencen con

```
--validator-keys
--validators-graffiti
--validators-proposer-default-fee-recipient
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, y luego `Enter`.
{% endtab %}
{% endtabs %}

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```bash
sudo systemctl daemon-reload
sudo systemctl enable consensus
```

Por último, inicie el cliente de la capa de consenso y compruebe su estado.

```bash
sudo systemctl start consensus
sudo systemctl status consensus
```

Presione `Ctrl` + `C` para salir del estado.

Compruebe los registros para confirmar que los clientes de consenso están activos y sincronizados.

```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de registros de cliente de consenso sincronizados **

```bash
teku[64122]: 02:24:28.010 INFO  - Slot Event  *** Slot: 19200, Block: 1468A43F874EDE790DB6B499A51003500B5BA85226E9500A7A187DB9A169DE20, Justified: 1132, Finalized: 1133, Peers: 70
teku[64122]: 02:24:40.010 INFO  - Slot Event  *** Slot: 19200, Block: 72B092AADFE146F5D3F395A720C0AA3B2354B2095E3F10DC18F0E9716D286DCB, Justified: 1132, Finalized: 1133, Peers: 70
```

### 4. Comandos de cliente de consenso útiles

{% tabs %}
{% tab title="View Logs" %}
```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de registros de cliente de consenso de Teku sincronizados**

```bash
teku[64122]: 02:24:28.010 INFO  - Slot Event  *** Slot: 19200, Block: 1468A43F874EDE790DB6B499A51003500B5BA85226E9500A7A187DB9A169DE20, Justified: 1132, Finalized: 1133, Peers: 70
teku[64122]: 02:24:40.010 INFO  - Slot Event  *** Slot: 19200, Block: 72B092AADFE146F5D3F395A720C0AA3B2354B2095E3F10DC18F0E9716D286DCB, Justified: 1132, Finalized: 1133, Peers: 70
```
{% endtab %}

{% tab title="Stop" %}
```bash
sudo systemctl stop consensus
```
{% endtab %}

{% tab title="Start" %}
```bash
sudo systemctl start consensus
```
{% endtab %}

{% tab title="View Status" %}
```bash
sudo systemctl status consensus
```
{% endtab %}

{% tab title="Reset Database" %}
Las razones comunes para restablecer la base de datos pueden incluir:

* Para reducir el uso de espacio en disco
* Para recuperarse de una base de datos dañada debido a un corte de energía o un error de hardware
* Para actualizar a un nuevo formato de almacenamiento 

```bash
sudo systemctl stop consensus
sudo rm -rf /var/lib/teku/beacon
sudo systemctl restart consensus
```

Con la sincronización de puntos de control habilitada, el tiempo para volver a sincronizar el cliente de consenso debería tardar solo uno o dos minutos.
{% endtab %}
{% endtabs %}

Ahora que el cliente de consenso está configurado e iniciado, tiene un nodo completo.

Continúe con el siguiente paso en la configuración de su cliente validador, que convierte un nodo completo en un nodo de participación. 

{% hint style="info" %}
Si querías configurar un nodo completo, no un nodo de staking, ¡detente aquí! ¡Felicidades por ejecutar tu propio nodo completo! :tada:
{% endhint %}
