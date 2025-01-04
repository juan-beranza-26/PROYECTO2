# Reth

## Descipción General

{% hint style="info" %}
**Reth** - abreviatura de Rust Ethereum, es una implementación de nodo completo de Ethereum que se centra en ser fácil de usar, altamente modular, además de rápida y eficiente.
{% endhint %}

{% hint style="warning" %}
:construction: Reth es la vanguardia de los clientes de Ethereum EL. Como software alfa, espere cambios rápidos y proceda con precaución. :construction:
{% endhint %}

#### Enlaces Oficiales



| Tema          | Enlace                                                                       |
| ------------- | -------------------------------------------------------------------------- |
| Lanamiento    | [https://github.com/paradigmxyz/reth](https://github.com/paradigmxyz/reth) |
| Documentación | [https://paradigmxyz.github.io/reth/](https://paradigmxyz.github.io/reth/) |
| Sitio Web     | [https://www.paradigm.xyz/oss/reth](https://www.paradigm.xyz/oss/reth)     |

### 1. Create service account and data directory

Cree un usuario de servicio para el servicio de ejecución, cree un directorio de datos y asigne propiedad.

```bash
sudo adduser --system --no-create-home --group execution
sudo mkdir -p /var/lib/reth
sudo chown -R execution:execution /var/lib/reth
```

Instalar dependencias.

```bash
sudo apt-get update
sudo apt install -y ccze jq curl
```

### **2. Instalar binarios**

* La descarga de archivos binarios suele ser más rápida y cómoda.
* Construir a partir de código fuente puede ofrecer una mejor compatibilidad y está más alineado con el espíritu de FOSS (software gratuito de código abierto).
<details>

<summary>Option 1 - Download binaries</summary>

```bash
RELEASE_URL="https://api.github.com/repos/paradigmxyz/reth/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep x86_64-unknown-linux-gnu.tar.gz$)"

eco Descargando URL: $BINARIES_URL

cd $HOME
wget -O reth.tar.gz $BINARIES_URL
tar -xzvf reth.tar.gz -C $HOME
rm reth.tar.gz
```

Instale los binarios y muestre la versión.

```bash
sudo mv $HOME/reth /usr/local/bin
reth --version
```

</details>

<details>

<summary>Option 2 - Build from source code</summary>

**Instalar dependencia de óxido**

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Cuando se le solicite, ingrese '1' para continuar con la instalación predeterminada.

Actualice sus variables de entorno.

```bash
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Instalar dependencias de óxido.

```bash
sudo apt-get update
sudo apt install -y git libclang-dev pkg-config build-essential
```

Construye los binarios.

```bash
mkdir -p ~/git
cd ~/git
git clone https://github.com/paradigmxyz/reth.git
cd reth
git fetch --tags
# Get latest tag name
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Checkout latest tag
git checkout $latestTag
# Build the release
cargo build --release
```

En caso de errores de compilación, ejecute la siguiente secuencia.

```bash
rustup update
cargo clean
cargo build --release
```

Verifique que Reth se haya creado correctamente comprobando el número de versión.

```bash
~/git/reth/target/release/reth --version
```

Instalar Binario.

```bash
sudo cp ~/git/reth/target/release/reth /usr/local/bin
```

</details>

### **3. Instalar y configurar systemd**

Cree un **archivo de unidad systemd** para definir su configuración `execution.service`.

```bash
sudo nano /etc/systemd/system/execution.service
```

Pegue la siguiente configuración en el archivo.

<pre class="language-bash"><code class="lang-bash">[Unit]
Description=Reth Execution Layer Client service for Mainnet
Wants=network-online.target
After=network-online.target
Documentation=https://www.coincashew.com

[Service]
Type=simple
User=execution
Group=execution
Restart=on-failure
RestartSec=3
KillSignal=SIGINT
TimeoutStopSec=900
Environment=RUST_LOG=info
ExecStart=/usr/local/bin/reth node \
    --full \
    --chain mainnet \
    --datadir=/var/lib/reth \
    --metrics 127.0.0.1:6060 \
    --port 30303 \
    --http \
    --http.port 8545 \
    --http.api="rpc,eth,web3,net,debug" \
    --log.file.directory=/var/lib/reth/logs \ 
    --max-outbound-peers 25 \
    --max-inbound-peers 25 \
    --authrpc.jwtsecret=/secrets/jwtsecret
   
<strong>[Install]
</strong>WantedBy=multi-user.target
</code></pre>

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

Ejecute lo siguiente para habilitar el inicio automático en el momento del arranque.

```bash
sudo systemctl daemon-reload
sudo systemctl enable execution
```

Finally, start your execution layer client and check it's status.

```bash
sudo systemctl start execution
sudo systemctl status execution
```

Presione `Ctrl` + `C` para salir del estado.

### 4. Comandos útiles del cliente de ejecución

{% tabs %}
{% tab title="View Logs" %}
<pre class="language-bash"><code class="lang-bash"><strong>sudo journalctl -fu execution | ccze
</strong></code></pre>

Un cliente de ejecución **Reth** que funcione correctamente indicará "Bloque agregado a la cadena canónica". Por ejemplo,

```
INFO reth::node::events: Forkchoice updated head_block_hash=2317ae..c41107 safe_block_hash=ab173f..33a21b finalized_block_hash=ab173f..33a21b status=Valid
INFO reth::node::events: Block added to canonical chain number=16000 hash=2317ae..c41107
INFO reth::node::events: Canonical chain committed number=16000 hash=2317ae..c41107 elapsed=12.508272ms
```
{% endtab %}

{% tab title="Stop" %}
```bash
sudo systemctl stop execution
```
{% endtab %}

{% tab title="Start" %}
```bash
sudo systemctl start execution
```
{% endtab %}

{% tab title="View Status" %}
```bash
sudo systemctl status execution
```
{% endtab %}

{% tab title="Reset Database" %}
Common reasons to reset the database can include:

* Recuperación de una base de datos dañada debido a un corte de energía o una falla de hardware.
* Resincronización para reducir el uso de espacio en disco.
* Actualización a una nueva forma de almacenamiento.

```bash
sudo systemctl stop execution
sudo rm -rf /var/lib/reth/*
sudo systemctl restart execution
```

Time to re-sync the execution client can take a few hours up to a day.
{% endtab %}
{% endtabs %}

Ahora que su cliente de ejecución está configurado e iniciado, continúe con el siguiente paso para configurar su cliente de consenso.

{% hint style="warning" %}
Si está revisando los registros y ve alguna advertencia o error, tenga paciencia, ya que normalmente se resolverán una vez que tanto su cliente de ejecución como el de consenso estén completamente sincronizados con la red Ethereum.
{% endhint %}
