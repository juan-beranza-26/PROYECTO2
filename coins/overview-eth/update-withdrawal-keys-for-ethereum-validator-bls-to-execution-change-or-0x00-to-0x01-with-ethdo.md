---
descripción: >-
  Una guía simplificada para ayudarle a actualizar las credenciales BLS 0x00 de sus validadores a
  ejecución de retiro de credenciales 0x01 utilizando la herramienta ETHDO de wealdtech.
---

# 🦉 Actualice las claves de retiro para el validador de Ethereum (BLS a cambio de ejecución o 0x00 a 0x01) con ETHDO

{% hint style="info" %}
Los siguientes pasos se alinean con nuestra [mainnet guide](guide-or-how-to-setup-a-validator-on-eth2-mainnet/). Es posible que deba ajustar los nombres de los archivos y las ubicaciones de los directorios cuando corresponda. Los conceptos centrales siguen siendo los mismos.
{% endhint %}

## :pregunta:Resumen: ¿Qué? ¿Retiros? ¿Búhos?

* ¡Saludos, compañero apostador de ETH! Si estaba apostando antes del 2 de abril de 2021, la configuración de las credenciales de retiro de ETH (0x01) aún no se publicó, por lo que esta guía es relevante para usted
* A partir de la actualización de Shapella, los validadores de ETH con credenciales 0x00 deben actualizarse a credenciales 0x01 para permitir retiros parciales y la eliminación del exceso de ETH > 32..
* Si su validador anteriormente salió voluntariamente o ahora desea detener sus funciones de validador, deberá configurar sus credenciales de retiro para reclamar completamente su ETH apostado.

<figure><img src="../../.gitbook/assets/withdrawal-owl.png" alt=""><figcaption><p>Nimbus CL says: WITHDROWLS ON</p></figcaption></figure>

