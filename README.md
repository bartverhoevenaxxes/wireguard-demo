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

Configure your firewall to allow Wireguard:
```
sudo ufw allow 51820/udp
sudo ufw enable
```

### 1.2. Configure Wireguard

Generate Wireguard public and private key => Location: /etc/wireguard

``
cd /etc/wireguard


# Generate Wireguard private key
wg genkey > privatekey

# Generate Wireguard public key based on private key
wg pubkey < privatekey > publickey

# Create a new config file: /etc/wireguard/wg0.conf


[Interface]
## VPN server private IP address ##
Address = 192.168.4.1/24

## VPN server port ##
ListenPort = 53222

## VPN server's private key => Read from /etc/wireguard/privatekey ##
PrivateKey = 0O7sdWfd5YI7O9pASH2eBNKw3AcnpUZ7rE2Y8fEs/n4=
Wireguard demo


sudo docker run -dit --name wireguard-server -p 52333:52333/udp --cap-add=NET_ADMIN --cap-add=SYS_MODULE --sysctl="net.ipv4.conf.all.src_valid_mark=1" --sysctl="net.ipv4.ip_forward=1" wireguard:alpine /bin/sh
