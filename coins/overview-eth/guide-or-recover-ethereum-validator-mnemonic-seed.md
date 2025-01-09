---
descripción: >-
  Cómo recuperar la frase de recuperación secreta de 24 palabras de un validador de ethereum con
  seedrecovery.py por btcrecover.
---

# 🔎 Guía | Recuperar la semilla mnemotécnica del validador de Ethereum

## :pregunta:¿Para quién es la guía, qué implica y cuándo la necesito?

* :people\_hugging: **WHO**: If you are an Ethereum validator experiencing errors due to a problematic secret recovery phrase or mnemonic, this guide can help you fix the majority of common errors.
* :chains: **WHAT**: Using [btcrecover's](https://github.com/3rdIteration/btcrecover) Seedrecover.py can fix the majorty of simple "invalid mnemonic" or "invalid seed" type errors. This commonly occurs when you have a valid seed backup that simply has a typo in it or missing word(s).
* :hourglass: **WHEN**: Typical scenarios include attempting to **set withdrawal credentials** or **re-generate keystore files**.

## :hammer\_pick: How to Recover?

## Paso 1: configurar una PC sin conexión con Ubuntu Live USB

* Primero, necesitarás crear un Ubuntu Live USB. [Download Ubuntu ISO](https://ubuntu.com/download/desktop) y flashee la imagen ISO a la unidad USB con [balenaEtcher](https://etcher.balena.io/). Vea demostraciones en vídeo a continuación o [this guide](https://itsfoss.com/create-live-usb-of-ubuntu-in-windows/).

{% hint style="info" %}
:tv: **Video Demos**

**Windows**: Instalación de BTCRecover en Windows: [https://youtu.be/8q65eqpf4gE](https://youtu.be/8q65eqpf4gE)

**Linux**: Instalación de BTCRecover en Ubuntu Live USB: [https://youtu.be/Met3NbxcZTU](https://youtu.be/Met3NbxcZTU)
{% endhint %}

## Step 2: Instalar btcrecover

Abra una ventana de terminal desde cualquier lugar escribiendo "Ctrl+Alt+T".

Con una terminal abierta dentro de su sesión de Ubuntu Live, ejecute lo siguiente.

```intento
# Install dependencies
sudo apt install python3-tk
sudo add-apt-repository universe
sudo apt install python3-pip

# Download btcrecover and install requirements
cd ~
wget https://github.com/3rdIteration/btcrecover/archive/master.zip
unzip master.zip
cd btcrecover-master
pip3 install -r requirements.txt
pip3 install git+https://github.com/ethereum/staking-deposit-cli.git@v2.5.0
pip3 install py_ecc

# Verify the install by running tests
python3 run-all-tests.py -vv
```

## Paso 3: desconectarse

* Desconecte el cable ethernet y/o desactive el WIFI.
* Verify no internet access by attemping to ping.&#x20;

```intento
ping 8.8.8.8
```

El resultado del ping debería decir **La red es inalcanzable**.

## Paso 4: Ejecute el proceso de recuperación

{% hint style="info" %}
La recuperación de semillas de Ethereum Validator es la misma que una recuperación de semillas estándar, pero utiliza la clave pública de los validadores (también conocida como clave de firma) en lugar de una dirección.


Seedrecover.py will automatically run through four search phases that should take a few hours at most. The four search phases include:

*Error tipográfico único
* Dos errores tipográficos, incluido uno en el que es posible que tengas una palabra completamente diferente
* Tres errores tipográficos, incluido uno en el que es posible que tengas una palabra completamente diferente
* Dos errores tipográficos que podrían ser palabras completamente diferentes.
{% endhint %}

Aquí está el comando para recuperar la fase de recuperación secreta de su validador ETH. Actualice en consecuencia.
```intento
python3 seedrecover.py --mnemonic "<secret recovery phrase>" --addrs <pubkey of validator> --wallet-type ethereumvalidator --addr-limit <number of validators created> --mnemonic-length 24
```

### :pregunta:Acerca de las banderas

* **--mnemonic** : Enter your best guess for your seed/mnemnoic/secret recovery phrase
* **--addrs** :  this is your validators public key without the 0x prefix. For example, if you have the keystores files the pubkey can be found inside "keystore-m\_12381\_3600\_0\_0\_0-xxxxxxx.json" Without keystore files, you can look up your validator's pubkey online at beaconcha.in or etherscan.io by following the validator's 32ETH deposit transaction.
* **--addr-limit** : Adjust this to the number of validators you created on this mnemonic.

{% hint style="warning" %}
No recomendado: ¿Ejecutar la recuperación en una máquina en línea? Agrega la bandera `--dsw`
{% endhint %}

### :thumbsup: Ejemplos de uso
Mejor suposición: solo tiene 23 palabras. Falta una palabra en alguna parte.
```bash
python3 seedrecover.py --mnemonic "cousin fury spatial aisle day write bulk empower fog fault stick broom demand valve fine able subway absent valve inform coconut oval season" --addrs 99722e2d3cdf850ef76516273273b5b2bfd062a6b706f6c395e116183fecd1ba6f9e9a479006a621168154e260f1a9d9 --wallet-type ethereumvalidator --addr-limit 1 --mnemonic-length 24
```

Coloque una x donde esté la palabra incorrecta, se conoce la posición.
```intento 
python3 seedrecover.py --mnemonic "cousin fury spatial aisle day write bulk empower fog fault stick broom demand valve fine able x absent valve inform coconut oval season master" --addrs 99722e2d3cdf850ef76516273273b5b2bfd062a6b706f6c395e116183fecd1ba6f9e9a479006a621168154e260f1a9d9 --wallet-type ethereumvalidator --addr-limit 1 --mnemonic-length 24
```

Dos palabras incorrectas o faltantes, se conoce la posición.
```intento 
python3 seedrecover.py --mnemonic "cousin fury spatial aisle day write bulk empower fog fault stick broom demand valve fine able x x absent valve inform coconut oval season master" --addrs 99722e2d3cdf850ef76516273273b5b2bfd062a6b706f6c395e116183fecd1ba6f9e9a479006a621168154e260f1a9d9 --wallet-type ethereumvalidator --addr-limit 1 --mnemonic-length 24
```

Creé 3 validadores. El límite de dirección es 3. Falta una palabra en alguna parte.
```intento
python3 seedrecover.py --mnemonic "cousin fury spatial aisle day write bulk empower fog fault stick broom demand valve fine able absent valve inform coconut oval season master" --addrs b9bf3e3781f288547a10a65f3ec18d38668be0af5b498b446b95530e2adee6eb30fa4c0a47abe93b198d8fed7d68385a --wallet-type ethereumvalidator --addr-limit 3 --mnemonic-length 24
```

### :tada: Muestra de registros al recuperar exitosamente el mnemónico.
```
Starting seedrecover 1.13.0-CryptoGuide, btcrecover 1.13.0-Cryptoguide on Python 3.10.12 64-bit, 21-bit unicodes, 64-bit ints

Assuming a 24 word mnemonic. (This can be overridden with --mnemonic-length)
2024-03-03 12:41:45 : Phase 1/4: up to 2 mistakes, excluding entirely different seed words.
Not enough entirely different seed words permitted; skipping this phase.
2024-03-03 12:41:45 : Search Complete
 Seed not found
2024-03-03 12:41:45 : Phase 2/4: 1 mistake which can be an entirely different seed word.
Wallet Type: btcrseed.WalletEthereumValidator
2024-03-03 12:41:48 : Using 10 worker threads
1660 of 2048 [#############################################################################################----------------------] 0:00:01, ETA:  0:00:00
2024-03-03 12:41:50 : Search Complete

If this tool helped you to recover funds, please consider donating 1% of what you recovered, in your crypto of choice to:
BTC: 37N7B7sdHahCXTcMJgEnHz7YmiR4bEqCrS 
BCH: qpvjee5vwwsv78xc28kwgd3m9mnn5adargxd94kmrt 
LTC: M966MQte7agAzdCZe5ssHo7g9VriwXgyqM 
ETH: 0x72343f2806428dbbc2C11a83A1844912184b4243 

Find me on Reddit @ https://www.reddit.com/user/Crypto-Guide

You may also consider donating to Gurnec, who created and maintained this tool until late 2017 @ 3Au8ZodNHPei7MQiSVAWb7NB2yqsb48GW4

Seed found: cousin fury spatial aisle day write bulk empower fog fault stick broom demand valve fine able subway absent valve inform coconut oval season master
```

## Paso 5: Apagar la PC sin conexión
Al cerrar la sesión ISO de Ubuntu Live, se olvidan todos los datos utilizados durante esta tarea.
{% hint style="success" %}
¡¡¡Felicitaciones por recuperar su validador ETH !!! Considere hacer una donación a los fabricantes de herramientas, btcrecover.{% endhint %}

## :track\_next: Próximos pasos

* :brain:**¡Haz una copia de seguridad de tu frase secreta de recuperación!** [Review Backups Checklist](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-ii-maintenance/backups-checklist-critical-staking-node-data)
* :moneybag: **Actualizar** [**Withdrawal Credentials**](https://www.coincashew.com/coins/overview-eth/update-withdrawal-keys-for-ethereum-validator-bls-to-execution-change-or-0x00-to-0x01-with-ethdo)
* :new: *Regenerado** [**Keystore Files**](https://www.coincashew.com/coins/overview-eth/guide-or-how-to-setup-a-validator-on-eth2-mainnet/part-iii-tips/adding-a-new-validator-to-an-existing-setup)
* :computer: **Gestiona tu nodo con** [**EthPillar**](https://www.coincashew.com/coins/overview-eth/ethpillar)
* ​:confetti\_ball: **Apóyanos con Gitcoin Grants:** ¡Construimos esta guía exclusivamente con el apoyo de la comunidad!🙏
## :books: References

*btcrecover github: [https://github.com/3rdIteration/btcrecover](https://github.com/3rdIteration/btcrecover)
* documentos oficiales: [https://btcrecover.readthedocs.io/en/latest/Usage\_Examples/basic\_seed\_recoveries/#basic-ethereum-validator-recoveries](https://btcrecover.readthedocs.io/en/latest/Usage\_Examples/basic\_seed\_recoveries/#basic-ethereum-validator-recoveries)
