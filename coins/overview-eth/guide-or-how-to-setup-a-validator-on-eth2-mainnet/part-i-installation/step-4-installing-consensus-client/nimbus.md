# Nimbus

## Overview

{% hint style="info" %}
[Nimbus](https://our.status.im/tag/nimbus/) es un proyecto de investigación y una implementación de cliente para Ethereum diseñado para funcionar bien en sistemas integrados y dispositivos móviles personales, incluidos teléfonos inteligentes más antiguos con hardware restringido por recursos. El equipo de Nimbus es de [Estado](https://status.im/about/) la empresa más conocida por [su aplicación de mensajería/billetera/navegador Web3](https://status.im)  con el mismo nombre. Nimbus (Apache 2)  está escrito en Nim, un lenguaje con sintaxis similar a Python que se compila en C.
{% endhint %}

{% hint style="info" %}
**Nota**: Nimbus está configurado para ejecutar ambos **cliente validador** y ** cliente de cadena beacon** en un proceso.
{% endhint %}

#### Official Links

| Asunto       | Enlaces                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------------------ |
| Lanzamientos      | [https://github.com/status-im/nimbus-eth2/releases](https://github.com/status-im/nimbus-eth2/releases) |
| Documentación | [https://nimbus.guide](https://nimbus.guide/)                                                          |
| Sitio web       | [https://our.status.im/tag/nimbus](https://our.status.im/tag/nimbus/)                                  |

### 1. Initial configuration

Cree un usuario de servicio para el servicio de consenso, cree un directorio de datos y asigne la propiedad.

```bash
sudo adduser --system --no-create-home --group consensus
sudo mkdir -p /var/lib/nimbus
sudo chown -R consensus:consensus /var/lib/nimbus
```

Instalar dependencias.

```bash
sudo apt install curl libsnappy-dev libc6-dev jq libc6 unzip ccze -y
```

### 2. Install Binaries

* Descargar binarios es a menudo más rápido y más conveniente.
* Construir a partir del código fuente puede ofrecer una mejor compatibilidad y está más alineado con el espíritu de FOSS (software libre de código abierto).

<details>

<summary>Option 1 - Descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, un-tar y cleanup.

```bash
RELEASE_URL="https://api.github.com/repos/status-im/nimbus-eth2/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep _Linux_amd64.*.tar.gz$)"

echo Downloading URL: $BINARIES_URL

cd $HOME
# Download
wget -O nimbus.tar.gz $BINARIES_URL
# Untar
tar -xzvf nimbus.tar.gz -C $HOME
# Rename folder
mv nimbus-eth2_Linux_amd64_* nimbus
# Cleanup
rm nimbus.tar.gz
```

Instale los binarios, la versión de visualización y la limpieza.

<pre class="language-bash"><code class="lang-bash"><strong>sudo mv nimbus/build/nimbus_beacon_node /usr/local/bin
</strong>sudo mv nimbus/build/nimbus_validator_client /usr/local/bin
nimbus_beacon_node --version
rm -r nimbus
</code></pre>

</details>

<details>

<summary>Option 2 - Construir a partir del código fuente</summary>

Instale dependencias.

```bash
sudo apt-get update
sudo apt-get install curl build-essential git -y
```

Compile el binario.

```bash
mkdir -p ~/git
cd ~/git
git clone -b stable https://github.com/status-im/nimbus-eth2
cd nimbus-eth2
make -j$(nproc) update
make -j$(nproc) nimbus_beacon_node
make -j$(nproc) nimbus_validator_client
```

Verifique que Nimbus se construyó correctamente mostrando la versión.

```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --version
```

Instale los archivos binario.

<pre class="language-bash"><code class="lang-bash"><strong>sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/local/bin
</strong><strong>sudo cp $HOME/git/nimbus-eth2/build/nimbus_validator_client /usr/local/bin
</strong></code></pre>

</details>

### **3. Setup and configure systemd**

Crear un **archivo de unidad systemd** para definir tu `consensus.service` configuración.

```bash
sudo nano /etc/systemd/system/consensus.service
```

Pegue la siguiente configuración en el archivo.

{% tabs %}
{% tab title="Standalone Beacon Node (Recommended)" %}
```shell
[Unit]
Description=Nimbus Consensus Layer Client service for Mainnet
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
ExecStart=/usr/local/bin/nimbus_beacon_node \
  --network=mainnet \
  --data-dir=/var/lib/nimbus \
  --tcp-port=9000 \
  --udp-port=9000 \
  --max-peers=100 \
  --rest-port=5052 \
  --enr-auto-update=true \
  --non-interactive \
  --status-bar=false \
  --in-process-validators=false \
  --web3-url=http://127.0.0.1:8551 \
  --rest \
  --metrics \
  --metrics-port=8008 \
  --jwt-secret="/secrets/jwtsecret" \
  --suggested-fee-recipient=<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>

[Install]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="Combined (BN+VC)" %}
{% hint style="info" %}
Esta configuración 
combina la cadena de baliza y el validador en un solo servicio en ejecución. Si bien es más fácil de administrar y ejecutar, esta configuración es menos flexible cuando se trata de ejecutar nodos de conmutación por error de EL+CL  o en momentos en que desea volver a sincronizar su cliente de ejecución y usarlo temporalmente [Nodos de rescate de Rocket Pool](https://rescuenode.com/docs/how-to-connect/solo).
{% endhint %}

```shell
[Unit]
Description=Nimbus Consensus Layer Client service for Mainnet
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
ExecStart=/usr/local/bin/nimbus_beacon_node \
  --network=mainnet \
  --data-dir=/var/lib/nimbus \
  --tcp-port=9000 \
  --udp-port=9000 \
  --max-peers=100 \
  --rest-port=5052 \
  --enr-auto-update=true \
  --web3-url=http://127.0.0.1:8551 \
  --rest \
  --metrics \
  --metrics-port=8008 \
  --jwt-secret="/secrets/jwtsecret" \
  --graffiti="🏠🥩🪙🛡️!" \
  --suggested-fee-recipient=<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>

[Install]
WantedBy=multi-user.target
```
{% endtab %}
{% endtabs %}

* Reemplazar `<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>` con tu propia dirección Ethereum que controlas. Las propinas se envían a esta dirección y son inmediatamente gastables
* **Not staking?** Si solo desea un nodo completo, use la configuración de Nodo de Beacon Independiente y elimine toda la línea comenzando con

```
--suggested-fee-recipient
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, y luego `Enter`.

Ejecute lo siguiente para sincronizar rápidamente con Checkpoint Sync.

{% hint style="info" %}
La sincronización de puntos de control le permite iniciar su capa de consenso en cuestión de minutos en lugar de días.
{% endhint %}

```bash
sudo -u consensus /usr/local/bin/nimbus_beacon_node trustedNodeSync \
--network=mainnet \
--trusted-node-url=https://beaconstate.info \
--data-dir=/var/lib/nimbus \
--backfill=false
```

Cuando se complete la sincronización del punto de control, verá el siguiente mensaje:

> Hecho, tu nodo de baliza está listo para servirte! No olvide verificar que está en la cadena canónica comparando la raíz del punto de control con otras fuentes en línea. Ver https://nimbus.guide/trusted-node-sync.html para más información.

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```bash
sudo systemctl daemon-reload
sudo systemctl enable consensus
```

Finalmente, inicie su cliente de capa de consenso y verifique su estado.

```bash
sudo systemctl start consensus
sudo systemctl status consensus
```

Presione `Ctrl` + `C` para salir del estado.

Verifique sus registros para confirmar que los clientes de consenso están actualizados y sincronizados.

```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de Registros de Clientes de Consenso Sincronizado**

```
nimbus_beacon_node[292966]: INF 2023-02-05 01:20:00.000+00:00 Slot start       topics="beacnde" slot=31205 epoch=903 sync=synced peers=80 head=13a131:31204 finalized=1111:cdba33411 delay=69us850ns
nimbus_beacon_node[292966]: INF 2023-02-05 01:20:08.000+00:00 Slot end         topics="beacnde" slot=31205 nextActionWait=7m27s985ms126us530ns nextAttestationSlot=31235 nextProposalSlot=-1 syncCommitteeDuties=none head=13a131:31204
```

### 4. Helpful consensus client commands

{% tabs %}
{% tab title="View Logs" %}
```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de Registros de Clientes de Consenso de Nimbus Sincronizados**

```bash
nimbus_beacon_node[292966]: INF 2023-02-05 01:20:00.000+00:00 Slot start       topics="beacnde" slot=31205 epoch=903 sync=synced peers=80 head=13a131:31204 finalized=1111:cdba33411 delay=69us850ns
nimbus_beacon_node[292966]: INF 2023-02-05 01:20:08.000+00:00 Slot end         topics="beacnde" slot=31205 nextActionWait=7m27s985ms126us530ns nextAttestationSlot=31235 nextProposalSlot=-1 syncCommitteeDuties=none head=13a131:31204
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
* Para recuperarse de una base de datos dañada debido a un corte de energía o falla de hardware
* Para actualizar a un nuevo formato de almacenamiento

```bash
sudo systemctl stop consensus
sudo rm -rf /var/lib/nimbus/db

#Perform checkpoint sync
sudo -u consensus /usr/local/bin/nimbus_beacon_node trustedNodeSync \
--network=mainnet \
--trusted-node-url=https://beaconstate.info \
--data-dir=/var/lib/nimbus \
--backfill=false

sudo systemctl restart consensus
```

Con la sincronización del punto de control, el tiempo para volver a sincronizar el cliente de consenso debe tomar solo un minuto o dos.
{% endtab %}
{% endtabs %}

Ahora que su cliente de consenso está configurado e iniciado, tiene un nodo completo.

Continúe con el siguiente paso para configurar su cliente validador, que convierte un nodo completo en un nodo de replanteo.

{% hint style="info" %}
Si desea configurar un nodo completo, no un nodo de replanteo, ¡deténgase aquí! ¡Felicidades por ejecutar tu propio nodo completo! :tada:
{% endhint %}
