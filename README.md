# Wireguard demo

This demo explains how to install and use Wireguard.

## 1. Server installation
### 1.1. Install Wireguard

First install the Wireguard package on your server

```
sudo apt install wireguard
```

Allow IP forwarding on your server. Change the following values in the file ``/etc/sysctl.conf``:
```
net.ipv6.conf.all.forwarding = 1
net.ipv4.ip_forward = 1
```
Apply your changes: ``sudo sysctl -p``

Configure your firewall to allow Wireguard. We will use port 53222 for Wireguard, but this can be different on your server.
```
sudo ufw allow 53222/udp
sudo ufw enable
```

### 1.2. Configure Wireguard

The keys and config files have to be put inside the folder ``/etc/wireguard``

Generate the server private key
```
cd /etc/wireguard

# Generate Wireguard private key
wg genkey > privatekey

# Make sure nobody can read our key
chmod 600 privatekey
```

Generate Wireguard public key based on the private key
```
wg pubkey < privatekey > publickey
```

Now, create the Wireguard config file: ```/etc/wireguard/wg0.conf```

```
[Interface]
## VPN server private IP address. ##
Address = 192.168.4.1/24

## VPN server port ##
ListenPort = 53222

## VPN server's private key => Read from /etc/wireguard/privatekey. Replace this with your own key! ##
PrivateKey = 0O7sdWfd5YI7O9pASH2eBNKw3AcnpUZ7rE2Y8fEs/n4=
```

Now, start Wireguard:
```
sudo wg-quick up wg0
```

To check the status, run the command ``wg show``

To run Wireguard inside a container, use the following command:
```
sudo docker run -dit --name wireguard-server -p 53222:53222/udp --cap-add=NET_ADMIN --cap-add=SYS_MODULE --sysctl="net.ipv4.conf.all.src_valid_mark=1" --sysctl="net.ipv4.ip_forward=1" wireguard:alpine /bin/sh
```
