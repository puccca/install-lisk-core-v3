## 10 steps to install lisk-core v3.1.0-rc.1 with npm on testnet network

System requirements:
- Ubuntu 20.04 LTS
- 4GB RAM
- Some basic configuration to secure the server (sudo user, public & private keys, change ssh port to access the server etc.)

## 1. Install prerequisites
```shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
sudo apt-get autoremove
sudo apt-get -y install wget tar zip unzip ufw npm git curl bash jq nodejs
sudo apt install -y libtool automake autoconf curl build-essential python2-minimal
```

## 2. Firewall
```shell
sudo ufw default deny incoming
sudo ufw default allow outgoing
```
**Lisk testnet ports: 7001 for core v3 and 7667 & 7887 for the migration to core v4**
```shell
sudo ufw allow "7001/tcp"
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

## 3. Install nvm
```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
```
*Disconnect & make a new connection*
```shell
nvm install 18
nvm alias default 18
nvm use default
```

## 4. Install lisk-core v3.1.0-rc.1 with npm
```shell
npm install lisk-core@3.1.0-rc.1 -g
```
*Check lisk-core location*
```shell
npm list --global --depth=0
```

## 5. Install pm2 & pm2-logrotate
```shell
npm i -g pm2
pm2 install pm2-logrotate
pm2 set pm2-logrotate:max_size 100M
```

## 6. Start lisk core
```shell
lisk-core start --network testnet
```

## 7. Create a new file
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

## 8. Start & Stop lisk-core
```shell
pm2 start core-start.json
pm2 logs
pm2 stop all
```

## 9. Import from snapshot
```shell
lisk-core blockchain:download --network testnet
lisk-core blockchain:import blockchain.db.tar.gz --force
```

## 10. Start lisk-core
```shell
pm2 start core-start.json
pm2 logs
```

## P. S.
This tutorial couldn't be possible without valuable advices and docs from gr33ndrag0n & punkrock & przemer.
