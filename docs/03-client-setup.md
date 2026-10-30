# 3. Client setup

## 3.1 Create the client keys

On a Linux client (or on the server, then copy the config over securely):

```bash
umask 077
wg genkey | tee client1.key | wg pubkey > client1.pub
```

On Windows, the WireGuard app generates the keys for you: **Add Tunnel > Add empty tunnel**.

## 3.2 Client config

```ini
[Interface]
PrivateKey = <contents of client1.key>
Address = 10.8.0.2/24
DNS = 1.1.1.1

[Peer]
PublicKey = <contents of server.pub>
Endpoint = <SERVER_PUBLIC_IP>:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

- `AllowedIPs = 0.0.0.0/0` sends **all** traffic through the VPN (full tunnel).
- Use `AllowedIPs = 10.8.0.0/24` instead to reach only the VPN network (split tunnel).

## 3.3 Register the client on the server

Add this block to `/etc/wireguard/wg0.conf` on the server:

```ini
[Peer]
PublicKey = <contents of client1.pub>
AllowedIPs = 10.8.0.2/32
```

Restart the interface:

```bash
sudo systemctl restart wg-quick@wg0
```

## 3.4 Connect

**Linux**

```bash
sudo apt install -y wireguard
sudo cp client1.conf /etc/wireguard/wg0.conf
sudo wg-quick up wg0
```

**Windows**: open the WireGuard app, **Import tunnel(s) from file**, select the `.conf` file and click **Activate**.

## 3.5 Verify

```bash
ping 10.8.0.1          # reach the server through the tunnel
sudo wg show           # "latest handshake" should be a few seconds ago
curl https://ifconfig.me   # in full-tunnel mode, shows the server's public IP
```

Next: [Adding more peers](04-adding-peers.md)
