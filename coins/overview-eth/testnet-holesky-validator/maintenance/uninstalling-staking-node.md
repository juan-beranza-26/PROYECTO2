# Desinstalación del Nodo de Replanteo

{% hint style="info" %}
Ya sea para cambiar de cliente por motivos de diversidad de clientes, trasladarse a un nuevo nodo o retirar un nodo de replanteo, a continuación se explica cómo desinstalar los tres componentes clave de un nodo de replanteo.
{% endhint %}

### Desinstalación del cliente de ejecución&#x20;

```bash
sudo systemctl parar ejecucion 
sudo systemctl desactivar ejecucion 
sudo rm /etc/systemd/systema/ejecucion.servicio

#Nethermind
sudo rm -rf /usr/local/bin/nethermind
sudo rm -rf /var/lib/nethermind

#Besu
sudo rm -rf /usr/local/bin/besu
sudo rm -rf /var/lib/besu

#Geth
sudo rm -rf /usr/local/bin/geth
sudo rm -rf /var/lib/geth

#Erigon
sudo rm -rf /usr/local/bin/erigon
sudo rm -rf /var/lib/erigon

#Reth
sudo rm -rf /usr/local/bin/reth
sudo rm -rf /var/lib/reth

sudo userdel execution
```

### Desinstalación del cliente de consenso&#x20;

```bash
sudo systemctl stop consenso
sudo systemctl disable consenso
sudo rm /etc/systemd/system/consenso.servicio

#Lighthouse
sudo rm -rf /usr/local/bin/lighthouse
sudo rm -rf /var/lib/lighthouse

#Lodestar
sudo rm -rf /usr/local/bin/lodestar
sudo rm -rf /var/lib/lodestar

#Teku
sudo rm -rf /usr/local/bin/teku
sudo rm -rf /var/lib/teku

#Nimbus
sudo rm -rf /usr/local/bin/nimbus_beacon_node
sudo rm -rf /var/lib/nimbus

#Prysm
sudo rm -rf /usr/local/bin/beacon-chain
sudo rm -rf /var/lib/prysm

sudo userdel consensus
```

### Desistalacion de validador 

```bash
sudo systemctl stop validator
sudo systemctl disable validator
sudo rm /etc/systemd/system/validator.service

#Lighthouse
sudo rm -rf /var/lib/lighthouse/validators

#Lodestar
sudo rm -rf /var/lib/lodestar/validators

#Teku, if running Standalone Teku Validator
sudo rm -rf /var/lib/teku_validator

#Nimbus, if running standalone Nimbus Validator
sudo rm -rf /var/lib/nimbus_validator
sudo rm -rf /usr/local/bin/nimbus_validator_client

#Prysm
sudo rm -rf /usr/local/bin/validator
sudo rm -rf /var/lib/prysm/validators

sudo userdel validador
```
