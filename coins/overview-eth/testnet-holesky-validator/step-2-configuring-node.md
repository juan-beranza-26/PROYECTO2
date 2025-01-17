# Paso 2: Configurar el nodo

## :hammer\_pick: Configuración del nodo

### Iniciar sesión en el nodo

**Usando Ubuntu Server**: Comience a conectarse con su cliente SSH.

```golpecito
ssh nombre de usuario@stake.node.ip.address
```

**Usando Ubuntu Desktop**: Es probable que estés frente a tu nodo **local**. Basta con abrir una ventana de terminal desde cualquier lugar escribiendo Ctrl+Alt+T.

### Actualizando el nodo

Asegúrese de que todos los paquetes, herramientas y parches más recientes estén instalados y, a continuación, reinicie el sistema.

```bash
sudo apt-get update -y && sudo apt dist-upgrade -y
sudo apt-get install git ufw curl ccze jq -y
sudo apt-get autoremove
sudo apt-get autoclean
sudo reboot
```

## :key : Configuración de seguridad

### Crear un usuario no root con privilegios sudo

<details>

<summary>Crear un usuario llamado ethereum</summary>

crear un nuevo usuario llamado `ethereum`

```bash
sudo useradd -m -s /bin /bash ethereum
```

Establecer la contraseña para el usuario ethereum

```bash
sudo passwd ethereum
```

Añadir ethereum al grupo sudo

```bash
sudo usermod -aG sudo etéreo
```

Cierra la sesión y vuelve a iniciarla como este nuevo usuario.


**Usando Ubuntu Server**: Utilice los siguientes comandos.

```golpecito
salida
ssh ethereum@s Taking.node.ip.address
```

**Usando Ubuntu Desktop**: Puede cerrar la sesión en la esquina superior derecha, debajo del icono de encendido. Haga clic en la cuenta de usuario `ethereum` e introduzca la contraseña.

</detalles>

{% estilo sugerencia="advertencia" %}
:fire:**Recordatorio importante**: Asegúrese de que ha iniciado sesión y ejecute todos los pasos de esta guía como este usuario no root, `ethereum`.
{% final %}

### Refuerzo del acceso SSH

{% sugerencia estilo="info" %}
**¿Nodo local**? Puede omitir esta sección sobre el Refuerzo del acceso SSH.
{% final %}

<detalles>

<summary>Crear una nueva clave SSH</summary>

Cree un nuevo par de claves SSH en **su máquina cliente (es decir, su computadora portátil local)**. Ejecute esto en **su máquina cliente,** sin remover el nodo. Actualiza el comentario con tu correo o un comentario.

```
ssh-keygen -t ed25519 -C "nombre@correo electrónico.com"
```

Verás esto a continuación:

```
Generando par de claves públicas/privadas ed25519.
Introduzca el archivo en el que desea guardar la clave (/home/<myUserName>/.ssh/id_ed25519):
```

Aquí se le pide que escriba un nombre de archivo en el que guardar la clave privada SSH. Si pulsa Introducción, puede utilizar el nombre de archivo predeterminado `id_ed25519`

A continuación, se te pedirá que introduzcas una frase de contraseña.
```
Introduzca la frase de contraseña (vacía si no hay frase de contraseña):
```

:information\_source: A **passphrase** añade una capa extra de protección a tu clave privada SSH. Cada vez que se conecte a través de SSH a su nodo remoto, introduzca esta frase de contraseña para desbloquear su clave privada SSH.

:fire: ¡La frase de contraseña es muy recomendable! No deje esta opción vacía si no desea una frase de contraseña.
:bulb:No olvide ni perder su frase de contraseña. Guárdela en un gestor de contraseñas.

**Ubicación**: Su par de claves SSH se almacena en su directorio de inicio en `~/.ssh`

**Nombre de archivo:** Si su nombre de clave predeterminado es `id_ed25519`, entonces

* su **clave SSH privada** es `id_ed25519`
* su **clave SSH pública** es `id_ed25519.pub`

:fire: **IMPORTANTE:** Realice varias copias de seguridad de su **archivo de clave SSH privada** en un almacenamiento externo, como una copia de seguridad USB clave, para fines de recuperación. ¡También haga una copia de seguridad de su **contraseña**!

Verifique el contenido de su archivo de clave SSH privada antes de continuar.

```
cat ~/.ssh/id_ed25519
```

Debería verse similar a este ejemplo.

` ``
-----INICIO OPENSSH PRIVADO CLAVE-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBAblzWLb7/0o62FZf9YjLPCV4qFhbqiSH3TBvZXBiYNgAAAJCWunkulrp5
LgAAAAtzc2gtZWQyNTUxOQ AAACBAblzWLb7/0o62FZf9YjLPCV4qFhbqiSH3TBvZXBiYNg
AAAEAxT+yCmifGWgbFnkauf0HyOAJANhYY5EElEX8fI+M4B0BuXNYtvv/SjrYVl/1iMs8J
XioWFuqJIfdMG9lcGJg2AAAACWV0aDJAZXRoMgECAwQ=
-----END OPENSSH PRIVATE KEY-----
```

</details>

