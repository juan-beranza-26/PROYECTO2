# Lighthouse

## Overview

{% hint style="info" %}
[Lighthouse](https://github.com/sigp/lighthouse)  es un cliente de Eth con un gran enfoque en la velocidad y la seguridad. El equipo detrás de él, [Sigma Prime](https://sigmaprime.io), es una empresa de seguridad de la información e ingeniería de software que ha financiado Lighthouse junto con la Fundación Ethereum, Consensys 
y particulares. Lighthouse está desarrollado en Rust y se ofrece bajo una licencia Apache 2.0.
{% endhint %}

#### Official Links

| Sujeto       | Enlaces                                                                                      |
| ------------- | ------------------------------------------------------------------------------------------ |
| Lanzamientos      | [https://github.com/sigp/lighthouse/releases](https://github.com/sigp/lighthouse/releases) |
| Documentación | [https://lighthouse-book.sigmaprime.io](https://lighthouse-book.sigmaprime.io/)            |
| Sitio web       | [https://lighthouse.sigmaprime.io](https://lighthouse.sigmaprime.io/)                      |

### 1. Initial configuration

Cree un usuario de servicio para el servicio de consenso, cree un directorio de datos y asigne 
propiedad.

```bash
sudo adduser --system --no-create-home --group consensus
sudo mkdir -p /var/lib/lighthouse
sudo chown -R consensus:consensus /var/lib/lighthouse
```

Instalar dependencias.

```bash
sudo apt install curl jq ccze -y
```

### 2. Install Binaries

* Descargar binarios suele ser más rápido y cómodo.&#x20;
* Desarrollar a partir del código fuente puede ofrecer una mejor compatibilidad y está más alineado con el espíritu del FOSS (software gratuito de código abierto).

<details>

<summary>Opción 1 - Descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, un-tar y cleanup.

```bash
RELEASE_URL="https://api.github.com/repos/sigp/lighthouse/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep x86_64-unknown-linux-gnu.tar.gz$)"

echo Downloading URL: $BINARIES_URL

cd $HOME
# Download
wget -O lighthouse.tar.gz $BINARIES_URL
# Untar
tar -xzvf lighthouse.tar.gz -C $HOME
# Cleanup
rm lighthouse.tar.gz
```

Instale los archivos binarios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo mv $HOME/lighthouse /usr/local/bin/lighthouse
</strong></code></pre>

</details>

<details>

<summary>Opción 2 - Construir desde el código fuente</summary>

**Instalar la dependencia de óxido**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Cuando se le solicite, ingrese '1' para continuar con la instalación predeterminada.

Actualice las variables de entorno.

```bash
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Instala dependencias de óxido.

```bash
sudo apt-get update
sudo apt install -y git gcc g++ make cmake pkg-config libssl-dev libclang-dev clang protobuf-compiler
```

Compile los archivos binarios.

```bash
mkdir -p ~/git
cd ~/git
git clone -b stable https://github.com/sigp/lighthouse.git
cd lighthouse
make
```

En caso de errores de compilación, ejecute la siguiente secuencia..

```bash
rustup update
cargo clean
make
```

Verifique que lighthouse se haya construido correctamente verificando el número de versión.

```
lighthouse --version
```

Instale los archivos binarios.

```bash
sudo cp $HOME/.cargo/bin/lighthouse /usr/local/bin/lighthouse
```

</details>

### **3. Setup and configure systemd**

Cree un**archivo de unidad systemd** para definir su `consensus.service` configuración.

```bash
sudo nano /etc/systemd/system/consensus.service
```

Pegue la siguiente configuración en el archivo.

```shell
[Unit]
Description=Lighthouse Consensus Layer Client service for Mainnet
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
ExecStart=/usr/local/bin/lighthouse bn \
  --datadir /var/lib/lighthouse \
  --network mainnet \
  --staking \
  --validator-monitor-auto \
  --metrics \
  --checkpoint-sync-url=https://beaconstate.info \
  --port 9000 \
  --quic-port 9001 \
  --http-port 5052 \
  --target-peers 100 \
  --metrics-port 8008 \
  --execution-endpoint http://127.0.0.1:8551 \
  --execution-jwt /secrets/jwtsecret

[Install]
WantedBy=multi-user.target
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, y luego `Enter`.

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```bash
sudo systemctl daemon-reload
sudo systemctl enable consensus
```

Por último, inicie su cliente de capa de consenso y verifique su estado.

```bash
sudo systemctl start consensus
sudo systemctl status consensus
```

Presione `Ctrl` + `C` para salir del estado.

Revise sus registros para confirmar que el cliente de consenso esté activo y sincronizado.

```bash
sudo journalctl -fu consensus | ccze
```

Presione `Ctrl` + `C` para salir de los registros.

### 4. Helpful consensus client commands

{% tabs %}
{% tab title="View Logs" %}
```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de registros de clientes de consenso de Lighthouse sincronizados**

```bash
Feb 03 01:02:36.000 INFO New block received                      root: 0xb5ccb2f85d981ca9e1c0d904f967403ddf8c47532c195fe213c94a28ffaf6a2e, slot: 2138
Feb 03 01:02:42.000 INFO Synced                                  slot: 2138, block: 0x1cb281a, epoch: 121, finalized_epoch: 120, finalized_root: 0x1dce0, exec_hash: 0x6827aeb (verified), peers: 50, service: slot_notifier
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

* Para reducir el uso del espacio en disco
* Para recuperarse de una base de datos dañada debido a un corte de energía o una falla de hardware
* Para actualizar a un nuevo formato de almacenamiento

```bash
sudo systemctl stop consensus
sudo rm -rf /var/lib/lighthouse/beacon
sudo systemctl restart consensus
```

Con la sincronización de puntos de control habilitada, el tiempo necesario para volver a sincronizar el cliente de consenso debería tomar solo uno o dos minutos
{% endtab %}
{% endtabs %}

Ahora que su cliente de consenso está configurado e iniciado, tiene un nodo completo.

Continúe con el siguiente paso para configurar su cliente validador, que convierte un nodo completo en un nodo de staking.

{% hint style="info" %}
Si querías configurar un nodo completo, no un nodo de participación, ¡detente aquí! ¡Felicitaciones por ejecutar tu propio nodo completo! :tada:
{% endhint %}