## :Thumbsup:Requisitos previos: antes de comenzar
* Las claves mnemotécnicas de su validador (the offline 24 word secrets)
*Una dirección de retiro de ETH a la que **usted controla las claves privadas**, idealmente una desde una billetera de hardware. :octagonal\_sign::octagonal\_sign: **DO NOT USE A EXCHANGE ADDRESS!** :octagonal\_sign::octagonal\_sign:
* Una **computadora aislada sin conexión**. Crea un Live USB de Linux como [Ubuntu](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) or [Tails](https://tails.boum.org/install/download/index.en.html);necesita una llave USB.
*Un nodo de participación de ETH que utiliza Ubuntu o Linux, también conocido como **computadora en línea**.
* Una llave de almacenamiento USB para mover archivos entre la computadora en línea y sin conexión.
* Familiarízate con el [Ethereum.org Staking Withdrawals guide](https://launchpad.ethereum.org/en/withdrawals).

### Paso 1: preparar la información de la cadena

*Si ya no tiene un nodo completo sincronizado, use la opción 1.
* La opción 2 utiliza su propio cliente de consenso para generar información en cadena.
<details>

<summary>Option 1: Download "offline-preparation.json" file, save to USB key, transfer to offline air-gapped computer.</summary>

1.En su **computadora en línea**, abra una ventana de terminal o shell. Atajo: CTRL + ALT + T

<!---->

2.Descargue Ethdo v1.30.0 desde Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Verifique que la suma de verificación sea válida. Ubicada en la página de lanzamiento, la cadena Checksum se encuentra en el archivo sha256 correspondiente.
```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

La verificación se realiza correctamente si ve "OK" en el resultado resultante.
```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extraer etdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5. Verifique el estado de su credencial de validador con su número de índice. Reemplazar`<MY-VALIDATOR-INDEX>` respectivamente.

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Ejemplo de salida de un validador con credenciales BLS. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

Si tiene credenciales BLS, continúe con el resto de esta guía. De lo contrario, deténgase porque ethdo generará "`Dirección de ejecución de Ethereum`" y eso significa que ya ha configurado su dirección de retiro.

6. Descargue archivos de preparación fuera de línea pregenerados y elaborados diariamente por EthStaker.&#x20;

<pre class="language-bash"><code class="lang-bash">#mainnet
wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz
<strong>wget https://files.ethstaker.cc/offline-preparation-mainnet.tar.gz.sha256
</strong>
#goerli
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz
wget https://files.ethstaker.cc/offline-preparation-goerli.tar.gz.sha256
</code></pre>

7. Verifique el hash sha256 del archivo con los archivos sha256 para garantizar que sea correcto.
```bash
#mainnet
sha256sum offline-preparation-mainnet.tar.gz

#goerli
sha256sum offline-preparation-goerli.tar.gz
```

La salida debe coincidir con el contenido del archivo .sha256. Ver el contenido:

```intento
#mainnet
cat offline-preparation-mainnet.tar.gz.sha256

#goerli
cat offline-preparation-goerli.tar.gz.sha256
```

8. Extraiga el archivo tar para buscar `offline-preparation.json`

```intento
#mainnet
tar -xvf offline-preparation-mainnet.tar.gz

#goerli
tar -xvf offline-preparation-goerli.tar.gz
```

9. Usando su llave USB, copie ambos

* the `ethdo` executable&#x20;
* and `offline-preparation.json` file&#x20;

a su computadora sin conexión a Internet.

</details>

<details>

<summary>Option 2: Generate "offline-preparation.json" file, save to USB key, transfer to offline air-gapped computer.</summary>

1.En su **computadora en línea**, abra una ventana de terminal o shell. Atajo: CTRL + ALT + T

<!---->

2.Descargue Ethdo v1.30.0 desde Github [https://github.com/wealdtech/ethdo/releases](https://github.com/wealdtech/ethdo/releases)

```
cd ~
wget https://github.com/wealdtech/ethdo/releases/download/v1.30.0/ethdo-1.30.0-linux-amd64.tar.gz
```

3. Verifique que la suma de verificación sea válida. Ubicada en la página de lanzamiento, la cadena Checksum se encuentra en el archivo sha256 correspondiente.
```
echo "6fbe587f522ad2eb8d6ce22dfdb15f7d163b491a670bf50e5acf12dd0f58125c ethdo-1.30.0-linux-amd64.tar.gz" | sha256sum -c
```

La verificación se realiza correctamente si ve "OK" en el resultado resultante.

```
ethdo-1.30.0-linux-amd64.tar.gz: OK
```

4. Extraer etdo.

```
tar -xvf ethdo-1.30.0-linux-amd64.tar.gz
```

5. Verifique el estado de su credencial de validador con su número de índice. Replace`<MY-VALIDATOR-INDEX>` respectivamente.

```
./ethdo validator credentials get --validator=<MY-VALIDATOR-INDEX>
```

Ejemplo de salida de un validador con credenciales BLS. :white\_check\_mark:

```
BLS credentials: 0x0002a0addda8106aed690654c7af7af0bc5ccde321c8e5e2319ff432cee70396
```

Si tiene credenciales BLS, continúe con el resto de esta guía. De lo contrario, deténgase porque ethdo generará "`Dirección de ejecución de Ethereum`" y eso significa que ya ha configurado su dirección de retiro.

6. Ejecute el siguiente comando para llamar a su cliente de consenso y generar una lista de validadores activos con información relevante para usar en su computadora fuera de línea. Para generar esta lista desde su nodo de baliza local, [ensure the REST API](https://github.com/wealdtech/ethdo#setting-up) is activado; de lo contrario, el nodo de baliza de reserva predeterminado, [http://mainnet-consensus.attestant.io](http://mainnet-consensus.attestant.io), will be called.

```
./ethdo validator credentials set --prepare-offline
```

Después de uno o dos minutos, deberías ver el texto., "`offline-preparation.json generated`"

7.Usando su llave USB, copie ambos

* el `ethdo` executable&#x20;
* y `offline-preparation.json` file&#x20;

a su computadora sin conexión a Internet.

</details>

### Paso 2: crear un archivo de credenciales de cambio

<details>

<summary>Run ethdo with your mnemonic and withdrawal address. Transfer "change-operations.json" file via USB key back to online computer.</summary>

1. En su **computadora aislada sin conexión**, desconecte todos los cables Ethernet de Internet o WiFi/bluetooth antes de continuar. ¡Asegúrate de estar realmente desconectado!
<!---->

2. Abra una ventana de terminal o shell. Atajo: CTRL + ALT + T
<!---->

3. Después de copiar el ejecutable `ethdo` y el archivo `offline-preparation.json` al **directorio INICIO de su computadora sin conexión**, asegúrese de que el archivo ethdo tenga permisos de ejecución.
```
chmod +x ethdo
```

4. Este comando ethdo establece sus credenciales de validador y el resultado se almacena en un `change-operations.json` file. Replace `<MY MNEMONIC PHRASE>` AND `<MY ETH WITHDRAW ADDRESS>` accordingly.&#x20;

:octagonal\_sign: Double check your work as this is permanent once set! :octagonal\_sign:&#x20;

:octagonal\_sign: FINAL REMINDER: DO NOT USE AN EXCHANGE ETH ADDRESS AS YOUR WITHDRAWAL ADDRESS :octagonal\_sign:

```
./ethdo validator credentials set --offline --mnemonic="<MY MNEMONIC PHRASE>” --withdrawal-address=<MY ETH WITHDRAW ADDRRESS>
```
5. Después de unos segundos, se crea `change- Operations.json`. Es normal que no se muestre ningún mensaje.
6. Verifique tres veces el archivo resultante para su dirección de retiro.

```
cat change-operations.json
```

7. Asegurar el campo**"to\_execution\_address":** contiene su dirección de retiro.

<!---->

8. Usando su llave USB, copie

* `change-operations.json` file

de nuevo a su computadora en línea.

9.Apague su **computadora aislada sin conexión.**

</details>

### 3: Difundir credenciales paso de cambio

<details>

<summary>Simply run the set command to send your change.</summary>

:bulb:Si ya no tiene un nodo completo sincronizado, también puede cargar `change-operation.json` archivo a [https://beaconcha.in/tools/broadcast](https://beaconcha.in/tools/broadcast)

1. En la **computadora en línea**, copie `change-operation.json` a su directorio de inicio, donde también se encuentra `ethdo`.
2. Ejecute el siguiente comando para transmitir sus credenciales de retiro. &#x20;

```
./ethdo validator credentials set
```

</details>

{% hint style="success" %}
¡Felicitaciones! Su cambio de BLS a Ejecución ahora está pendiente en una cola, esperando ser incluido en un bloque.&#x20;
{% endhint %}

## :fast\_forward:Next Steps

#### For your information:

* En cada bloque propuesto se incluyen hasta 16 cambios de BLS a Ejecución.
* Dependiendo del tamaño de la cola de retiro, su cambio de retiro puede demorar algunos días en finalizarse.
* Terminología: prefijo de 0x01 = "Tipo 1" = credenciales de retiro de ejecución = Retiros habilitados
* Como retiro parcial, periódicamente, cada pocos días, cualquier cantidad de ETH superior a 32 se transferirá automáticamente a su dirección de retiro.
#### Learn more from:

*Referencias oficiales de retirada de la capa de consenso
  * prisma: [https://docs.prylabs.network/docs/wallet/withdraw-validator](https://docs.prylabs.network/docs/wallet/withdraw-validator)
  * Nimbo: [https://nimbus.guide/withdrawals.html](https://nimbus.guide/withdrawals.html)
  * Faro: [https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds](https://lighthouse-book.sigmaprime.io/voluntary-exit.html#withdrawal-of-exited-funds)
  * Teku: [https://docs.teku.consensys.net/HowTo/Withdrawal-Keys](https://docs.teku.consensys.net/HowTo/Withdrawal-Keys)
  * Lodestar: [https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change](https://chainsafe.github.io/lodestar/reference/cli/#validator-bls-to-execution-change)
* Ethdo official withdrawals guide: [https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md](https://github.com/wealdtech/ethdo/blob/master/docs/changingwithdrawalcredentials.md)
*Publicación del declarante: [https://www.attestant.io/posts/understanding-withdrawals/](https://www.attestant.io/posts/understanding-withdrawals/)

#### **¿Necesita soporte adicional en vivo?**&#x20;

* Encuentra a Ethstaker frens en el [Ethstaker](https://discord.io/ethstaker) Discord!
* Usar reddit: [r/Ethstaker](https://www.reddit.com/r/ethstaker/), or [DMs](https://www.reddit.com/user/coincashew), or [r/coincashew](https://www.reddit.com/r/coincashew/)

#### ¿Te gustan estas guías?

* [Tips much appreciated](../../donations.md) :pray:
* [**Support us on Gitcoin Grants**](https://gitcoin.co/grants/1653/eth2-staking-guides-by-coincashew)**:** ¡Construimos esta guía exclusivamente con el apoyo de la comunidad!🙏
*Comentarios o solicitudes de extracción:[https://github.com/coincashew/coincashew](https://github.com/coincashew/coincashew)

## :libros: preguntas frecuentes

<details>

<summary>Using my node, how can I check if my change is pending in the withdrawals queue?</summary>

Replace \<MY VALIDATOR INDEX>. Adjust the REST API port number, if needed.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s "http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes" | jq '.data | map(select(.message.validator_index=="<MY VALIDATOR INDEX>"))'
```

Salida de ejemplo:

```
[
  {
    "message": {
      "validator_index": "96193",
      "from_bls_pubkey": "0xb67aca71f04b673037b54009b760f1961f3836e5714141c892afdb75ec0834dce6784d9c72ed8ad7db328cff8fe9f13e",
      "to_execution_address": "0xb9d7934878b5fb9610b3fe8a5e441e8fad7e293f"
    },
    "signature": "0x988251748925e7a2966f28230c250e8c37495346d551e86fd89ea53148302b1145eb069647572801a689c9c1c5b8f2071019e652e01d92055d9aa99aa86696eb453889de38733caf2d5dce7a2786fed910365dcb7df082a62b130436fb9a1035"
  }
]
```

Sin embargo, si el resultado muestra \[], esto significa que su cambio se completó y ya no está en la cola.

</details>

<details>

<summary>¿Cuánto más debo esperar para que este cambio surta efecto?</summary>

Cada bloque puede sumar 16 `blstoexecutionchange`mensajes y el tiempo para procesar un cambio BLS depende del tamaño de la cola de retiro.&#x20;

Encuentre el tamaño de la cola con el siguiente comando.&#x20;

Ajuste el número de puerto de la API REST, si es necesario.&#x20;

Lighthouse/Nimbus=5052. Prysm=3500. Lodestar=9596. Teku=5051.

```
curl -s http://localhost:5052/eth/v1/beacon/pool/bls_to_execution_changes | jq '.data | length'
```

</details>

<details>

<summary>¿Cómo sé que el cambio de credencial funcionó?</summary>

Reemplazar `<MyValidatorIndex>` y ejecute el siguiente comando ethdo:

```
ethdo validator credentials get --validator=<MyValidatorIndex>
```

La salida resultante comenzará con: `Ethereum execution address`

Alternativamente, consulte su explorador de cadenas de balizas favorito, como [beaconcha.in](https://beaconcha.in/validators/withdrawals) y [etherscan.io](https://etherscan.io/) para las credenciales 0x01.

</details>

<details>

<summary>Salí voluntariamente hace un tiempo y ya no tengo un nodo sincronizado. ¿Cuáles son mis opciones?</summary>

Utilice Ethdo en una computadora fuera de línea para crear el mensaje de salida, como se muestra en el paso 2 anterior, y luego realice el paso 3 usando el método de transmisión alternativo con beaconcha.in

</details>

<details>

<summary>¿Puedo usar una dirección de Gnosis Safe como mi dirección de retiro de ETH?</summary>

Sí, de hecho, esta también es una gran idea, ya que le permite rotar claves privadas (y mantener la misma dirección pública) o utilizar otras estrategias más multifirma.

referencia: [https://safe.global](https://safe.global/)

</details>

<details>

<summary>¿La dirección del destinatario de la tarifa es la misma que esta dirección de retiro?</summary>

Ambos se pueden configurar con la misma dirección ETH; sin embargo, comprenda que estas son independientes y que las **credenciales de retiro** tienen un propósito diferente al de su **destinatario de la tarifa**, que recibe sugerencias sobre las tarifas de transacción de los bloques propuestos.

</details>

<details>

<summary>¿Retiros parciales versus retiros completos?</summary>

* **Retiro completo del validador:** Para retirar toda su apuesta en Ethereum y dejar de realizar tareas de validador. Salga de su validador y luego, después de que su solicitud de salida avance a través de la cola de retiro, finalmente el saldo completo del validador se transfiere a su dirección de retiro.
* **Retiro parcial del validador:** Para retirar únicamente las ganancias de su validador. Para un validador, cualquier monto superior al depósito inicial de 32 ETH son las ganancias y se transfieren automáticamente cada pocos días a la dirección de retiro.

</details>

<details>

<summary>No tengo una frase mnemotécnica. Usé una clave privada.</summary>

En el paso 2, utilice este comando de configuración de credenciales.

```
ethdo validator credentials set --private-key=<my-priv-key> --withdrawal-address=<my-eth-withdrawal-address>
```

</details>

<details>

<summary>Quiero direcciones de retiro diferentes para cada uno de mis validadores.</summary>

En el paso 2, utilice este comando de configuración de credenciales.

```
ethdo validator credentials set --mnemonic="<my-mnemonic-phrase>" --path='m/12381/3600/<my iTH validator>/0' --withdrawal-address=<my-eth-withdrawal-address>
```

Donde la ruta es la ruta de derivación a su clave de retiro.

* Por ejemplo, `m/12381/3600/`_`i`_`/0` es el camino a una clave de retiro, donde _i_ starts en 0 para su primer validador, 1 para su segundo validador...

</details>

<details>

<summary>Necesito cambiar mi dirección de retiro.</summary>

La única forma de cambiar las direcciones de retiro es realizar un retiro completo saliendo de un validador y luego creando una nueva clave de validador como si comenzara el proceso de apuesta nuevamente.

</details>