#### Transferencia de la clave pública SSH al nodo remoto

<details> <summary>Opción 1: Transferencia con ssh-copy-id</summary>

Funciona con Linux o MacOS. Utilice la opción 2 para Windows.

```bash
ssh-copy-id -i ~/.ssh/id_ed25519 ethereum@staking.node.ip.address
```

</details>

<details>

<summary>Opción 2: Copiar la clave manualmente</summary> Primero, comience por obtener su clave pública SSH.

Para Linux/Mac,

```
cat ~/.ssh/id_ed25519.pub
```

Para Windows,

Abra un símbolo del sistema (tecla Windows + R, luego `cmd`, finalmente presione ingresar).

```
type %USERPROFILE%\.ssh\id_ed25519.pub
```

El resultado será similar al siguiente:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAoc78lv+XDh2znunKXUF/9zBNJrM4Nh67yut9RN14SX name@email.com
```

Copia en tu portapapeles esto salida, también conocida como su clave SSH pública.

On your **remote node**, run the following:

```
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Primero, se crea un directorio llamado **.ssh**, luego `Nano` es un editor de texto para editar un archivo especial llamado **authorized\_keys**

Con nano abriendo el archivo authorized\_keys, haga clic derecho con el mouse para pegar su clave SSH pública en este archivo.

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

Verifique que su clave SSH pública se haya pegado correctamente en el archivo.

```
cat ~/.ssh/authorized_keys
```

</details>

#### Deshabilitar la autenticación de contraseña

<details>

<summary>Deshabilitar el inicio de sesión raíz y el inicio de sesión basado en contraseña</summary>

:information\_source: Con la autenticación de clave SSH habilitada, aún existe la posibilidad de conectarse a su nodo remoto con nombre de usuario y contraseña, un vector de ataque mucho menos seguro y susceptible de ser atacado por fuerza bruta.

Inicia sesión a través de ssh con tu nuevo usuario de Ethereum

```
ssh ethereum@staking.node.ip.address
```

Edita el archivo de configuración de ssh

```
sudo nano /etc/ssh/sshd_config
```

Ubica **PubkeyAuthentication** y actualiza a yes. Elimina el # que está al frente.

```
PubkeyAuthentication yes
```

Ubica **PasswordAuthentication** y actualiza a no. Elimina el # que está al frente.

```
PasswordAuthentication no
```

Ubica **PermitRootLogin** y actualiza a prohibit-password. Elimina el # que está al frente.

```
PermitRootLogin prohibit-password
```

Ubica **PermitEmptyPasswords** y actualiza a no. Elimina el # que está al frente.

```
PermitEmptyPassword no
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

Valide la sintaxis de su nueva configuración SSH.

```
sudo sshd -t
```

Si no hay errores con la validación de sintaxis, reinicie el proceso SSH.

```
sudo systemctl restart sshd
```

Verifique que el inicio de sesión aún funciona.

```
ssh ethereum@staking.node.ip.address
```

**Opcional**: facilite el inicio de sesión actualizando su configuración SSH local.

Para simplificar el comando ssh necesario para iniciar sesión en su servidor, considere actualizar en su máquina cliente local el archivo `$HOME/myUserName/.ssh/config`:

```bash
Host ethereum-server
User ethereum
HostName <staking.node.ip.address>
Port 22
```

Esto le permitirá iniciar sesión con `ssh ethereum-server` en lugar de tener que pasar todos los parámetros ssh explícitamente.

</details>

### Sincronización de la hora con Chrony

chrony es una implementación del Protocolo de tiempo de red y ayuda a mantener la hora de su computadora sincronizada con NTP.

{% hint style="info" %}
Dado que el cliente de consenso depende de horas precisas para realizar atestaciones y producir bloques, la hora de su nodo debe ser precisa con respecto a la hora NTP real dentro de los 0,5 segundos.
{% endhint %}

Para instalar chrony:

```bash
sudo apt-get install chrony -y
```

Para ver la fuente de los datos de sincronización.

```
chronyc source
```

Para ver el estado actual de chrony.

```
chronyc tracking
```

### Configuración de la zona horaria

Para elegir su zona horaria, ejecute el siguiente comando:

```bash
sudo dpkg-reconfigure tzdata
```

Busque su región utilizando la sencilla interfaz gráfica de usuario basada en texto.

En caso de que esté utilizando un sistema nacional como el `IST` de la India, seleccione:

```
Asia/Kolkata
```

Esto será apropiado para todas las configuraciones regionales del país (`IST`, `GMT+0530`).

### Creación del archivo jwtsecret

Un archivo jwtsecret contiene una cadena hexadecimal que se pasa tanto al cliente de la capa de ejecución como a los clientes de la capa de consenso, y se utiliza para garantizar las comunicaciones autenticadas entre ambos clientes.

```bash
#Almacene el archivo jwtsecret en /secrets
sudo mkdir -p /secrets

#Cree el archivo jwtsecret
openssl rand -hex 32 | tr -d "\n" | sudo tee /secrets/jwtsecret

