# Lodestar

## Overview

{% hint style="info" %}
[Lodestar ](https://lodestar.chainsafe.io)es una implementación de Typescript de la especificación oficial de Ethereum por parte del equipo de[ChainSafe.io](https://lodestar.chainsafe.io). Además del cliente de cadena Beacon, el equipo también está trabajando en 22 paquetes y bibliotecas. Puede encontrar una lista completa [aqui](https://hackmd.io/CcsWTnvRS\_eiLUajr3gi9g). Finalmente, el equipo de Lodestar es líder en la investigación y el desarrollo de clientes ligeros y ha recibido financiación de EF y Moloch DAO para este propósito.
{% endhint %}

#### Official Links

| Sujeto        | Enlaces                                                                                            |
| ------------- | ------------------------------------------------------------------------------------------------ |
| Lanzamientos  | [https://github.com/ChainSafe/lodestar/releases](https://github.com/ChainSafe/lodestar/releases) |
| Documentación | [https://chainsafe.github.io/lodestar](https://chainsafe.github.io/lodestar/)                    |
| Sirio web     | [https://lodestar.chainsafe.io](https://lodestar.chainsafe.io/)                                  |

### 1. Initial configuration

Cree un usuario de servicio para el servicio de consenso, cree un directorio de datos y asigne propiedad.

```bash
sudo adduser --system --no-create-home --group consensus
sudo mkdir -p /var/lib/lodestar
sudo chown -R consensus:consensus /var/lib/lodestar
```

Instalar dependencias.

```bash
sudo apt-get install gcc g++ make git curl ccze -y
```

### 2. Install Binaries

* Desarrollar a partir del código fuente puede ofrecer una mejor compatibilidad y está más alineado con el espíritu del FOSS (software gratuito de código abierto).

<details>

<summary>Opción 1 - Construir desde el código fuente</summary>

Instala el hilo.

```bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update
sudo apt install yarn -y
```

Confirme que el hilo esté instalado correctamente.

```bash
yarn --version
# Should output version >= 1.22.19
```

Instalar nodejs.

```bash
#Download and import the Nodesource GPG key
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg

#Create deb repository
NODE_MAJOR=20
echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_$NODE_MAJOR.x nodistro main" | sudo tee /etc/apt/sources.list.d/nodesource.list

#Run Update and Install
sudo apt-get update
sudo apt-get install nodejs -y
```

Instalar y compilar Lodestar.

```bash
mkdir -p ~/git
cd ~/git
git clone -b stable https://github.com/chainsafe/lodestar.git
cd lodestar
yarn install
yarn run build
```

Verifique que Lodestar se haya instalado correctamente mostrando la versión.

```bash
./lodestar --version
```

Ejemplo de salida de una versión compatible.

```
🌟 Lodestar: TypeScript Implementation of the Ethereum Consensus Beacon Chain.
  * Version: v1.8.0/stable/a4b29cf
  * by ChainSafe Systems, 2018-2022
```

Instale los archivos binarios.

```bash
sudo cp -a $HOME/git/lodestar /usr/local/bin/lodestar
```

</details>

### **3. Setup and configure systemd**

Cree un **archivo de unidad systemd** para definir su `consensus.service` configuración.

```bash
sudo nano /etc/systemd/system/consensus.service
```

Pegue la siguiente configuración en el archivo.

```shell
[Unit]
Description=Lodestar Consensus Layer Client service for Mainnet
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
WorkingDirectory=/usr/local/bin/lodestar
ExecStart=/usr/local/bin/lodestar/lodestar beacon \
  --dataDir /var/lib/lodestar \
  --network mainnet \
  --rest.port 5052 \
  --port 9000 \
  --targetPeers 100 \
  --metrics.port 8008 \
  --metrics true \
  --checkpointSyncUrl https://beaconstate.info \
  --jwt-secret /secrets/jwtsecret \
  --execution.urls http://127.0.0.1:8551 \
  --suggestedFeeRecipient <0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>

[Install]
WantedBy=multi-user.target
```

* Reemplace**`<0x_CHANGE_THIS_TO_MY_ETH_FEE_RECIPIENT_ADDRESS>`** con su propia dirección de Ethereum que usted controla. Las propinas se envían a esta dirección y se pueden gastar de inmediato.
* **No estás haciendo staking?** Si solo quieres un nodo completo, elimina toda la línea que comienza con


```
--suggestedFeeRecipient
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

### 4. Helpful consensus client commands

{% tabs %}
{% tab title="View Logs" %}
```bash
sudo journalctl -fu consensus | ccze
```

**Ejemplo de registros de clientes de consenso de Lodestar sincronizados**

```bash
Mar-19 04:09:49.000    info: Synced - slot: 3338 - head: 3355 0x5abb_ac30 - execution: valid(0x1a3c_2ca5) - finalized: 0xfa22_1142:3421 - peers: 25
Mar-19 04:09:52.000    info: Synced - slot: 3339 - head: 3356 0xcd2a_8b32 - execution: valid(0xab34_fa32) - finalized: 0xfa22_1142:3421 - peers: 25
Mar-19 04:09:04.000    info: Synced - slot: 3340 - head: 3357 0xff1a_f12a - execution: valid(0xfaf1_b35f) - finalized: 0xfa22_1142:3421 - peers: 25
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
sudo rm -rf /var/lib/lodestar/chain-db
sudo systemctl restart consensus
```

Con la sincronización de puntos de control habilitada, el tiempo necesario para volver a sincronizar el cliente de consenso debería tomar solo uno o dos minutos.
{% endtab %}
{% endtabs %}

Ahora que su cliente de consenso está configurado e iniciado, tiene un nodo completo.

Continúe con el siguiente paso para configurar su cliente validador, que convierte un nodo completo en un nodo de staking.

{% hint style="info" %}
 Si querías configurar un nodo completo, no un nodo de participación, detente aquí! Felicitaciones por ejecutar tu propio nodo completo! :tada:
{% endhint %}
