# bootnode

```bash

apt-get update
apt-get upgrade -y
apt-get install build-essential

wget https://dl.google.com/go/go1.13.3.linux-amd64.tar.gz
sudo tar -xvf go1.13.3.linux-amd64.tar.gz
sudo mv go /usr/local

nano ~/.profile

# Add the lines below to the bottom of the file
export GOROOT=/usr/local/go
export GOPATH=$ROOT
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

# Exit nano with CRTL+X (Make sure you save) - then exit your SSH session - and re-open it

git clone https://github.com/Ether1Project/Ether1 && git clone https://github.com/xero-official/go-xerom && cd Ether1 && make && cd && cd go-xerom && make && cd

nano /etc/systemd/system/ether1node.service

# Copy and paste the following into the file - remember to replace <name> with your node name

[Unit]
Description=Ether-1 Boot Node
After=network.target

[Service]
User=root
Group=root
Type=simple
Restart=always
ExecStart=/root/Ether1/build/bin/geth --cache=512 -ethstats "<name>:ether1stats@stats.ether1.org" --syncmode "full" --lightserv 75 --lightpeers 100

[Install]
WantedBy=default.target

# Exit nano - save your changes!

nano /etc/systemd/system/xeronode.service

# Copy and paste the following into the file - remember to replace <name> with your node name

[Unit]
Description=Xerom Boot Node
After=network.target

[Service]
User=root
Group=root
Type=simple
Restart=always
ExecStart=/root/go-xerom/build/bin/geth --cache=512 -ethstats "<name>:dec@stats.xerom.org:3000" --syncmode "full" --lightserv 75 --lightpeers 100 --port 30307

[Install]
WantedBy=default.target

# Exit nano - save your changes!

systemctl enable ether1node && systemctl enable xeronode

systemctl start ether1node && systemctl start xeronode

# Now run the below to get Xerom's ENODE ID

/root/go-xerom/build/bin/geth --exec "admin.nodeInfo" attach ipc://./$HOME/.xerom/geth.ipc

# Now run the below to get Ether-1's ENODE ID

/root/Ether1/build/bin/geth --exec "admin.nodeInfo" attach ipc://./$HOME/.ether1/geth.ipc

# Send your ENODE ID & Public IP to Fallen or James. 

