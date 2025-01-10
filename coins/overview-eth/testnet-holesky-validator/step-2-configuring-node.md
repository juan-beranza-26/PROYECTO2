# Step 2: Configuring Node

## :hammer\_pick: Node Configuration

### Logging to the node

**Using Ubuntu Server**: Comience por conectarse con su cliente SSH.

```bash
ssh username@staking.node.ip.address
```

**Using Ubuntu Desktop**: Es probable que estés frente a tu **local** nodo. Basta con abrir una ventana de terminal desde cualquier lugar escribiendo Ctrl+Alt+T.

### Updating the node

Asegúrese de que todos los paquetes, herramientas y parches más recientes están instalados y, a continuación, reinicie el sistema.

```bash
sudo apt-get update -y && sudo apt dist-upgrade -y
sudo apt-get install git ufw curl ccze jq -y
sudo apt-get autoremove
sudo apt-get autoclean
sudo reboot
```

## :key: Security Configuration

### Create a non-root user with sudo privileges

<details>

<summary>Creating a user called ethereum</summary>

crear un nuevo usario llamado `ethereum`

```bash
sudo useradd -m -s /bin/bash ethereum
```

Establecer la contraseña para el usuario ethereum


```bash
sudo passwd ethereum
```

Añadir ethereum al grupo sudo

```bash
sudo usermod -aG sudo ethereum
```

Cierre la sesión y vuelva a iniciarla como este nuevo usuario.


**Using Ubuntu Server**: Use the following commands.

```bash
exit
ssh ethereum@staking.node.ip.address
```

**Using Ubuntu Desktop**: Puede cerrar la sesión en la esquina superior derecha, debajo del icono de encendido. Haga clic en la cuenta de usuario `ethereum` e introduzca la contraseña.

</details>

{% hint style="warning" %}
:fire:**Important reminder**: Asegúrese de que ha iniciado sesión y ejecute todos los pasos de esta guía como este usuario no root, `ethereum`.
{% endhint %}

### Hardening SSH Access

{% hint style="info" %}
**Local node**? Puede omitir esta sección sobre el Refuerzo del acceso SSH.
{% endhint %}

<details>

<summary>Creating a new SSH Key</summary>

Create a new SSH key pair on **your client machine (i.e. local laptop)**. Run this on **your client machine,** no remover el nodo. actualizar el comentario con tu correo o un comentario.

```
ssh-keygen -t ed25519 -C "name@email.com"
```

Verás esto a continuación:

```
Generating public/private ed25519 key pair.
Introduzca el archivo en el que desea guardar la clave (/home/<myUserName>/.ssh/id_ed25519):
```

Aquí se le pide que escriba un nombre de archivo en el que guardar la clave privada SSH. Si pulsa Intro, puede utilizar el nombre de archivo predeterminado `id_ed25519`

A continuación, se te pedirá que introduzcas una frase de contraseña.
```
Introduzca la frase de contraseña (vacía si no hay frase de contraseña):
```

:information\_source: A **passphrase** aañade una capa extra de protección a tu clave privada SSH. Cada vez que se conecte a través de SSH a su nodo remoto, introduzca esta frase de contraseña para desbloquear su clave privada SSH.

:fire: Passphrase is highly recommended! No deje esta opción vacía si no desea una frase de contraseña.
:bulb:No olvide ni pierda su frase de contraseña. Guárdela en un gestor de contraseñas.

**Location**: Your SSH key pair is stored in your home directory under `~/.ssh`

**File name:** If your default keyname is`id_ed25519`, then

* your **private SSH key** is `id_ed25519`
* your **public SSH key** is `id_ed25519.pub`

:fire: **IMPORTANT:** Make multiple backup copies of your **private SSH key file** to external storage, such as a USB backup key, for recovery purposes. Also backup your **passphrase**!

Verify the contents of your private SSH key file before moving on.

```
cat ~/.ssh/id_ed25519
```

It should look similar to this example.

```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
QyNTUxOQAAACBAblzWLb7/0o62FZf9YjLPCV4qFhbqiSH3TBvZXBiYNgAAAJCWunkulrp5
LgAAAAtzc2gtZWQyNTUxOQAAACBAblzWLb7/0o62FZf9YjLPCV4qFhbqiSH3TBvZXBiYNg
AAAEAxT+yCmifGWgbFnkauf0HyOAJANhYY5EElEX8fI+M4B0BuXNYtvv/SjrYVl/1iMs8J
XioWFuqJIfdMG9lcGJg2AAAACWV0aDJAZXRoMgECAwQ=
-----END OPENSSH PRIVATE KEY-----
```

