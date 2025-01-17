---
descripción: Guía rápida para configurar MEV-boost para su validador ETH.
---

# 💰 Guide | MEV-boost for Ethereum Staking

{% hint style=«info» %}
Los siguientes pasos se alinean con nuestra [guía mainnet](../guide-or-how-to-setup-a-validator-on-eth2-mainnet/). Es posible que tenga que ajustar los nombres de los archivos y las ubicaciones de los directorios donde corresponda. Los conceptos básicos siguen siendo los mismos.
{% endhint %}

## :question:¿Qué es mev-boost?

* Permite a los productores individuales y domésticos acceder al MEV, Valor Máximo Extraíble.
* Permite a los validadores obtener mayores recompensas por bloque.
* Opcional y no necesario para las apuestas ETH.
* Middleware de código abierto gestionado por validadores para acceder a un mercado competitivo de construcción de bloques.
* Construido por Flashbots como una implementación de [proposer-builder separation (PBS)](https://ethresear.ch/t/proposer-block-builder-separation-friendly-fee-market-designs/9725) para Ethereum de prueba de participación (PoS).
* `home-staker (you) >> mevboost >> relay >> builder >> searcher +/- frontrun/sandwich += efficient markets :)`

{% hint style="info" %}
**tldr**: A partir de agosto de 2023, se estima que MEV representa el 24 % de las recompensas de un validador. Otras estimaciones sugieren que puede [aumentar las recompensas de participación en más del 60 %].(https://hackmd.io/@flashbots/mev-in-eth2)
{% endhint %}

<figure><img src="../../../.gitbook/assets/mev24.png" alt=""><figcaption><p>Ganancias estimadas por validador. Fuente: <a href="https://ultrasound.money/">https://ultrasound.money</a></p></figcaption></figure>

## :hammer\_pick: ¿Cómo usar MEV?

{% hint style="warning" %}
**Requisito previo:** Ejecuta un nodo Ethereum completo (cliente de capa de ejecución [p. ej., geth/besu/nethermind/erigon] + cliente de capa de consenso [p. ej., prysm/lighthouse/teku/lodestar/nimbus]) y un validador.
{% endhint %}

### Paso 1: Crea una cuenta de servicio de mevboost

El servicio systemd se ejecutará bajo esta cuenta, `mevboost`

```golpecito
sudo useradd --no-create-home --shell /bin/false mevboost
```

### Paso 2: Instalar mevboost

* Descargar binarios suele ser más rápido y cómodo.&#x20;
* Construir a partir del código fuente puede ofrecer una mejor compatibilidad y está más alineado con el espíritu de FOSS (software libre de código abierto).

<detalles>

<summary>Opción 1: descargar archivos binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.

```bash
URL_DE_RELEASE="https://api.github.com/repos/flashbots/mev-boost/releases/latest"
URL_DE_BINARIOS="$(curl -s $URL_DE_RELEASE | jq -r ".assets[] | select (.name) | .browser_download_url" | grep linux_amd64.tar.gz$)"

echo URL de descarga: $BINARIES_URL

cd $HOME
# Descargar
wget -O mev-boost.tar.gz $BINARIES_URL
# Descomprimir
tar -xzvf mev-boost. tar.gz -C $HOME
# Limpieza
rm mev-boost.tar.gz LICENSE README.md
```

Instala los binarios.

<pre class="language-bash"><code class="lang-bash"><strong>sudo mv $HOME/mev-boost /usr/local/bin
</strong></code></pre>

< /details>

<details>

<summary>Opción 2: compilar desde el código fuente</summary>

Instalar dependencias de compilación.

```bash
sudo apt -y install build-essential
```

Instalar Go y eliminar cualquier instalación anterior de Go.

< pre clase="idioma-bash"><código clase="idioma-bash"><strong>cd $HOME
</strong>wget -O go.tar.gz https://go.dev/dl/go1.19.6 .linux-amd64.tar.gz
sudo rm -rf /usr/local/go &#x26;&#x26; sudo tar -C /usr/local -xzf go.tar.gz
rm go.tar.gz
echo export PATH=$PATH:/usr/local/go/bin >> $HOME/.bashrc
source $HOME/.bashrc
< /code></pre>

Verifique que está instalada la versión Go 1.18+ imprimiendo la información de la versión.

```bash
go version
```

instale mev-boost con `go install`

```bash
CGO_CFLAGS="-O -D__BLST_PORTABLE__" go instalar github.com/flashbots/mev-boost@latest
```

instalar binarios en `/usr/local/bin` y actualizar permisos de propiedad.

```golpecito
sudo cp $HOME/go/bin/mev-boost /usr/local/bin
```

</detalles>

crear el archivo mevboost de la unidad systemd.
```golpecito
sudo nano /etc/systemd/system/mevboost.service
```

La línea `ExecStart` enumera los relés: **Flashbots, UltraSound, Aestus, bloXroute Max Profit, WenMerge**. bastante o añada otros relés según sus preferencias. Agregue tantos o tan pocos relés como desee.
{% sugerencia estilo="info" %}
Encontrar puntos finales de retransmisión en:

* [Lista de retransmisiones MEV](mev-relay-list.md)
* [https://boost.flashbots.net](https://boost.flashbots.net/)
* [https://github.com/remyroy/ethstaker/blob/main/MEV-relay-list.md](https://github.com/remyroy/ethstaker/blob/main/MEV-relay-list.md )

Se pueden especificar varios relés mediante `-relay`.
Example:

```
-relay https://RELAY1.COM \
-relay https://RELAY2.COM \
-relay https://RELAY3.COM

Importante: Asegúrese de que cada línea de relé termine en \, excepto la última línea de relé.
```
{% final %}

Pegue lo siguiente en su archivo `mevboost.service`. Para salir y guardar desde el editor `nano`, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

{% tabs %}
{% tab title="Red principal de Ethereum" %}
```bash
[Unidad]
Descripción=Servicio MEV-Boost para la red principal de Ethereum
Deseos=network-online.target
Después=network-online.target
Documentación=https: //www.coincashew.com

[Servicio]
Usuario=mevboost
Grupo=mevboost
Tipo=simple
Reiniciar=siempre
RestartSec=5
ExecStart=/usr/local/bin/mev-boost \
-mainnet \
-min-bid 0.03 \
-relay- comprobar \
-relé https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net \
-relay https://0xa1559ace749633b997cb3fdacffb890aeebdb0f5a3b6aaa7eeeaf1a38af0a8fe88b9e4b1f61f236d2e64d95733327a62@relay.ultrasonido.dinero \
-relay https://0xa15b52576bcbf1072f4a011c0f99f9fb6c66f3e1ff321f11f461d15e31b1cb359caa092c71bbded0bae5b5ea401aab7e@aestus.live \
-relé https://0x8b5d2e73e2a3a55c6c87b8b6eb92e0149a125c852751db1422fa951e42a09b82c142c3ea98d0d9930b056a3bc9896b8f@bloxroute.max-profit.blxrbdn.com \
-relé https://0x8c7d33605ecef85403f8b7289c8058f440cbb6bf72b055dfe2f3e2c6695b6a1ea5a9cd0eb3a7982927a463feb4c3dae2@relay.wenmerge.com

[Instalar]
WantedBy=multi-user.target
```
{% endtab %}

{% tab title="Red de prueba Holesky" %}
```bash
[Unidad]
Descripción=MEV -Servicio de refuerzo para Holesky
Wants=network-online.target
After=network-online.target
Documentación=https://www.coincashew.com

[Servicio]
Usuario=mevboost
Grupo=mevboost
Tipo=simple
Reiniciar=siempre
RestartSec=5
ExecStart=/usr/local /bin/mev-boost \
-holesky \
-min-bid 0.03 \
-relay-check \
-relay https://0x821f2a65afb70e7f2e820a925a9b4c80a159620582c1766b1b09729fec178b11ea22abb3a51f07b288be815a1a2ff516@bloxroute.holesky.blxrbdn.com \
-relay https://0xb1d229d9c21298a87846c7022ebeef277dfc321fe674fa45312e20b5b6c400bfde9383f801848d7837ed5fc449083a12@relay-holesky.edennetwork.io \
-relay https://0xafa4c6985aa049fb79dd37010438cfebeb0f2bd42b115b89dd678dab0670c1de38da0c4e9138c9290a398ecd9a0b3110@boost-relay-holesky.flashbots.net \
-relay https://0xaa58208899c6105603b74396734a6263cc7d947f444f396a90f7b7d3e65d102aec7e5e5291b27e08d02c50a050825c2f@holesky.titanrelay.xyz \
-relé https://0xab78bf8c781c58078c3beb5710c57940874dd96aef2835e7742c866b4c7c0406754376c2c8285a36c630346aa5c5f833@holesky.aestus.live \
-relé https://0xb1559beef7b5ba3127485bbbb090362d9f497ba64e177ee2c8e7db74746306efad687f2cf8574e38d70067d40ef136dc@relay-stag.ultrasound.money

[Instalar]
WantedBy=multi-user.target
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Usando `-min -bandera `bid`, puede establecer un valor de oferta mínimo en ETH.&#x20;
* Si todos los relés no pueden pujar por encima de su valor mínimo, su cliente de ejecución local producirá el bloque.&#x20;
* Bal establecer este valor, puede capturar oportunidades MEV para bloques de mayor valor y mantener un grado de control para la producción local de bloques que ayuda a fortalecer la resistencia a la censura y una red Ethereum neutral..&#x20;
{% endhint %}

Recarga systemctl para recoger el nuevo archivo de servicio.

```bash
sudo systemctl daemon-reload
```

{% hint style="info" %}
**Es bueno saberlo**: Si agrega o elimina puntos finales de retransmisión, deberá volver a ejecutar este comando systemctl`daemon-reload` y reiniciar los servicios de mevboost.

```bash
sudo systemctl daemon-reload
sudo systemctl restart mevboost
```
{% endhint %}

Habilite mevboost para que se inicie automáticamente al reiniciar el sistema e inicie el servicio.

```bash
sudo systemctl enable mevboost
sudo systemctl start mevboost
```

Verifique que el servicio se haya iniciado correctamente.

```bash
sudo systemctl status mevboost
```

Ejemplo de registros de systemd que muestran que mevboost se ejecuta normalmente.

```
● mevboost.service - red principal de Ethereum de mev-boost
Cargado: cargado (/etc/systemd/system/mevboost.service; habilitado; valor predeterminado del proveedor: habilitado)
Activo: activo (en ejecución) desde el lunes 17 de septiembre de 2022 a las 23:32:00; hace 10 s
PID principal: 12321 (mev-boost)
Tareas: 11 (límite: 34236)
Memoria: 15,4 M
CGroup: /system.slice/mevboost.service
└─12321 /usr/local/bin/mev-boost -mainnet -relay-check -relays https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net,https://0xad0a8bb54565c2211cee576363f3>
```

Ver registros con el siguiente comando:

```bash
sudo journalctl -fu mevboost
```

Ejemplo de registros que muestran que mevboost se está ejecutando normalmente.

```
17 de septiembre 23:32:23 ethstaker systemd[1]: Se inició el relé MEV-Boost.
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: tiempo="2022-09-17T23:32:32-00:00" nivel=info msg="mev-boost v1.3.1" módulo=cli
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: tiempo="2022-09-17T23:32:32-00:00" nivel=info msg="Usando la versión de la bifurcación de Génesis: 0x00000000" módulo=cli
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: tiempo="2022-09-17T23:32:32-00:00" nivel=info msg="usando 2 relés" módulo=cli relés="[{0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net} {0xad0a8bb54565c2211cee576363f3a347089d2f07cf72679d16911d740262694cadb62d7fd7483f27afd714ca0f1b9118 https://0xad0a8bb54565c2211cee576363f3a347089d2f07cf72679d16911d740262694cadb62d7fd7483f27afd714ca0f1b9118@bloxroute.ethical.blxrbdn.com}]"
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: hora="2022-09-17T23:32:32-00:00" nivel=información msg="Comprobando relé" módulo=servicio relé="https://0xac6e77dfe25ecd6110b8e780608cce0dab71fdd5ebea22a16c0205200f2f8e2e3ad3b71d3499c54ad14d6c21b41a37ae@boost-relay.flashbots.net"
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: hora="2022-09-17T23:32:32-00:00" nivel=información msg="Comprobando relé" módulo=servicio relay="https://0xad0a8bb54565c2211cee576363f3a347089d2f07cf72679d16911d740262694cadb62d7fd7483f27afd714ca0f1b9118@bloxroute.ethical.blxrbdn.com"
17 de septiembre 23:32:23 ethstaker mev-boost[12321]: time="2022-09-17T23:32:32-00:00" level=info msg="listening on localhost:18550" module=cli
```

### Paso 3: Actualizar el cliente y el validador de la capa de consenso

{% hint style="info" %}
Tanto el cliente como el validador de la capa de consenso requerirán **API de compilador** adicional banderas.
{% endhint %}

**Cambios en la capa de cliente de consenso (cadena de balizas)**

Agregue la bandera adecuada a la línea `ExecStart` de su archivo de servicio de **cliente** de consenso.&#x20;

Seleccione la pestaña adecuada para su configuración de staking.

Para salir y guardar desde el editor `nano`, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

{% tabs %}
{% tab title="Configuración de staking V2 (actual)" %}
```bash
sudo nano /etc/systemd/system/consensus.service
```
{% endtab %}

{% tab title="Configuración de staking V1" %}
```bash
sudo nano /etc/systemd/system/beacon-chain.service
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Lighthouse" %}
```
--builder http://127.0.0.1:18550
```
{% endtab %}

{% tab title="Teku" %}
#### Opción 1: Configuración del archivo de servicio Systemd - Uso para la configuración de staking de Teku V2

Si su cliente Teku está configurado por --parameters en el **archivo de servicio systemd**, agregue los siguientes cambios.

```bash
--validators-builder-registration-default-enabled=true --builder-endpoint=http://127.0.0.1:18550
```

#### Opción 2: Configuración de TOML - Uso para la configuración de staking de Teku V1

Si su cliente Teku está configurado pasando un **archivo TOML (es decir, teku.yaml),** edite `teku.yaml` con nano.

```bash
sudo nano /etc/teku/teku.yaml
```

Agregue las siguientes líneas al archivo yaml:

```bash
# mevboost
validators-builder-registration-default-enabled: true
builder-endpoint: "http://127.0.0.1:18550"
```

{% hint style="info" %}
¡Use una configuración u otra, pero no ambas!
{% endhint %}
{% endtab %}

{% tab title="Lodestar" %}
```
--builder --builder.urls http://127.0.0.1:18550
```
{% endtab %}

{% tab title="Nimbus" %}
```
--payload-builder=true --payload-builder-url=http://127.0.0.1:18550
```
{% endtab %}

{% tab title="Prysm" %}
```
--http-mev-relay=http://127.0.0.1:18550
```
{% endtab %}
{% endtabs %}

Por ejemplo, este es el resultado esperado de una línea `ExecStart` actualizada de un consenso de Prysm de **V2 Staking Setup** **Archivo de servicio del cliente**.

La bandera se agrega en la última línea.

Al agregar una nueva línea, tenga en cuenta que las líneas anteriores requieren una barra invertida `\`

```bash
ExecStart=/usr/local/bin/beacon-chain \
--mainnet \
--checkpoint-sync-url=https://beaconstate.info \
--genesis-beacon-api-url=https://beaconstate.info \
--execution-endpoint=http://localhost:8551 \
--jwt-secret=/secrets/jwtsecret \
--suggested-fee-recipient=0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS \
--accept-terms-of-use \
--http-mev-relay=http://127.0.0.1:18550
```

**Cambios en el cliente del validador**

Si es necesario, agregue el indicador apropiado a la línea `ExecStart` de su Archivo de servicio **cliente** del validador. Para salir y guardar desde el editor `nano`, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

```bash
sudo nano /etc/systemd/system/validator.service
```

{% tabs %}
{% tab title="Lighthouse" %}
```
--builder-proposals
```
{% endtab %}

{% tab title="Teku" %}
Para Teku que se ejecuta en una configuración de validador independiente,

```bash
--validators-builder-registration-default-enabled=true
```

Para Teku que se ejecuta en una configuración combinada de BN+VC, no se necesita ninguna configuración adicional.
{% endtab %}

{% tab title="Lodestar" %}
```
--builder
```
{% endtab %}

{% tab title="Nimbus" %}
Para Nimbus que se ejecuta en una configuración de validador independiente,

```
--payload-builder=true
```

Para Nimbus que se ejecuta en una configuración combinada de BN+VC, no se necesita ninguna configuración adicional.
{% endtab %}

{% tab title="Prysm" %}
```
--enable-builder
```
{% endtab %}
{% endtabs %}

Por ejemplo, aquí está el resultado esperado de una línea `ExecStart` actualizada de un archivo de servicio del cliente de validación de Prysm de **V2 Staking Setup**.&#x20;

Se agrega una bandera en la última línea.

Al agregar una nueva línea, tenga en cuenta que las líneas anteriores requieren una barra invertida `\`

```bash
ExecStart=/usr/local/bin/validator \
--mainnet \
--accept-terms-of-use \
--datadir=/var/lib/prysm/validators \
--beacon-rpc-provider=localhost:4000 \
--wallet-dir=/var/lib/prysm/validators \
--wallet-password-file=/var/lib/prysm/validators/password.txt \
--graffiti "" \
--suggested-fee-recipient=<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS> \
--enable-builder
```

Después de configurar su cliente de consenso y validador para habilitar mevboost, vuelva a cargar y reinicie sus servicios. Por último, verifique que sus registros estén libres de errores y muestren el uso de las nuevas configuraciones de MEV.
{% tabs %}
{% tab title="V2 Staking Setup (Current)" %}
**Lighthouse, Lodestar, Prysm**

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong>sudo systemctl restart consensuado validador

sudo journalctl -fu consensuado
sudo journalctl -fu consensuado
</code></pre>

**Teku o Nimbus**

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong>sudo systemctl restart consensuado

sudo journalctl -fu consensuado
</code></pre>
{% endtab %}

{% tab title="Configuración de staking V1" %}
**Lighthouse, Lodestar, Prysm**

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong>sudo systemctl restart Validador de beacon-chain

sudo journalctl -fu beacon-chain
sudo journalctl -fu validator
</code></pre>

**Teku o Nimbus**

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong>sudo systemctl restart beacon-chain

sudo journalctl -fu beacon-chain
</code></pre>
{% endtab %}
{% endtabs %}

{% hint style="success" %}
¡Felicitaciones! Su validador con mev-boost obtendrá más recompensas al proponer un bloque.
{% endhint %}

## :dart: Cómo actualizar MEV-boost

Actualice a la última versión con los siguientes comandos.

[Revise las últimas notas de la versión de MEV-boost](https://github.com/flashbots/mev-boost/releases) para conocer los nuevos requisitos o los cambios importantes.

<details>

<summary>Opción 1: descargar binarios</summary>

Ejecute lo siguiente para descargar automáticamente la última versión de Linux, descomprimir y limpiar.

```bash
RELEASE_URL="https://api.github.com/repos/flashbots/mev-boost/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux_amd64.tar.gz$)"

echo URL de descarga: $BINARIES_URL

cd $HOME
# Descargar
wget -O mev-boost.tar.gz $BINARIES_URL
# Descomprimir
tar -xzvf mev-boost.tar.gz -C $HOME
# Limpieza
rm mev-boost.tar.gz LICENCIA README.md
```

Detenga el servicio, instale los binarios e inicie el servicio.

```bash
sudo systemctl stop mevboost
sudo mv $HOME/mev-boost /usr/local/bin
sudo systemctl start mevboost
```

Verifique el nuevo número de versión.
```bash
/usr/local/bin/mev-boost --version
```

</details>

<details>

<summary>Opción 2: compilar desde el código fuente</summary>

Compilar nuevos binarios y detener el servicio.

```bash
CGO_CFLAGS="-O -D__BLST_PORTABLE__" go install github.com/flashbots/mev-boost@latest
sudo systemctl stop mevboost
```

Instalar nuevos binarios e iniciar el servicio.

```bash
sudo cp ~/go/bin/mev-boost /usr/local/bin
sudo systemctl start mevboost
```

Verificar el nuevo número de versión.

```bash
/usr/local/bin/mev-boost --version
```

</details>

## :wastebasket: Desinstalación de MEV-boost

```bash
sudo systemctl stop mevboost
sudo systemctl deshabilitar mevboost
sudo rm /etc/systemd/system/mevboost.service
sudo rm /usr/local/bin/mev-boost
sudo userdel mevboost
```

Por último, elimine los cambios de la API de Builder realizados en el paso 3 en su cliente de consenso y validador.

## :question: Preguntas frecuentes

<details>

<summary>¿Cómo verifico que estoy registrado en mis relés?</summary>

Verifique que su validador esté registrado en un relé en particular haciendo una solicitud a la API del relé.

Puede consultar manualmente la API del relé o usar el [script de dabauxi para verificar el registro del relé MEV-Boost](https://github.com/dabauxi/check-mevboost-registration).

### Verificar el registro del relé MEV-Boost por dabauxi&#x20;

Revise las notas y el código fuente [aquí](https://github.com/dabauxi/check-mevboost-registration). Requiere Python.

```
# Instalar Python
sudo apt install python3

# Descargar el script
wget https://raw.githubusercontent.com/dabauxi/check-mevboost-registration/main/check_mevboost_registration.py

# Asignar permisos de ejecución
chmod +x check_mevboost_registration.py

# Verificar el registro de mevboost
./check_mevboost_registration.py <your-validator-address>
```

Ejemplo de salida que muestra el registro de su validador en los relés.

```
./check_mevboost_registration.py 0x8000a44457e18388c5be046e22e86aedae1a07638394df63adfcd32d29b4e86c030219e94782ebebe398c9a05a8a28e7

Validador '0x8000a44457e18388c5be046e22e86aedae1a07638394df63adfcd32d29b4e86c030219e94782ebebe398c9a05a8a28e7'
Relé: 'bloxroute.ethical.blxrbdn.com', ❌ no encontrado
Relé: 'relay.edennetwork.io', ❌ no encontrado
Relay: 'builder-relay-mainnet.blocknative.com', ✔️ registrado
Relay: 'bloxroute.max-profit.blxrbdn.com', ✔️ registrado
Relay: 'boost-relay.flashbots.net', ✔️ registrado
Relay: 'bloxroute.regulated.blxrbdn.com', ❌ no encontrado
Relay: 'builder-relay-mainnet.blocknative.com', ✔️ registrado
Relay: 'relay.edennetwork.io', ❌ no encontrado
Relay: 'mainnet-relay.securerpc.com', ✔️ registrado
Relay: 'relayooor.wtf', ✔️ registrado
Relay: 'relay.ultrasound.money', ✔️ registrado
Relay: 'agnostic-relay.net', ✔️ registrado
```

### &#x20;Verificar manualmente

Por ejemplo, para verificar que su validador esté registrado con el relé flashbots, ingrese la siguiente URL en su navegador. Reemplace `<myPubKey>` con la clave pública de su validador y verá datos de registro como la dirección del destinatario de su tarifa.

```
https://boost-relay.flashbots.net/relay/v1/data/validator_registration?pubkey=<myPubKey>
```

Ejemplo de comando:

```
https://boost-relay.flashbots.net/relay/v1/data/validator_registration?pubkey=0xb510871a4600b184e83b1ca28402e4de31b5db968f28196419ab64c6e4e2b39920815a61b0bdfe8c928ae8a4db308517
```

Ejemplo salida:

```
{"mensaje":{"destinatario_de_tarifa": "0xebec795c9c8bbd61ffc14a6662944748f299cacf", "límite_de_gas": "30000000", "marca_de_tiempo": "1663454829", "clave pública": "0xb510871a4600b184e83b1ca28402e4de31b5db968f28196419ab64c6e4e2b39920815a61b0bdfe8c928ae8a4db308517"},"si gnature": "0xaeaffeb2f67f378fc8e0e31929a958bf51895d64e93246372e6bb8609c15b3d64b4ad56a5454bc3ac1be0bc57dce031c12c066c973125312d7e4c5020509edd0aaf98a6a190081305723a89e3dcd7b3f6b1ca40b92bb1a50e5714c28407e1bf9"}
```

</details>

<details>

<summary>Dónde ¿Recibiré pagos de MEV-boost?</summary>

Cuando se produce un bloque utilizando MEV-boost, puede recibir su pago de una de tres maneras.

Específicamente, su pago de MEV puede llegar como:

1\) al establecerlo como el Destinatario de la Tarifa del bloque

2\) una transacción interna

3\) o una transacción normal

</details>

<details>

<summary>¿Por qué ejecutar MEV-boost?</summary>

Consulte [este artículo de Stephane Gosslin](https://writings.flashbots.net/writings/why-run-mevboost), que explica los beneficios de MEV-boost para la red y para usted, como validador.

</details>

<details>

<summary>¿Cómo funciona MEV-boost?</summary>

* Los participantes de Ethereum deben ejecutar tres piezas de software: un cliente de validación, un cliente de consenso y un cliente de ejecución.
* MEV-boost es una pieza independiente de software de código abierto, que consulta y subcontrata la creación de bloques a una red de constructores.
* Los constructores de bloques preparan bloques completos, optimizando la extracción de MEV y la distribución justa de las recompensas.
* Luego envían sus bloques a los relés.
<!---->

* Los relés agregan bloques de **varios** constructores para seleccionar el bloque con las tarifas más altas.
* Un validador puede configurar una instancia de MEV-boost para conectarse a **varios** relés.
* El cliente de la capa de consenso de un validador propone el bloque más rentable recibido de MEV-boost a la red Ethereum para su certificación e inclusión en bloque.

<img src="../../../.gitbook/assets/mev-boost-integration-overview.png" alt="" data-size="original">

</details>

<details>

<summary>¿Cuáles son los riesgos de ejecutar MEV-boost?</summary>

* Agregar más relés aumenta el riesgo de agregar un relé "malo" (pirateado, retiene la oferta, problemas de rendimiento) y hace que su validador pierda una propuesta.
* Más relés = más posibilidades de obtener un bloque con una oferta alta; sin embargo, esto también aumenta las posibilidades de que los relés "malos" lo ataquen y de que pierda una propuesta. Requiere confianza en que los relés y los creadores de bloques actuarán honestamente. MEV aún no es un proceso sin confianza hasta que exista una separación entre proponente y creador a nivel de protocolo (PBS).

Explicación detallada: [https://writings.flashbots.net/writings/understanding-mev-boost-liveness-risks](https://writings.flashbots.net/writings/understanding-mev-boost-liveness-risks/)

Resumen de riesgos: solo agregue relés en los que confíe.

</details>

<details>

<summary>¿Cuál es el estado en tiempo real de MEV?</summary>

Haga un seguimiento de la participación en la red, los bloques MEV recientes, los principales relés y los constructores de bloques en [https://www.mevboost.org](https://www.mevboost.org/)

</details>

<details>

<summary>Estoy usando varios relés. ¿Cuál es el elegido?</summary>

Si hay varios relés disponibles, se elegirá el relé que ofrezca la recompensa MEV más alta. Si no están disponibles todos los relés, el cliente de ejecución local construye el bloque sin MEV.

</details>

<details>

<summary>¿Qué hace que un relé MEV sea ético o no?</summary>

En función de los distintos grados de beneficio o censura, los relés MEV pueden decidir qué transacciones agrupar en un bloque.

* Relés éticos: no censurarán las transacciones ni se beneficiarán de los ataques front running o sandwich, que son perjudiciales para los usuarios cotidianos de Ethereum. * Relés OFAC: censurarán las transacciones según la lista OFAC.
* Relés de máxima ganancia: la ganancia es todo lo que importa, la ética no tiene importancia.

</details>

<details>

<summary>¿Necesito abrir algún puerto de entrada en el firewall?</summary>

No se necesitan cambios. mevboost solo realiza llamadas TCP salientes.

</details>

## :track\_next: Próximos pasos

* :moneybag: **Suavizado MEV:** ¡Gana recompensas de manera constante! Comparte potencialmente bloques de lotería. Calcula el promedio de tus recompensas MEV.
* **Smoothly -** [**https://docs.smoothly.money/how-to-guide**](https://docs.smoothly.money/how-to-guide)
* **Smooth de Dappnode -** [**https://smooth.dappnode.io/how-to**](https://smooth.dappnode.io/how-to)
* :new: **Manténgase actualizado**: Suscríbase al [repositorio mev-boost de flashbot](https://github.com/flashbots/mev-boost/releases) para recibir notificaciones sobre nuevos lanzamientos. Presione el botón Notificaciones.
* :telephone\_receiver: **Manténgase en contacto**: Siga a los [colaboradores de Twitter de MEV-Boost](https://github.com/thegostep/awesome-mev-boost#twitter)
* :rocket: **Ideas futuras**: Conozca el futuro de MEV democratizado por [PBS](https://members.delphidigital.io/reports/the-hitchhikers-guide-to-ethereum/).
* ​:confetti\_ball: **Apóyanos en Gitcoin Grants:** ¡Creamos esta guía exclusivamente con el apoyo de la comunidad!🙏

## :books: Referencias

* [https://github.com/remyroy/ethstaker/blob/main/prepare-for-the-merge.md#choosing-and-configuring-an-mev-solution](https://github.com/remyroy/ethstaker/blob/main/prepare-for-the-merge.md#choosing-and-configuring-an-mev-solution)
* [https://github.com/flashbots/mev-boost/wiki/Testing](https://github.com/flashbots/mev-boost/wiki/Testing)
* [https://boost.flashbots.net/](https://boost.flashbots.net/)
* [https://github.com/flashbots/mev-boost/blob/main/README.md](https://github.com/flashbots/mev-boost/blob/main/README.md)

## :thumbsup: Créditos

Inspirado en la [Guía de Remyyroy sobre cómo prepararse para la fusión](https://github.com/remyroy/ethstaker/blob/main/prepare-for-the-merge.md#choosing-and-configuring-an-mev-solution)
