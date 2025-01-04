# Configuración de claves de validación

## :seedling: 1. Obtener ETH de la red de prueba

{% hint style="info" %}
Cada 32 ETH que posees te permite hacer 1 validador. Puede ejecutar miles de validadores con su nodo. Sin embargo, en la red de pruebas, solo ejecute 1 o 2 validadores para mantener la cola de activación razonablemente rápida.
{% endhint %}

<details>

<summary>Opción 1: Canal de validación de #cheap hoyos de Ethstaker</summary>

* **Paso 1**: Visita el [Ethstaker Discord](https://discord.io/ethstaker) y únete al canal de validadores de #cheap-holesky.

<!---->

* **Paso 2**: Usa el comando de barra diagonal y sigue las instrucciones del bot. Debe comenzar a escribir el comando de barra diagonal y se mostrará encima de su cuadro de entrada donde puede usarlo.

<!---->

* **Requisito**: Usa el comando de barra diagonal y sigue las instrucciones del bot. Debe comenzar a escribir el comando de barra diagonal y se mostrará encima de su cuadro de entrada donde puede usarlo. `0x4D496CcC28058B1D74B7a19541663E21154f9c84`

</details>

<details>

<summary>Opción 2: Use el grifo de pk910</summary>

Enlace: [https://holesky-faucet.pk910.de](https://holesky-faucet.pk910.de/)

</details>

## :key: 2. Generación de claves de validación

#### Antes de continuar, tenga a mano lo siguiente:

* [ ] **Dirección de billetera de hardware o dirección** [**Safe multisig wallet**](https://app.safe.global/welcome) **address**: Esta es para su [Withdrawal Address](https://notes.ethereum.org/@launchpad/withdrawals-faq#Q-What-are-the-two-types-of-withdrawals). Debe estar en formato de suma de comprobación, lo que significa que algunas letras están en mayúsculas. Si es necesario, valide el formato de suma de comprobación de su dirección con un explorador de bloques, como [https://etherscan.io](https://etherscan.io)
* [ ] **Browser dApp Wallet** (es decir, Metamask) con 32 Ethers para cada validador

<figure><img src="../../../../.gitbook/assets/checksum.png" alt=""><figcaption><p>Ejemplo de la dirección de Vitalik en formato de suma de comprobación</p></figcaption></figure>

#### Antes de continuar, comprenda lo siguiente:

* [ ] La **dirección de retiro** es:
  * donde su ETH se devuelve al "salir voluntariamente de un validador", o también se conoce como retiro completo.
  * donde recibe retiros parciales, que es donde cualquier exceso de saldo superior a 32 ETH se raspa periódicamente y se pone a disposición para su uso.
* [ ] Como esto es **permanente** una vez configurado, **verifique tres veces** su dirección.
* [ ] NO UTILICE UNA :octagonal\_sign: **DIRECCIÓN DE INTERCAMBIO** :octagonal\_sign: COMO DIRECCIÓN DE RETIRO.
* [ ] Para **fines de la red de prueba**, está bien usar una dirección de navegador/billetera caliente.

{% hint style="warning" %}
**Mejores prácticas de generación de claves fuera de línea**: La semilla mnemotécnica (24 palabras) de su validador debe protegerse manteniéndola sin conexión. Usa Tails OS (ver opción 3) o un [Linux Live USB with staking-deposit-cli](https://www.youtube.com/watch?v=oDELXYNSS5w) (Opción 1) o Wagyu para generar claves.

Si esto no es posible, al menos desconéctate físicamente de la red desconectando el cable Ethernet o desconectándote de Wifi.
{% endhint %}

Formas de crear tus claves de validación:

<details>

<summary>Opción 1 para Ubuntu - staking-deposit-cli</summary>

**1. Descarga** [**staking-deposit-cli**](https://github.com/ethereum/staking-deposit-cli#introduction) **desde Github.**

```bash
#Install dependencies
sudo apt install jq curl -y

#Setup variables
RELEASE_URL="https://api.github.com/repos/ethereum/staking-deposit-cli/releases/latest"
BINARIES_URL="$(curl -s $RELEASE_URL | jq -r ".assets[] | select(.name) | .browser_download_url" | grep linux-amd64.tar.gz$)"
BINARY_FILE="staking-deposit-cli.tar.gz"

echo "Downloading URL: $BINARIES_URL"

cd $HOME
#Download binary
wget -O $BINARY_FILE $BINARIES_URL
#Extract archive
tar -xzvf $BINARY_FILE -C $HOME
#Rename
mv staking_deposit-cli*amd64 staking-deposit-cli
cd staking-deposit-cli
```

**2. Haz una nueva mnemotecnia**

Reemplace con la dirección de retiro. `<HARDWARE_WALLET_ADDRESS>`

```
./deposit new-mnemonic --chain holesky --execution_address <HARDWARE_WALLET_ADDRESS>
```

* Elige tu idioma
* Repita su dirección de retiro/ejecución para confirmar
* Elija el idioma de la lista de palabras mnemotécnicas
* Elija cuántos validadores nuevos desea ejecutar
* Cree una **contraseña de almacén de claves** contraseña de almacén de claves
* Repita la **contraseña de su almacén de claves** para confirmar
* Escribe tu semilla mnemotécnica de 24 palabras
* Escribe tu mnemotecnia, las primeras 4 letras son suficientes

Verá los siguientes mensajes después de generar con éxito los almacenes de claves y los depósitos:

```bash

                  #####     #####
                ##     #####     ##
    ###         ##   #######     #########################
    ##  ##      #####               ##                   ##
    ##     #####                 ##                       ##
    ##     ##                     ##                      ###
   ########                        ##                     ####
   ##        ##   ###         #####                       #####
   #                          ##                         # #####
   #                            #                        #  #####
   ##                             ##                    ##
   ##                              ##                   ##
   ##             ###              ##                   ##
   ###############                 ##                   ##
   ###               ##                                 ##
      #############################                    ##
                     ##                             ###
                     #######     #################     ###
                     ##   ## ##        ##   ##    ###
                     ##############          #############

Creating your keys:               [####################################]  <N>/<N>
Creating your keystores:          [####################################]  <N>/<N>
Verifying your keystores:         [####################################]  <N>/<N>
Verifying your deposits:          [####################################]  <N>/<N>

Success!
Your keys can be found at: /home/username/staking-deposit-cli/validator_keys
```

**3. Verifica la semilla mnemotécnica**

Al regenerar los archivos del almacén de claves y compararlos con los originales, se verifica que la mnemotecnia vista es correcta al ser reproducible.

```bash
#Make temp directory to verify seeds
mkdir -p ~/staking-deposit-cli/verify_seed
#Re-generate keys
./deposit existing-mnemonic --chain holesky --folder verify_seed --execution_address <HARDWARE_WALLET_ADDRESS>
```

* Elige tu idioma
* Repita su dirección de retiro/ejecución para confirmar
* Escribe tu semilla mnemotécnica, las primeras 4 letras son suficientes
* Dado que es la primera vez que se generan claves, introduzca el número de índice como 0.
* Repita el índice para confirmar, 0.
* Introduzca el número de validadores que desea ejecutar (igual que antes)
* Introduzca cualquier contraseña del almacén de claves, ya que es temporal y se eliminará

Compare los archivos **deposit_data**.

```bash
diff -s validator_keys/deposit_data*.json verify_seed/validator_keys/deposit_data*.json
```

Cuando los archivos **deposit_data** son iguales, esto significa que su semilla mnemotécnica es correcta.

Ejemplo de salida:

```
Files validator_keys/deposit_data-16945983.json and verify_seed/validator_keys/deposit_data-16647657.json are identical
```

Limpiar archivos duplicados.
```bash
rm -r verify_seed
```

</details>

<details>

<summary>Opción 2 para Windows/Linux/Mac - Wagyu GUI</summary>

**Wagyu** es una aplicación destinada a bajar el listón técnico para hacer staking en Ethereum.

Como "instalador de un solo clic", proporciona una interfaz de usuario limpia que automatiza la configuración y la gestión de toda la infraestructura necesaria para apostar sin que el usuario tenga ningún conocimiento técnico.

**Descargar Wagyu**: [https://wagyu.gg](https://wagyu.gg/)

1. Haz clic en crear nueva frase secreta de recuperación
2. Selecciona tu red
3. Haga clic en crear
4. Escribe tu frase secreta de recuperación de 24 palabras
5. Revisa la frase para confirmar que la has copiado correctamente
6. Especificar el número de claves nuevas que se van a generar
7. Especifique la contraseña del almacén de claves
8. Especifica tu dirección de retiro
9. Haga clic en siguiente
10. Vuelva a escribir la contraseña de su almacén de claves
11. Vaya al lugar donde desea guardar las llaves
12. Revise la información y cierre

</details>

<details>

<summary>Opción 3 - Tails fuera de línea con staking-deposit-cli</summary>

Aprenderá cómo iniciar una PC con Windows en un [sistema operativo Tails](https://tails.boum.org/index.en.html). con espacio aéreo.

El sistema operativo Tails es un sistema operativo amnésico, lo que significa que no guardará nada y no dejará rastros cada vez que lo inicie.

**1. Requisitos previos**

Te hace falta:

* 2 medios de almacenamiento (pueden ser memorias USB, tarjetas SD o discos duros externos)
* Uno de ellos debe ser > de 8 GB
* Computadora Windows o Mac
* 30 minutos o más, dependiendo de la velocidad de descarga

**2. Descargar Tails OS**

Descarga la imagen oficial del [sitio web de Tails](https://tails.boum.org/install/index.en.html). Puede tomar un tiempo, ve a tomar un café.

Asegúrate de seguir la guía en el sitio web de Tails para verificar tu descarga de Tails.

**3. Descarga e instala el software para transferir tu imagen de Tails en tu memoria USB**

Para Windows, use uno de los siguientes

* [Etcher](https://tails.boum.org/etcher/Etcher-Portable.exe)
* [Generador de imágenes de disco Win32](https://win32diskimager.org/#download)
* [Rufus](https://rufus.ie/en\_US/)

Para Mac, descarga [Etcher](https://tails.boum.org/etcher/Etcher.dmg)

**4. Hacer que su memoria USB de arranque**

Ejecute el software anterior. Este es un ejemplo de cómo se ve en Mac OS con grabador, pero otro software debería ser similar.

<img src="../../../../.gitbook/assets/etcher_in_mac.png" alt="" data-size="original">

Selecciona la imagen del sistema operativo Tails que descargaste como imagen. A continuación, seleccione la memoria USB (la más grande).

A continuación, flashea la imagen en la memoria USB más grande.
**5. Descargue y verifique la CLI de depósito de staking**

Descargue el último binario de staking-deposit-cli consultando los pasos de la Opción 1.

Copie el archivo en la otra memoria USB.

**6. Reinicia tu computadora y entra en el sistema operativo Tails**

Una vez que haya hecho todo lo anterior, puede reiniciar. Si está conectado a Internet mediante un cable LAN, puede desconectarlo manualmente.
Conecta la memoria USB que tiene tu sistema operativo Tails.

En Mac, mantenga presionada la tecla Opción inmediatamente después de escuchar el timbre de inicio. Suelte la clave después de que aparezca el Administrador de inicio.

En Windows, depende del fabricante de su computadora. Por lo general, es presionando F1 o F12. Si no funciona, intente buscar en Google "Ingresar al menú de opciones de arranque en [Inserte la marca de su PC]"

Elige la memoria USB que cargaste con el sistema operativo Tails para arrancar en Tails.

**7. Bienvenido al sistema operativo Tails**

<img src="../../../../.gitbook/assets/grub.png" alt="" data-size="original">

Puede arrancar con todas las configuraciones predeterminadas.

**8. Ejecute la CLI de depósito de staking**

Conecte su otra llave USB con el archivo. `staking-deposit-cli`

Localice la llave USB, monte la unidad y agregue permisos de ejecución.
```bash
# Locate the usb key
sudo fdisk -l
# Create a mount point
sudo mkdir -p /media/usb-drive
# Mount the usb key. Change device name
sudo mount /dev/sda1 /media/usb-drive
# Change directories
cd /media/usb-drive/staking-deposit-cli
# Add execute permissions
sudo chmod +x ./deposit
```

**9. Haz una nueva mnemotecnia**

Reemplace con la dirección de retiro. `<HARDWARE_WALLET_ADDRESS>` 

```
./deposit new-mnemonic --chain holesky --execution_address <HARDWARE_WALLET_ADDRESS>
```

* Elige tu idioma
* Repita su dirección de retiro/ejecución para confirmar
* Elija el idioma de la lista de palabras mnemotécnicas
* Elija cuántos validadores nuevos desea ejecutar
* Cree una **contraseña de almacén de claves** que proteja los archivos del almacén de claves del validador
* Repita **contraseña de su almacén de claves** para confirmar
* Escribe tu semilla mnemotécnica de 24 palabras
* Escribe tu mnemotecnia, las primeras 4 letras son suficientes

Verá los siguientes mensajes después de generar con éxito los almacenes de claves y los depósitos:

```bash

                  #####     #####
                ##     #####     ##
    ###         ##   #######     #########################
    ##  ##      #####               ##                   ##
    ##     #####                 ##                       ##
    ##     ##                     ##                      ###
   ########                        ##                     ####
   ##        ##   ###         #####                       #####
   #                          ##                         # #####
   #                            #                        #  #####
   ##                             ##                    ##
   ##                              ##                   ##
   ##             ###              ##                   ##
   ###############                 ##                   ##
   ###               ##                                 ##
      #############################                    ##
                     ##                             ###
                     #######     #################     ###
                     ##   ## ##        ##   ##    ###
                     ##############          #############

Creating your keys:               [####################################]  <N>/<N>
Creating your keystores:          [####################################]  <N>/<N>
Verifying your keystores:         [####################################]  <N>/<N>
Verifying your deposits:          [####################################]  <N>/<N>

Success!
Your keys can be found at: /home/username/staking-deposit-cli/validator_keys
```

**Resultado**: una carpeta llamada que contiene archivos keystore-m y deposit_data.json `validator_keys`

**10. Verificar la semilla mnemotécnica**

Al regenerar los archivos del almacén de claves y compararlos con los originales, se verifica que la mnemotecnia vista es correcta al ser reproducible.

```bash
#Make temp directory to verify seeds
mkdir verify_seed
#Re-generate keys
./deposit existing-mnemonic --chain holesky --folder verify_seed --execution_address <HARDWARE_WALLET_ADDRESS>
```

* Elige tu idioma
* Repita su dirección de retiro/ejecución para confirmar
* Escribe tu semilla mnemotécnica, las primeras 4 letras son suficientes
* Dado que es la primera vez que se generan claves, introduzca el número de índice como 0.
* Repita el índice para confirmar, 0.
* Introduzca el número de validadores que desea ejecutar (igual que antes)
* Introduzca cualquier contraseña del almacén de claves, ya que es temporal y se eliminará

Compare los archivos **deposit_data**.

```bash
diff -s validator_keys/deposit_data*.json verify_seed/validator_keys/deposit_data*.json
```

Cuando los archivos **deposit\_data** son iguales, esto significa que su semilla mnemotécnica es correcta.

Ejemplo de salida:

```
Files validator_keys/deposit_data-16945983.json and verify_seed/validator_keys/deposit_data-16647657.json are identical
```

Limpiar archivos duplicados.

```bash
rm -r verify_seed
```

Si ejecutaste este comando directamente desde tu memoria USB que no sea de Tails, las claves del validador deberían permanecer en ella.

Si no lo ha hecho, copia el directorio en tu memoria USB que no sea de Tails.

Confirme que sus validator_keys están en la memoria USB antes de salir.

```bash
ls /media/usb-drive/staking-deposit-cli/validator_keys
```

:fire: Asegúrate de haber guardado tu directorio de claves de validación en tu otra memoria USB (que no sea Tails OS) antes de apagar Tails. Tails eliminará todo lo que se guarde en él después de que lo apagues.

:tada: Felicidades por aprender a usar Tails OS para hacer un sistema con espacio de aire.

</details>

### Resultado: Se generan dos tipos de archivos.

| Tipo de archivo                                                           | Propósito                                                                                                                                                                                                                                        |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Archivo(s) de almacén de claves</p><p>[es decir, keystore-16945983.json]</p>        | <ul><li>Controla la capacidad del validador para firmar transacciones</li><li>Importado y cargado en su validador</li><li>Mantén la privacidad. No compartir con nadie</li><li>Se puede recrear a partir de tu semilla mnemotécnica/frase de recuperación secreta</li></ul> |
| <p>Archivo(s) de datos de depósito</p><p>[es decir, deposit_data-16945983.json]</p> | <ul><li>Información pública sobre su validador</li><li>Necesario para ejecutar tu depósito a través de Ethereum Launchpad</li><li>Se puede recrear a partir de tu semilla mnemotécnica/frase de recuperación secreta</li></ul>                                      |

## :arrow\_up: 3. Transferir claves de validador al nodo

{% hint style="info" %}
**Nodo local**: Omite este paso si generaste tus claves en tu nodo con **staking-deposit-cli**. No es necesario transferir, ya que ya están allí.
{% endhint %}

Después de crear las claves de validador sin conexión, querrá copiar estas claves de validador en su nodo.

Para alinearse con los pasos de esta guía, cree la ruta de validator_keys predeterminada en el nodo.

<pre class="language-bash"><code class="lang-bash"><strong>mkdir -p $HOME/staking-deposit-cli/validator_keys
</strong></code></pre>

Para transferir archivos de clave de validador a su nodo desde su computadora local, considere usar:

<details>

<summary>Opción 1 - Transferencia de archivos</summary>

* Transferencia de archivos
  * Sistema operativo Windows: use [WinSCP](https://winscp.net) o [FileZilla](https://filezilla-project.org/download.php?type=client)
  * Mac o Linux: use [FileZilla](https://filezilla-project.org/download.php?type=client) or [SFTP](https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server) o [rsync](https://linuxize.com/post/how-to-use-rsync-for-local-and-remote-data-transfer-and-synchronization/)

Transfiera los archivos **keystore-m_xxxxxxxx.json** a la siguiente ubicación en el nodo.

```bash
$HOME/staking-deposit-cli/validator_keys
```

</details>

<details>

<summary>Opción 2 - Llave USB</summary>

### **Paso 1: Desde la máquina OFFLINE, copie las claves del validador a una llave USB.**

Conecte la llave USB a la máquina sin conexión y, a continuación, localice el nombre del dispositivo.

```bash
# Locate the usb key
sudo fdisk -l
```

Al ejecutar el comando anterior, obtendrá una salida similar a la que se muestra a continuación:

```bash
Disk /dev/sdc: 7.4 GiB, 7948206080 bytes, 15523840 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdc1  *     8192 15555555 25555555 7.4G  b W95 FAT32
```

Monta la llave. Cambie el nombre del dispositivo en consecuencia.
```bash
# Create a mount point
sudo mkdir -p /media/usb-drive
# Mount the usb key
sudo mount /dev/sdc1 /media/usb-drive
```

Copie las claves. Ajuste los nombres de las rutas si es necesario.

```bash
# Create a directory on the usb drive to copy the keys into
sudo mkdir -p /media/usb-drive/staking-deposit-cli/validator_keys
# Copy the keys to the usb drive
sudo cp $HOME/staking-deposit-cli/validator_keys/*.json /media/usb-drive/staking-deposit-cli/validator_keys
# Cleanup
sudo umount /media/usb-drive
```

### **Paso 2: Desde una llave USB, copie las claves del validador en el NODO.**

Conecte la llave USB al nodo y, a continuación, localice el nombre del dispositivo.

```bash
# Locate the usb key
sudo fdisk -l
```

Al ejecutar el comando anterior, obtendrá una salida similar a la que se muestra a continuación:

```bash
Disk /dev/sdc: 7.4 GiB, 7948206080 bytes, 15523840 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x00000000

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdc1  *     8192 15555555 25555555 7.4G  b W95 FAT32
```

Monta la llave. Cambie el nombre del dispositivo en consecuencia.

```bash
# Create a mount point
sudo mkdir -p /media/usb-drive
# Mount the usb key
sudo mount /dev/sdc1 /media/usb-drive
```

Copie las claves. Ajuste los nombres de las rutas si es necesario.
|
```bash
# Create a directory copy the keys into
sudo mkdir -p $HOME/staking-deposit-cli/validator_keys
# Copy the keys to the default path
sudo cp /media/usb-drive/staking-deposit-cli/validator_keys/*.json $HOME/staking-deposit-cli/validator_keys
# Cleanup
sudo umount /media/usb-drive
```

</details>

Después de transferir los archivos, verifique que los archivos keystore-m estén en la ubicación correcta en su nodo.

```bash
ls -l $HOME/staking-deposit-cli/validator_keys
```

Salida de muestra esperada:

```bash
-r--r----- 1 ethereum ethereum 706 Oct  1 02:33 deposit_data-1696645983.json
-r--r----- 1 ethereum ethereum 710 Oct  1 02:33 keystore-m_12381_3600_0_0_0-161664283.json
```

## :woman\_technologist: 4. Transacciones de depósito en el Launchpad

1. Siga el tutorial en el Launchpad: [https://holesky.launchpad.ethstaker.cc](https://holesky.launchpad.ethstaker.cc)

{% hint style="danger" %}
**¡No envíe ETH real de la red principal durante este proceso!** :octagonal\_sign: Usa solo Holesky ETH.
{% endhint %}

2. Sube lo que encuentres en el directorio.`deposit_data-#########.json` `validator_keys`.
3. Conecte la plataforma de lanzamiento con su billetera, revise y acepte los términos. Asegúrate de estar conectado a la red **de Holešky**.

{% hint style="info" %}
:whale: **Consejo de depósito por lotes**: Si tiene muchos depósitos para hacer para muchos validadores, considere usar [la herramienta eth2depositor de Abyss.finance.](https://abyss.finance/eth2depositor) Esto mejora en gran medida la experiencia de depósito, ya que se pueden agrupar varios depósitos en una sola transacción, lo que ahorra tarifas de gas y ahorra sus dedos al minimizar los clics en Metamask.

En el cuadro desplegable de la herramienta, seleccione **Red de Holešky**.

Fuente: [https://twitter.com/AbyssFinance/status/1379732382044069888](https://twitter.com/AbyssFinance/status/1379732382044069888)
{% endhint %}

4. Confirme la(s) transacción(es). Hay una transacción de depósito de 32 ETH para cada validador.

* **Ejemplo de depósito**: Si desea ejecutar 3 validadores, deberá tener (32 x 3) = 96 Holesky ETH más algo adicional para cubrir las tarifas de gas.
* **Verificar el contrato de depósito:** Su transacción es depositar su ETH en la dirección del contrato de depósito de Holesky. **Verifique**, _verifique dos veces, _**verifique tres veces**_ que la dirección del contrato de depósito de Holesky sea correcta. [`0x4242424242424242424242424242424242424242`](https://holesky.beaconcha.in/address/4242424242424242424242424242424242424242)
* **Usuarios de la billetera Ledger Nano Hardware**: Si tienen dificultades para realizar la transacción de depósito, habiliten la firma a ciegas y los datos del contrato.

## 4. Revisa las copias de seguridad

{% hint style="danger" %}
:fire: **Recordatorio crítico de criptomonedas:** **Mantenga sus mnemotecnias, mantenga su ETH.**

* **Mantente desconectado**: Escribe tu semilla mnemotécnica **sin conexión**. No el correo electrónico. No en la nube.
* **Más de 1 copia de seguridad de mnemotécnico**: Varias copias con varias ubicaciones es mejor. Es mejor almacenarlo en una [semilla de metal.](https://jlopp.github.io/metal-bitcoin-storage-reviews/)
* **Verifica la copia de seguridad de tu billetera de hardware:** Lo más importante de todos los datos es que aquí es donde pertenece su dirección de retiro y, en última instancia, controla los 32 ETH
* **En caso de recuperación**: Almacenado en una llave USB, guarde copias de
  * `validator_keys directory` - Contiene todos los archivos de .json del almacén de claves
  * Contraseña del almacén de claves: se utiliza para cifrar archivos del almacén de claves
{% endhint %}

#### :tada:¡La configuración de la clave del validador y los depósitos están completos!
