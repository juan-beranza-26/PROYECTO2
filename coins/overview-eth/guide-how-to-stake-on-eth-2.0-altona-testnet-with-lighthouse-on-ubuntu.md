---
descripción: >-
  Altona es una red de prueba ETH 2.0 multicliente. Implementa la Fase Ethereum 2.0
  0 para una cadena de bloques de prueba de participación, que permite a cualquiera que tenga Goerli
  Pruebe ETH para unirse.
---

# Guía: Cómo apostar en ETH 2.0 Altona Testnet con Lighthouse en Ubuntu

## 📶 Estadísticas de la red 0.0 Altona ETH 2.0

* Genesis Time: `1593433805` \(2020-06-29 12:30:05 +0000 UTC\)
* Genesis Block Root: `0x9e2ffe8ab9316e03e45409eaaffab10cf02e1b60e609276113e07240d4a31b7a`
* Deposit Contract: [`0x16e82D77882A663454Ef92806b7DeCa1D394810f`](https://goerli.etherscan.io/address/0x16e82D77882A663454Ef92806b7DeCa1D394810f) \([Goerli Testnet](https://github.com/goerli/testnet)\)
* Blockchain Explorer: [altona.beaconcha.in](https://altona.beaconcha.in/)
* Multi-client test net predecessors: Schlsi and Witti
* Implements ETH 2.0 spec v0.12.1
* [More information](https://github.com/goerli/altona)

## 🐣 0.1 Requisitos previos

### \*\*\*\*🎗 **Requisitos mínimos de configuración**

* **Sistema operativo:** 64-bit Linux \(i.e. Ubuntu 18.04.4 LTS\)
* **Procesador:** Dual core CPU
* **Memoria:** 4GB RAM
* **Almacenamiento:** 20GB SSD
* **Internet:** 24/7 broadband internet connection with speeds at least 1 Mbps.
* **Poder:** 24/7 electrical power
* **ETH balance:** at least 32 Goerli ETH
* **Portafolio**: Metamask installed

Si necesita instalar Ubuntu, consulte

{% page-ref page="../overview-xtz/guide-how-to-setup-a-baker/install-ubuntu.md" %}

Si necesita instalar Metamask, consulte

{% page-ref page="../../wallets/browser-wallets/metamask-ethereum.md" %}

## 🤖 1.Descargue un nodo ETH1, geth

```texto
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update -y
sudo apt-get install ethereum -y
```

o descargarlo manualmente en:

{% embed url="https://geth.ethereum.org/downloads/" %}

## 📄 2. Crear un script de inicio geth

```texto
cat > startGethNode.sh << EOF 
geth --goerli --datadir="$HOME/Goerli" --rpc
EOF
```

## 🐣 3.Inicie el nodo geth para la red de prueba ETH Goerli

```texto
chmod +x startGethNode.sh
./startGethNode.sh
```

{% hint style="info" %}
La sincronización del nodo podría tardar hasta 1 hora.
{% endhint %}

{% hint style="success" %}
Estás completamente sincronizado cuando ves el mensaje: `Nuevo segmento de cadena importado`
{% endhint %}

## ⚙ 4. Obtenga la red de prueba Goerli ETH

Unete a [Prysmatic Labs Discord](https://discord.com/invite/YMVYzv6) y envíe una solicitud de ETH en el **`-request-goerli-eth channel`**

```texto
!send <your metamask goerli network ETH address>
```

De lo contrario, visite a continuación y use metamask para solicitar ETH en el paso 2, "Obtener GöETH - Probar ether".

{% embed url="https://prylabs.net/participate" %}

{% hint style="warning" %}
En ocasiones, este método no es fiable.
{% endhint %}

## 👩💻 5.Instalar óxido

```texto
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

{% hint style="info" %}
En caso de errores de compilación, ejecute`rustup update`
{% endhint %}

Ingrese '1' para continuar con la instalación predeterminada.

Actualice sus variables de entorno.

```texto
echo export PATH="$HOME/.cargo/bin:$PATH" >> ~/.bashrc
source ~/.bashrc
```

Instale dependencias de óxido.

```texto
sudo apt install -y git gcc g++ make cmake pkg-config libssl-dev
```

## 💡 6.Instalar faro

```texto
git clone https://github.com/sigp/lighthouse.git
cd lighthouse
make
```

{% hint style="info" %}
Este proceso de construcción puede tardar hasta una hora.
{% endhint %}

Verifique que el faro esté instalado correctamente.

```texto
lighthouse --version
```

## 🏂 7.Iniciar la cadena de balizas

En una nueva terminal, inicie la cadena de balizas.

```texto
lighthouse beacon --eth1 --http
```

## 🚥 8. Crea una billetera y guarda la semilla mnemotécnica

Esto crea una portafolio llamada `my-validators`protegido por una contraseña generada aleatoriamente y almacenada en `my-validators.pass`

```texto
lighthouse account wallet create --name my-validators --passphrase-file my-validators.pass
```

{% hint style="danger" %}
Asegúrate de guardar tu semilla mnemotécnica.
{% endhint %}

Cree los **voting** y **withdrawal** **keystores** para un validador. Cambie `--count` al \# de validadores que desee.
```texto
lighthouse account validator create --wallet-name my-validators --wallet-passphrase my-validators.pass --count 1
```

> Salida de muestra:
>
> `1/1 0xa777c8a16638c4f3e1f5d80f1b91f8f5fca70b0c3ee56c8e71a616bb04c48d2df4f0928ef8fabde42c8ba201e0b6382a`

{% hint style="info" %}
Tenga en cuenta la clave pública del validador \(0xxa777...64 chars\). [block explorer](https://altona.beaconcha.in/) para ver la actividad de su validador.
{% endhint %}

{% hint style="info" %}
Los datos de la clave pública del validador se guardan en `~$HOME/.lighthouse/validators`
{% endhint %}

## 🏁 9. Configurar firewall y/o reenvío de puertos

De forma predeterminada, el tráfico p2p de Lighthouse se transmite a través del puerto 9000. Abra y/o reenvíe este puerto

## 🧬 10.Iniciar el validador

```texto
lighthouse validator --auto-register
```

{% hint style="info" %}
**Validator client** -Responsable de producir nuevos bloques y certificaciones en la cadena de balizas y cadenas de fragmentos.

**Beacon chain client** -Responsable de gestionar el estado de la cadena de balizas, la mezcla del validador y más.
{% endhint %}

## 📄 11. Copiar los datos del depósito del validador.

Copia el texto de tu `eth1_deposit_data.rlp`  en su directorio de validadores ubicado en

```texto
cd ~/.lighthouse/validators/<your validator's public key>
```

> Por ejemplo, si su nombre de usuario de ubuntu es `ethereum` y la clave pública de su validador es`0xabc123...`, entonces tu `eth1-deposit-data.rlp` se encuentra en
>
> `/home/ethereum/.lighthouse/validators/0xabc123.../`

Vea los datos del depósito y luego cópielos en su portapapeles.

```texto
cat eth1-deposit-data.rlp
```

Guarde los **datos del depósito** para el siguiente paso. Los datos de depósito de muestra son los siguientes:

> 0x2289511800000000000000000000000000000000000000000000000120d77ff7f6ee42ff448b239856012c2650752b664a3e17927135b0a363a78c1b550000000000000000000000000000000000000000000000000000000000000030b539868a621d45b51f66ce88bc80e35099e01f31a0aec8484e7fbd04936056483053c5f2b1d195273b651599555ef35e0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200086c2c1fb70ed4e6435d2f32a3f6a5fdd4596ad5dc82bd6254ef73959d1ec2b0000000000000000000000000000000000000000000000000000000000000060a8480dd7d6341273789afa176e00e2c105cfe76adb670a211da5604c74cb7fd1ee6ceb4753a25400227fbf01cc344e98000d0705db8f3a964692f85901e4cb4fb6211aa5091967c22f550666adfa65bbde8b33c41cdc56fb62564a73a2135c20

## 📩 12. Enviar el depósito del validador

{% hint style="danger" %}
No envíe ETH de la red principal.
{% endhint %}

1. Abrir **Metamask** billetera en tu navegador
2. Asegúrese de haber seleccionado el **Goerli Test Network** en el menú desplegable.
3.Haga clic en el Identicon de su cuenta, el icono circular colorido.
4. Vaya a **Settings then Advanced**
5.Permitir **"Show Hex Data"**
6. Presiona **Send**
7. Ingrese la dirección del contrato de depósito de Altona como destinatario:  [0x16e82D77882A663454Ef92806b7DeCa1D394810f](https://goerli.etherscan.io/address/0x16e82D77882A663454Ef92806b7DeCa1D394810f)
8. Ingrese **32 ETH** en el campo **Cantidad**
9. Pega el **Deposit Data** desde el paso 11 al **Hex Data** campo.
10. Presiona **Next** para enviar.
    
![Turning on Show Hex Data](../../.gitbook/assets/eth2-onyx-metamask.png)

![Sending the deposit data transaction](../../.gitbook/assets/altona.png)

{% hint style="success" %}
Felicidades. Una vez que su cadena de baliza esté sincronizada y el validador esté en funcionamiento, simplemente espere la activación. Este proceso dura hasta 8 horas. Cuando se le asigne, su validador comenzará a crear y votar bloques mientras gana recompensas de apuesta ETH. Encuentre el estado de su validador en [beaconcha.in](https://altona.beaconcha.in)
{% endhint %}

## 🧩 13. Material de referencia

{% embed url="https://lighthouse-book.sigmaprime.io/intro.html" %}