</details>

#### Transferring the SSH Public Key to Remote node

<details>

<summary>Option 1: Transferring with ssh-copy-id</summary>

Works with Linux or MacOS. Use option 2 for Windows.

```bash
ssh-copy-id -i ~/.ssh/id_ed25519 ethereum@staking.node.ip.address
```

</details>

<details>

<summary>Option 2: Copying the key manually</summary>

First, begin by obtaining your SSH Public key.

For Linux/Mac,

```
cat ~/.ssh/id_ed25519.pub
```

For Windows,

Open a command prompt (Windows Key + R, then `cmd`, finally press enter).

```
type %USERPROFILE%\.ssh\id_ed25519.pub
```

The output will look similar to the following:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAoc78lv+XDh2znunKXUF/9zBNJrM4Nh67yut9RN14SX name@email.com
```

Copy into your clipboard this output, also known as your public SSH key.

On your **remote node**, run the following:

```
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

First, a directory called **.ssh** is created, then `Nano` is a text editor for editing a special file called **authorized\_keys**

With nano opening the authorized\_keys file, right-click your mouse to paste your public SSH key into this file.

To exit and save, press `Ctrl` + `X`, then `Y`, then`Enter`.

Verify your public SSH key was properly pasted into the file.

```
cat ~/.ssh/authorized_keys
```

</details>

#### Disabling Password Authentication

<details>

<summary>Disabling root login and password based login</summary>

:information\_source: With SSH key authentication enabled, there's still the possibility to connect to your remote node with login and password, a much less secure and brute force-able attack vector.

Login via ssh with your new ethereum user

```
ssh ethereum@staking.node.ip.address
```

Edit the ssh configuration file

```
sudo nano /etc/ssh/sshd_config
```

Locate **PubkeyAuthentication** and update to yes. Delete the # in front.

```
PubkeyAuthentication yes
```

Locate **PasswordAuthentication** and update to no. Delete the # in front.

```
PasswordAuthentication no
```

Locate **PermitRootLogin** and update to prohibit-password. Delete the # in front.

```
PermitRootLogin prohibit-password
```

Locate **PermitEmptyPasswords** and update to no. Delete the # in front.

```
PermitEmptyPassword no
```

To exit and save, press `Ctrl` + `X`, then `Y`, then`Enter`.

Validate the syntax of your new SSH configuration.

```
sudo sshd -t
```

If no errors with the syntax validation, restart the SSH process.

```
sudo systemctl restart sshd
```

Verify the login still works.

```
ssh ethereum@staking.node.ip.address
```

**Optional**: Make logging in easier by updating your local ssh config.

To simplify the ssh command needed to log in to your server, consider updating on your local client machine the `$HOME/myUserName/.ssh/config` file:

```bash
Host ethereum-server
  User ethereum
  HostName <staking.node.ip.address>
  Port 22
```

This will allow you to log in with `ssh ethereum-server` rather than needing to pass through all ssh parameters explicitly.

</details>

### Synchronizing time with Chrony

chrony is an implementation of the Network Time Protocol and helps to keep your computer's time synchronized with NTP.

{% hint style="info" %}
Because the consensus client relies on accurate times to perform attestations and produce blocks, your node's time must be accurate to real NTP time within 0.5 seconds.
{% endhint %}

To install chrony:

```bash
sudo apt-get install chrony -y
```

To see the source of synchronization data.

```
chronyc sources
```

To view the current status of chrony.

```
chronyc tracking
```

### Setting Timezone

To pick your timezone run the following command:

```bash
sudo dpkg-reconfigure tzdata
```

Find your region using the simple text-based GUI.

In the event that you are using national system like India's `IST` select:

```
Asia/Kolkata
```

This will be appropriate for all locales in the country (`IST`, `GMT+0530`).

### Creating the jwtsecret file

A jwtsecret file contains a hexadecimal string that is passed to both Execution Layer client and Consensus Layer clients, and is used to ensure authenticated communications between both clients.