#Habilite el acceso de lectura
sudo chmod 644 /secrets/jwtsecret
```

## :link: Configuración de red

El firewall estándar UFW - Uncomplicated se puede utilizar para controlar el acceso de red a su nodo y protegerse contra intrusos no deseados.

### Configurar los valores predeterminados de UFW

De manera predeterminada, deniega todo el tráfico entrante y permite el tráfico saliente.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Configurar el puerto SSH 22

Si su nodo está remoto en la nube, o en su hogar pero en un servidor sin interfaz gráfica diferente, deberá habilitar el puerto SSH 22 para poder conectarse.

```bash
# Permitir acceso ssh para nodos remotos
sudo ufw allow 22/tcp comment 'Permitir puerto SSH'
```

Si su nodo es local en su hogar y tiene **acceso mediante teclado**, es una buena práctica denegar el puerto SSH 22.

```bash
# Denegar acceso ssh para nodos locales
sudo ufw deny 22/tcp comment 'Denegar puerto SSH'
```

### Permitir puerto de cliente de ejecución 30303

Al conectarse en el puerto 30303, los clientes de ejecución usan este puerto para comunicarse con otros pares de la red.

```bash
sudo ufw allow 30303 comment 'Permitir puerto de cliente de ejecución'
```

### Permitir puerto de cliente de consenso

Los clientes de consenso generalmente usan el puerto 9000 para comunicarse con otros pares de la red. Al utilizar el puerto TCP 13000 y el puerto UDP 12000, Prysm utiliza una configuración ligeramente diferente.

```bash
# Lighthouse, Lodestar, Nimbus, Teku
sudo ufw allow 9000 comment 'Allow consensus client port'

# Lighthouse Quic Port https://lighthouse-blog.sigmaprime.io/Quic.html
sudo ufw allow 9001/udp comment 'Allow lighthouse client quic port'

# Prysm
sudo ufw allow 13000/tcp comment 'Allow consensus client port'
sudo ufw allow 12000/udp comment 'Allow consensus client port'
```

### Habilitar el firewall

Por último, habilite el firewall y revise la configuración.

```bash
sudo ufw enable
sudo ufw status numbered
```

Ejemplo de estado de ufw para un nodo de participación remoto configurado para el cliente de consenso Prysm.

> ```csharp
> A la acción desde
> -- ------ ----
> [ 1] 22/tcp PERMITIR EN CUALQUIER LUGAR
> [ 2] 9000 PERMITIR EN CUALQUIER LUGAR
> [ 3] 30303 PERMITIR EN CUALQUIER LUGAR
> [ 4] 22/tcp (v6) PERMITIR EN CUALQUIER LUGAR (v6)
> [ 5] 9000 (v6) PERMITIR EN CUALQUIER LUGAR (v6)
> [ 6] 30303 (v6) PERMITIR EN CUALQUIER LUGAR (v6)
> ```

### Configurar el reenvío de puertos

**Sugerencia de reenvío de puertos para participantes locales en casa:** Deberá reenviar puertos a su validador.

Para una conectividad óptima, asegúrese de que el reenvío de puertos esté configurado para su enrutador. Aprenda a reenviar puertos con las guías que se encuentran en [https://portforward.com/how-to-port-forward](https://portforward.com/how-to-port-forward/)

Verifique que el reenvío de puertos esté funcionando con lo siguiente.

**Opción 1:** Desde la terminal en la máquina de staking. Elija según sus clientes.

```bash
# Lighthouse, Lodestar, Nimbus, Teku
curl https://eth2-client-port-checker.vercel.app/api/checker?ports=30303,9000

# Prysm
curl https://eth2-client-port-checker.vercel.app/api/checker?ports=30303,12000,13000
```

**Resultado:** Se mostrarán los puertos abiertos si se puede acceder a ellos desde el público.

\
**Opción 2:** Usar el navegador

* [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/)
* o [https://canyouseeme.org](https://canyouseeme.org)

Por ejemplo, para Lighthouse, deberá verificar que los puertos 9000 y 30303 sean accesibles.

### Opcional: incluir conexiones en la lista blanca

La inclusión en la lista blanca, que significa permitir conexiones desde una IP específica, se puede configurar mediante el siguiente comando.

```bash
sudo ufw allow from <your client machine>
# Ejemplo
# sudo ufw allow from 192.168.50.22
```

### :chains: **Instalar Fail2ban**

{% hint style="info" %}
Fail2ban es un sistema de prevención de intrusiones que monitorea los archivos de registro y busca patrones particulares que corresponden a un intento de inicio de sesión fallido. Si se detecta una cierta cantidad de inicios de sesión fallidos desde una dirección IP específica (dentro de un período de tiempo especificado), fail2ban bloquea el acceso desde esa dirección IP.
{% endhint %}

Para instalar fail2ban:

```bash
sudo apt-get install fail2ban -y
```

Edite un archivo de configuración que monitoree los inicios de sesión SSH.

```bash
sudo nano /etc/fail2ban/jail.local
```

Agregue las siguientes líneas al final del archivo.

```bash
[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
```

Para salir y guardar, presione `Ctrl` + `X`, luego `Y`, luego `Enter`.

Reinicie fail2ban para que la configuración surta efecto.

```bash
sudo systemctl restart fail2ban
```
