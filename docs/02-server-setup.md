# 2. Server setup

## 2.1 Install WireGuard

```bash
sudo apt update
sudo apt install -y wireguard
```

## 2.2 Generate the server keys

```bash
umask 077
wg genkey | sudo tee /etc/wireguard/server.key | wg pubkey | sudo tee /etc/wireguard/server.pub
```

`server.key` is secret. `server.pub` is shared with clients.

## 2.3 Enable IP forwarding

Needed only if clients should reach the internet or LAN through the server.

```bash
echo "net.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/99-wireguard.conf
sudo sysctl --system
```

## 2.4 Write the server config

Create `/etc/wireguard/wg0.conf`:

```ini
[Interface]
Address = 10.8.0.1/24
ListenPort = 51820
PrivateKey = <contents of /etc/wireguard/server.key>

# NAT so clients can use the server's internet connection (replace eth0)
PostUp   = iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# One [Peer] block per client (added in step 3)
```

Protect the file:

```bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

## 2.5 Open the firewall

If you use UFW:

```bash
sudo ufw allow 51820/udp
sudo ufw allow OpenSSH
sudo ufw enable
```

If UFW is active and you use full-tunnel mode, also allow forwarding by setting `DEFAULT_FORWARD_POLICY="ACCEPT"` in `/etc/default/ufw` and running `sudo ufw reload`.

## 2.6 Start the service

```bash
sudo systemctl enable --now wg-quick@wg0
sudo wg show
```

You should see the `wg0` interface listening on port `51820`.

Next: [Client setup](03-client-setup.md)