```bash
#store the jwtsecret file at /secrets
sudo mkdir -p /secrets

#create the jwtsecret file
openssl rand -hex 32 | tr -d "\n" | sudo tee /secrets/jwtsecret

#enable read access
sudo chmod 644 /secrets/jwtsecret
```

## :link: Network Configuration

The standard UFW - Uncomplicated firewall can be used to control network access to your node and protect against unwelcome intruders.

### Configure UFW Defaults

By default, deny all incoming traffic and allow outgoing traffic.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Configure SSH Port 22

If your node is remote in the cloud, or at home but on a different headless server, you will need to enable SSH port 22 in order to connect.

```bash
# Allow ssh access for remote node
sudo ufw allow 22/tcp comment 'Allow SSH port'
```

If your node is local at home and you have **keyboard access** to it, it's good practice to deny SSH port 22.

```bash
# Deny ssh access for local node
sudo ufw deny 22/tcp comment 'Deny SSH port'
```

### Allow Execution Client Port 30303

Peering on port 30303, execution clients use this port for communication with other network peers.

```bash
sudo ufw allow 30303 comment 'Allow execution client port'
```

### Allow Consensus Client port

Consensus clients generally use port 9000 for communication with other network peers. Using tcp port 13000 and udp port 12000, Prysm uses a slightly different configuration.

```bash
# Lighthouse, Lodestar, Nimbus, Teku
sudo ufw allow 9000 comment 'Allow consensus client port'

# Lighthouse Quic Port https://lighthouse-blog.sigmaprime.io/Quic.html
sudo ufw allow 9001/udp comment 'Allow lighthouse client quic port'

# Prysm
sudo ufw allow 13000/tcp comment 'Allow consensus client port'
sudo ufw allow 12000/udp comment 'Allow consensus client port'
```

### Enable firewall

Finally, enable the firewall and review the configuration.

```bash
sudo ufw enable
sudo ufw status numbered 
```

Example of ufw status for a remote staking node configured for Prysm consensus client.

> ```csharp
>      To                         Action      From
>      --                         ------      ----
> [ 1] 22/tcp                     ALLOW IN    Anywhere
> [ 2] 9000                       ALLOW IN    Anywhere
> [ 3] 30303                      ALLOW IN    Anywhere
> [ 4] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
> [ 5] 9000 (v6)                  ALLOW IN    Anywhere (v6)
> [ 6] 30303 (v6)                 ALLOW IN    Anywhere (v6)
> ```

### Configure Port Forwarding

**Port Forwarding Tip for Local Stakers at Home:** You'll need to forward ports to your validator.

For optimal connectivity, ensure Port Forwarding is setup for your router. Learn to port forward with guides found at [https://portforward.com/how-to-port-forward](https://portforward.com/how-to-port-forward/)

Verify port forwarding is working with the following.

**Option 1:** From the terminal on staking machine. Choose accordingly to your clients.

```bash
# Lighthouse, Lodestar, Nimbus, Teku
curl https://eth2-client-port-checker.vercel.app/api/checker?ports=30303,9000

# Prysm
curl https://eth2-client-port-checker.vercel.app/api/checker?ports=30303,12000,13000
```

**Result:** Open ports will be shown if reachable from public.

\
**Option 2:** Using the browser

* [https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/)
* or [https://canyouseeme.org](https://canyouseeme.org)

As an example, for Lighthouse, you would verify ports 9000 and 30303 are reachable.

### Optional: Whitelisting Connections

Whitelisting, which means permitting connections from a specific IP, can be setup via the following command.

```bash
sudo ufw allow from <your client machine>
# Example
# sudo ufw allow from 192.168.50.22
```

### :chains: **Install Fail2ban**

{% hint style="info" %}
Fail2ban is an intrusion-prevention system that monitors log files and searches for particular patterns that correspond to a failed login attempt. If a certain number of failed logins are detected from a specific IP address (within a specified amount of time), fail2ban blocks access from that IP address.
{% endhint %}

To install fail2ban:

```bash
sudo apt-get install fail2ban -y
```

Edit a config file that monitors SSH logins.

```bash
sudo nano /etc/fail2ban/jail.local
```

Add the following lines to the bottom of the file.

```bash
[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
```

To exit and save, press `Ctrl` + `X`, then `Y`, then`Enter`.

Restart fail2ban for settings to take effect.

```bash
sudo systemctl restart fail2ban
```
