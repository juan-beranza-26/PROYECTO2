# Actualizando Cliente de Ejecución

## :rocket: Actualizaciones Automatizadas

:pill:**Instalar** [**EthPillar**](../../ethpillar.md): ¡una sencilla interfaz de usuario complementaria para la gestión de nodos!&#x20;

Actualice su software con sólo pulsar una tecla.

Para actualizar, vaya a

`EthPillar > Cliente de ejecución > Actualizar a la última versión`.

<figure><img src=«../../../../.gitbook/assets/el-update.png» alt=«»><figcaption><p>Actualización de EthPillar</p></figcaption></figure>

## :fast_forward: Actualizaciones manuales

Cuando se corta una nueva versión, usted querrá actualizar a la última versión estable. A continuación se muestra cómo actualizar su cliente de ejecución.

{% hint style=«warning» %}
Revise siempre las **notas de lanzamiento** antes de actualizar. Puede haber cambios que requieran su atención.

* [Nethermind](https://github.com/NethermindEth/nethermind/releases)
* [Besu](https://github.com/hyperledger/besu/releases)
* Geth](https://github.com/ethereum/go-ethereum/releases)
* Erigon](https://github.com/ledgerwatch/erigon/releases)
* [Reth](https://github.com/paradigmxyz/reth)
{% endhint %}

{% hint style=«success» %}
:fuego: **Consejo profesional**: Planifique su actualización para que coincida con el intervalo de atestación más largo. [Aprenda cómo aquí.](../../guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-ii-maintenance/finding-the-longest-attestation-slot-gap.md)
{% endhint %}

## Paso 1: Seleccione su cliente de ejecución.

### Nethermind

<detalles>

<summary>Opción 1 - Descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de linux, descomprimir y limpiar.

```bash
RELEASE_URL="https://api.github.com/repos/NethermindEth/nethermind/releases/latest»
BINARIOS_URL="$(curl -s $RELEASE_URL | jq -r “.assets[] | select(.name) | .browser_download_url” | grep linux-x64)»

echo URL de descarga: $BINARIES_URL

cd $HOME
wget -O nethermind.zip $BINARIES_URL
unzip -o nethermind.zip -d $HOME/nethermind
rm nethermind.zip
```

Detener servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop execution
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/nethermind
sudo mv $HOME/nethermind /usr/local/bin/nethermind
sudo systemctl Iniciar Ejecucion
```

</details>

<details>

<summary>Opción 2 - Construir desde el código fuente</summary>

Construye los binarios.

```bash
cd ~/git/nethermind
# Get new tags
git fetch --tags
# Get latest tag name
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Checkout latest tag
git checkout $latestTag
# Build
dotnet publish src/Nethermind/Nethermind.Runner -c release -o nethermind
```

Verifique que Nethermind fue construido apropiadamente revisando la versión.

```shell
./nethermind/nethermind --version
```

Ejemplo de salida de una versión compatible.

```
Version: 1.25.2+78c7bf5f
Commit: 78c7bf5f2c0819f23e248ee6d108c17cd053ffd3
Build Date: 2024-01-23 06:34:53Z
OS: Linux x64
Runtime: .NET 8.0.1
```

Detener Servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop execution
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/nethermind
sudo mv $HOME/git/nethermind/nethermind /usr/local/bin
sudo systemctl start execution
```

</details>

### Besu

<details>

<summary>Option 1 - Download binaries</summary>

Run the following to automatically download the latest linux release, un-tar and cleanup.

```bash
RELEASE_URL="https://api.github.com/repos/hyperledger/besu/releases/latest"
TAG=$(curl -s $RELEASE_URL | jq -r .tag_name)
BINARIES_URL="https://github.com/hyperledger/besu/releases/download/$TAG/besu-$TAG.tar.gz"

echo Downloading URL: $BINARIES_URL

cd $HOME
wget -O besu.tar.gz $BINARIES_URL
tar -xzvf besu.tar.gz -C $HOME
rm besu.tar.gz
sudo mv $HOME/besu-* besu
```

Detener los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop execution
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/besu
sudo mv $HOME/besu /usr/local/bin/besu
sudo systemctl start execution
```

</details>

<details>

<summary>Option 2 - Build from source code</summary>

Construye los binarios.

```bash
cd ~/git/besu
# Get new tags
git fetch --tags
# Get latest tag name
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Checkout latest tag
git checkout $latestTag
# Build
./gradlew installDist
```

Compruebe que Besu se ha creado correctamente verificando la versión.

```shell
./build/install/besu/bin/besu --version
```

Ejemplo de salida de una versión compatible.

```
besu/v23.4.0/linux-x86_64/openjdk-java-17
```

Stop the services.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Elimine los archivos binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/besu
sudo cp -a $HOME/git/besu/build/install/besu /usr/local/bin/besu
sudo systemctl start implementation
```

</details>

### Geth

<details>

<summary>Opción 1: descargar binarios</summary>

<pre class="language-bash"><code class="lang-bash">RELEASE_URL="https://geth.ethereum.org/downloads"
<strong>FILE="https://gethstore.blob.core.windows.net/builds/geth-linux-amd64[a-zA-Z0-9./?=_%:-]*.tar.gz"
</strong>BINARIES_URL="$(curl -s $RELEASE_URL | grep -Eo $FILE | head -1)"

echo URL de descarga: $BINARIES_URL

cd $HOME
wget -O geth.tar.gz $BINARIES_URL
tar -xzvf geth.tar.gz -C $HOME --strip-components=1
</code></pre>

Detener los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Instalar nuevos binarios, reiniciar los servicios y limpiar los archivos.

```bash
sudo mv $HOME/geth /usr/local/bin
sudo systemctl start implementation
rm geth.tar.gz COPYING
```

</details>

<details>

<summary>Opción 2: compilar desde el código fuente</summary>

Compile el binario.

```bash
cd $HOME/git/go-ethereum
# Obtener nuevas etiquetas
git fetch --tags
# Obtener el nombre de la última etiqueta
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Verificar la última etiqueta
git checkout $latestTag
# Generar
make geth
```

Detener los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Eliminar los binarios antiguos, instalar los nuevos y reiniciar los servicios.
```bash
sudo rm -rf /usr/local/bin/geth
sudo cp $HOME/git/go-ethereum/build/bin/geth /usr/local/bin
sudo systemctl start execution
```

</details>

### Erigon

<details>

<summary>Option 1 - Download binaries</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.

<pre class="language-bash"><code class="lang-bash">RELEASE_URL="https://api.github.com/repos/ledgerwatch/erigon/releases/latest"
<strong>BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux_amd64)"
</strong>
echo URL de descarga: $BINARIES_URL

cd $HOME
wget -O erigon.tar.gz $BINARIES_URL
tar -xzvf erigon.tar.gz -C $HOME
rm erigon.tar.gz README.md
</code></pre>

Detenga los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/erigon
sudo mv $HOME/erigon /usr/local/bin/erigon
sudo systemctl start implementation
```

</details>

<details>

<summary>Opción 2: compilar desde el código fuente</summary>

Compile el binario.

```bash
cd $HOME/git/erigon
git fetch --tags
# Obtener el nombre de la última etiqueta
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Obtener la última etiqueta
git checkout $latestTag
make erigon
```

Detenga los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/erigon
sudo cp $HOME/git/erigon/build/bin/erigon /usr/local/bin
sudo systemctl start implementation
```

</details>

### Reth

<details>

<summary>Opción 1: descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.

```bash
RELEASE_URL="https://api.github.com/repos/paradigmxyz/reth/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep x86_64-unknown-linux-gnu.tar.gz$)"

echo URL de descarga: $BINARIES_URL

cd $HOME
wget -O reth.tar.gz $BINARIES_URL
tar -xzvf reth.tar.gz -C $HOME
rm reth.tar.gz
```

Detenga los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos, muestre la versión y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/reth
sudo mv $HOME/reth /usr/local/bin
reth --version
sudo systemctl restart implementation
```

</details>

<details>

<summary>Opción 2: compilar desde el código fuente</summary>

Compile los binarios.
```bash
cd ~/git/reth
git fetch --tags
# Get latest tag name
latestTag=$(git describe --tags `git rev-list --tags --max-count=1`)
# Checkout latest tag
git checkout $latestTag
# Build the release
cargo build --release
```

Verifique que Reth se haya compilado correctamente verificando el número de versión.

```bash
~/git/reth/target/release/reth --version
```

En caso de errores de compilación, ejecute la siguiente secuencia.

```bash
rustup update
cargo clean
cargo build --release
```

Detenga los servicios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl stop implementation
</strong></code></pre>

Elimine los binarios antiguos, instale los nuevos y reinicie los servicios.

```bash
sudo rm -rf /usr/local/bin/reth
sudo cp ~/git/reth/target/release/reth /usr/local/bin
sudo systemctl restart implementation
```

</details>

## Paso 2: Verifique que los servicios y los registros funcionen correctamente

```bash
# Verifique el estado de los servicios
sudo systemctl status implementation
```

```bash
# Verifique los registros
sudo journalctl -fu implementation
```

## Paso 3: Opcional: Verifique las certificaciones de su validador en el explorador de bloques público

1\) Visite [https://holesky.beaconcha.in](https://holesky.beaconcha.in/)

2\) Ingrese la clave pública de su validador en la barra de búsqueda y busque certificaciones exitosas.
