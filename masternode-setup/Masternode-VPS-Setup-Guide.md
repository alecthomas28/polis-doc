## Preperation

1. Get a VPS from a provider like DigitalOcean, Vultr, Linode, etc. 
   - Recommended VPS size at least 1gb RAM 
   - **It must be Ubuntu 16.04 (Xenial)**
2. Make sure you have a transaction of exactly 1000 POLIS in your desktop cold wallet.

## Cold Wallet Setup Part 1

1. Open your wallet on your desktop.

   Click Settings -> Options -> Wallet
   
   Check "Enable coin control features"
   
   Check "Show Masternodes Tab"
   
   Press Ok (you will need to restart the wallet)
   
   ![Alt text](https://github.com/digitalmine/Guide/blob/master/poliswalletsettings.png "Wallet Settings")

   
   
   
2. Go to the "Tools" -> "Debug console"
3. Run the following command: `masternode genkey`
4. You should see a long key that looks like:
```
3HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg
```
5. This is your `private key`, keep it safe, do not share with anyone.




## VPS Setup

1. Log into your VPS
   - Windows users [follow this guide](https://www.digitalocean.com/community/tutorials/how-to-log-into-your-droplet-with-putty-for-windows-users) to log into your VPS.
2. Copy/paste these commands into the VPS and hit enter: (One Box At A Time)
```
apt-get -y update
```
```
apt-get -y upgrade
```
```
apt-get -y install software-properties-common
```
```
apt-add-repository -y ppa:bitcoin/bitcoin
```
```
apt-get -y update
```
```
apt-get -y install \
    wget \
    git \
    unzip \
    libevent-dev \
    libboost-dev \
    libboost-chrono-dev \
    libboost-filesystem-dev \
    libboost-program-options-dev \
    libboost-system-dev \
    libboost-test-dev \
    libboost-thread-dev \
    libdb4.8-dev \
    libdb4.8++-dev \
    libminiupnpc-dev \
    build-essential \
    libtool \
    autotools-dev \
    automake \
    pkg-config \
    libssl-dev \
    libevent-dev \
    bsdmainutils \
    libzmq3-dev \
    nano
```
```
wget https://github.com/polispay/polis/releases/download/v1.2.0/poliscore-1.2.0-linux.zip
```
```
unzip poliscore-1.2.0-linux.zip
```
```
rm poliscore-1.2.0-linux.zip
```
```
cp poliscore-1.2.0-linux/usr/local/bin/polis{d,-cli} /usr/local/bin
```
```
cd
```
```
mkdir -p .poliscore
```
```
nano .poliscore/polis.conf
```
```
rpcuser=randuser43897ty8943
rpcpassword=passhf95uiygr5308h08r3h0249fbgh7389h973
rpcallowip=127.0.0.1
listen=1
server=1
daemon=0
logtimestamps=1
maxconnections=256
externalip=VPS_IP_ADDRESS
masternodeprivkey=WALLET_GENKEY
masternode=1
connect=35.227.49.86:24126
connect=192.243.103.182:24126
connect=185.153.231.146:24126
connect=91.223.147.100:24126
connect=96.43.143.93:24126
connect=104.236.147.210:24126
connect=159.89.137.114:24126
connect=159.89.139.41:24126
connect=174.138.70.155:24126
connect=174.138.70.16:24126
connect=45.55.247.25:24126
```
CTRL X - To Save
```
polisd &
```
```
apt-get -y install virtualenv python-pip
```
```
git clone https://github.com/polispay/sentinel /root/sentinel
```
```
cd /root/sentinel
```
```
virtualenv venv
```
```
. venv/bin/activate
```
```
pip install -r requirements.txt
```
```
crontab -e
```
```
* * * * * cd /root/sentinel && ./venv/bin/python bin/sentinel.py >/dev/null 2>&1
```

3.Use `watch polis-cli getinfo` to check and wait til it's synced 
  (look for blocks number and compare with block explorer http://block.polispay.org/ )


## Cold Wallet Setup Part 2 

1. On your local machine open your `masternode.conf` file.
   Depending on your operating system you will find it in:
   * Windows: `%APPDATA%\polisCore\`
   * Mac OS: `~/Library/Application Support/polisCore/`
   * Unix/Linux: `~/.poliscore/`
   
   Leave the file open
2. Go to "Tools" -> "Debug console"
3. Run the following command: `masternode outputs`
4. You should see output like the following if you have a transaction with exactly 1000 POLIS:
```
{
    "12345678xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx": "0"
}
```
5. The value on the left is your `txid` and the right is the `vout`
6. Add a line to the bottom of the already opened `masternode.conf` file using the IP of your
VPS (with port 24126), `private key`, `txid` and `vout`:
```
mn1 1.2.3.4:24126 3xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 12345678xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx 0 
```
7. Save the file, exit your wallet and reopen your wallet.
8. Go to the "Masternodes" tab
9. Click "Start All"
10. You will see "WATCHDOG_EXPIRED". Just wait few minutes

Cheers !
