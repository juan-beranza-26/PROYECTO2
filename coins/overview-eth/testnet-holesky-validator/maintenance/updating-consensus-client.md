---
descripcion : >-
  Escenario: En tu discord, tu vez una alerta de los consensos del cliente 
  justamente anunciado ahora, Como seria la mejor actualizacion?
  
---

# Cargando Consensos del cliente

## :rocket: Automatizando Cargas

:pill:**Instalando** [**EthPillar**](../../ethpillar.md): a una simple comapñia de UI para nodos de gestion!&#x20;

Actualice su software con sólo pulsar una tecla.

para actualizar, navega por

`EthPillar > Consensos de cliente > actualizaiones anteriores realizadas`

<figure><img src="../../../../.gitbook/assets/cl-update.png" alt=""><figcaption><p>EthPillar Update</p></figcaption></figure>

## :fast\_forward: Actualizacion Manual

Cuando salga una nueva versión, querrá actualizar a la última versión estable. A continuación se muestra cómo actualizar la cadena de balizas y el validador.

{% hint style="warning" %}
Always review the **release notes** antes de atualizar. ellos pueden tener unos cambios de atencion.

* [Lighthouse](https://github.com/sigp/lighthouse/releases)
* [Lodestar](https://github.com/ChainSafe/lodestar/releases)
* [Teku](https://github.com/ConsenSys/teku/releases)
* [Nimbus](https://github.com/status-im/nimbus-eth2/releases)
* [Prysm](https://github.com/prysmaticlabs/prysm/releases)
{% endhint %}

{% hint style="success" %}
:fire: **Pro tip**: Planifique su actualización para que coincida con el atestado más largo gap. [Learn how here.](../../guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-ii-maintenance/finding-the-longest-attestation-slot-gap.md)
{% endhint %}

## Step 1: Selecion de consensos de cliente.

### Lighthouse

<details>

<summary>Option 1 - Download binaries</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, un-tar and cleanup.

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

Para los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus validator
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

<pre class="language-bash"><code class="lang-bash">sudo rm /usr/local/bin/lighthouse
<strong>sudo mv $HOME/lighthouse /usr/local/bin/lighthouse
</strong><strong>sudo systemctl start consensus validator
</strong></code></pre>

</details>

<details>

<summary>Option 2 - Build from source code</summary>

Build the binaries.

```bash
cd ~/git/lighthouse
git fetch --all && git checkout stable && git pull
make
```

:bulb:**Tip**: ¿Mejorar algunos puntos de referencia de Lighthouse en torno a un 20% a costa de aumentar el tiempo de compilación? Usa `maxperf` perfiles.

* Para copilar con maxperf, sustituya el comando `make` anterior por.

```bash
PROFILE=maxperf make
```

En caso de copilar errores, ve el seguimiento de errores.

```bash
rustup update
cargo clean
make
```

Compruebe que lighthouse se ha creado correctamente verificando el número de versión.

```
lighthouse --version
```

Stop the services.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus validator
</strong></code></pre>

Remove old binaries, install new binaries and restart the services.

<pre class="language-bash"><code class="lang-bash"><strong>sudo rm /usr/local/bin/lighthouse
</strong><strong>sudo cp $HOME/.cargo/bin/lighthouse /usr/local/bin/lighthouse
</strong>sudo systemctl start consensus validator
</code></pre>

</details>

### Lodestar

<details>

<summary>Option 1 - Build from source code</summary>

Obtenga el código fuente más reciente y compile Lodestar.

```bash
cd ~/git/lodestar
git checkout stable && git pull
yarn install
yarn run build
```

:warning: Si se producen errores de compilación o faltan dependencias, ejecute el siguiente comando.

```bash
yarn clean:nm && yarn install
```

Verify Lodestar was installed properly by displaying the version.

```bash
./lodestar --version
```

Sample output of a compatible version.

```
🌟 Lodestar: Implementación en TypeScript de la cadena de balizas de consenso de Ethereum.
  * Version: v1.8.0/stable/a4b29cf
  * by ChainSafe Systems, 2018-2022
```

Detener los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus validator
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.
```bash
sudo rm -rf /usr/local/bin/lodestar
sudo cp -a $HOME/git/lodestar /usr/local/bin/lodestar
sudo systemctl start consensus validator
```

</details>

### Teku

<details>

<summary>Option 1 - Download binaries</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.
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

Parar el servicio.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus
</strong><strong>
</strong># If running Standalone Teku Validator
sudo systemctl stop validator
</code></pre>

Eliminar los binarios antiguos, instalar los nuevos y reiniciar los servicios.

```bash
sudo rm -rf /usr/local/bin/teku
sudo mv $HOME/teku /usr/local/bin/teku
sudo systemctl start consensus

# If running Standalone Teku Validador
sudo systemctl iniciar validador
```

</detalles>

<detalles>

<summary>Option 2 - Build from source code</summary>

Obtén las últimas etiquetas y compila los binarios.

```bash
cd ~/git/teku
# Get new tags
git fetch --tags
RELEASETAG=$(curl -s https://api.github.com/repos/ConsenSys/teku/releases/latest | jq -r .tag_name)
git checkout tags/$RELEASETAG
./gradlew distTar installDist
```

Verifique que Teku fue construido correctamente mostrando la version.

```shell
cd $HOME/git/teku/build/install/teku/bin
./teku --version
```

parar los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus
</strong><strong>
</strong># If running Standalone Teku Validador
sudo systemctl parar validador
</code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/teku
sudo cp -a $HOME/git/teku/build/install/teku /usr/local/bin/teku
sudo systemctl start consenso

# Si ejecuta el validador Teku independiente
sudo systemctl start validador
```

</details>

### Nimbus

<details>

<summary>Option 1 - Download binaries</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.

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

Detener los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus
</strong># If running standalone Nimbus Validator
<strong>sudo systemctl stop validator
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos, limpie y reinicie los servicios.

```bash
sudo rm /usr/local/bin/nimbus_beacon_node
sudo rm /usr/local/bin/nimbus_validator_client
sudo mv nimbus/build/nimbus_beacon_node /usr/local/bin
sudo mv nimbus/build/nimbus_validator_client /usr/local/bin
rm -r nimbus
sudo systemctl start consensus
# If running standalone Nimbus Validator
sudo systemctl start validador
```

Recordatorio: En la configuración combinada CL+VC Nimbus, no habrá servicio systemctl del validador.

</details>

<details>

<summary>Opción 2 - Construir desde el código fuente</summary>

pon el último código fuente y construir el binario.

<pre class="language-bash"><code class="lang-bash">cd ~/git/nimbus-eth2
git checkout stable &#x26;&#x26; git pull
make -j$(nproc) update
<strong>make -j$(nproc) nimbus_beacon_node
</strong>make -j$(nproc) nimbus_validador de cliente
</code></pre>

Verifica que Nimbus fue construido correctamente mostrando la versión.
```bash
cd $HOME/git/nimbus-eth2/build
./nimbus_beacon_node --version
```

Detener servicios

```bash
sudo systemctl stop consenso
# Si se ejecuta el validador Nimbus independiente
sudo systemctl stop validador
```

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios..

```bash
sudo rm /usr/local/bin/nimbus_beacon_node
sudo rm /usr/local/bin/nimbus_validator_client
sudo cp $HOME/git/nimbus-eth2/build/nimbus_beacon_node /usr/local/bin
sudo cp $HOME/git/nimbus-eth2/build/nimbus_validator_client /usr/local/bin
sudo systemctl start consenso
# Si se ejecuta el Validador Nimbus independiente
sudo systemctl start validador
```

Recordatorio: En la configuración combinada CL+VC Nimbus, no habrá servicio systemctl del validador.

</details>

### Prysm

<details>

<summary>Option 1 - Descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente los últimos binarios.

```bash
cd $HOME
prysm_version=$(curl -f -s https://prysmaticlabs.com/releases/latest)
file_beacon=cadena-${prysm_version}-linux-amd64
file_validator=validador-${prysm_version}-linux-amd64
curl -f -L «https://prysmaticlabs.com/releases/${file_beacon}» -o beacon-chain
curl -f -L «https://prysmaticlabs.com/releases/${file_validator}» -o validator
chmod +x baliza-cadena validador
```

Detener servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus validator
</strong></code></pre>

Eliminar los binarios antiguos, instalar los nuevos y reiniciar los servicios.

```bash
sudo rm /usr/local/bin/beacon-chain
sudo rm /usr/local/bin/validator
sudo mv validador beacon-chain /usr/local/bin
sudo systemctl start validador de consenso
```

</details>

<details>

<summary>Option 2 - Build from source code</summary>

Extraer el código fuente más reciente y compilar los binarios.

```bash
cd $HOME/git/prysm
git fetch --tags
RELEASETAG=$(curl -s https://api.github.com/repos/prysmaticlabs/prysm/releases/latest | jq -r .nombre_etiqueta)
git checkout tags/$RELEASETAG
go build -o=./build/beacon-chain ./cmd/beacon-chain
go build -o=./build/validator ./cmd/validator
```

Detener Servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop consensus validator
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm /usr/local/bin/beacon-chain
sudo rm /usr/local/bin/validator
sudo cp $HOME/git/prysm/build/beacon-chain /usr/local/bin
sudo cp $HOME/git/prysm/build/validator /usr/local/bin
sudo systemctl start validador de consenso
```

</details>

## Paso 2: Verificar que los servicios y registros funcionan correctamente

{% tabs %}
{% tab title=«Faro | Prysm | Lodestar | Nimbus | Teku» %}
```bash
# Verificar el estado de los servicios
sudo systemctl status consensus validator
```

```bash
# Check logs
sudo journalctl -fu consenso
```

```bash
sudo journalctl -fu validador
```
{% endtab %}

{% tab title=" Combined BN+VC for Nimbus | Teku" %}
```bash
# Comprobar el estado de los servicios
sudo systemctl status consensus 
```

```bash
# Check logs
sudo journalctl -fu consenso
```
{% endtab %}
{% endtabs %}

## Paso 3: Opcional - Verifique las atestaciones de su validador en el explorador público de bloques

1\) Visita [https://holesky.beaconcha.in](https://holesky.beaconcha.in/)

2\) Introduzca la pubkey de su validador en la barra de búsqueda y busque los certificados correctos.
