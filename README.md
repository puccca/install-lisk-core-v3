## How to install lisk-core v3.1.0 with npm on testnet network

## Install prerequisites
```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get autoremove
sudo apt-get -y install wget tar zip unzip ufw npm git curl bash jq nodejs
sudo apt install -y libtool automake autoconf curl build-essential python2-minimal
```

## Firewall
```shell
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
**Lisk ports**
```shell
sudo ufw allow "7667/tcp"
sudo ufw allow "7887/tcp"
```
**SSH port**
```shell
sudo ufw allow "YOUR_SSH_PORT/tcp"
```

**Activate firewall**
```shell
sudo ufw --force enable
sudo ufw status
```

*Test a new connection after a system restart*
```shell
sudo shutdown -r now
```

## Install nvm
```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```

*Disconnect & make a new connection*

```shell
nvm install 18.17.1
```

## Install lisk-core v3.1.0 with npm
```shell
npm install lisk-core@3.1.0-rc.0 -g
```

*Check lisk-core location*
```shell
npm list --global --depth=0
```

## Install pm2
```shell
npm i -g pm2
```

## Start lisk core
```shell
lisk-core start --network testnet
```

## Create a new file
```shell
cd && nano core-start.json
```

**Paste the following lines and save with CTRL+X+Y+Enter**

```shell
{
  "name": "lisk-core",
  "script": "lisk-core start --api-ws -c ~/.lisk/lisk-core/config/testnet/config.json",
  "env": {
  "LISK_NETWORK": "testnet"
  }
 }
```

## Start & Stop lisk core
```shell
pm2 start core-start.json
pm2 logs
pm2 stop all
```

## Import from snapshot
```shell
lisk-core blockchain:download --network testnet
lisk-core blockchain:import blockchain.db.tar.gz --force
```

## Start core
```shell
pm2 start core-start.json
pm2 logs
```
