# 4. Adding more peers

Each client needs:

- its own key pair (never reuse keys between devices)
- a unique tunnel IP, for example `10.8.0.3`, `10.8.0.4`, ...

| Device   | Tunnel IP   | AllowedIPs on server |
|----------|-------------|----------------------|
| client1  | 10.8.0.2    | 10.8.0.2/32          |
| client2  | 10.8.0.3    | 10.8.0.3/32          |
| phone    | 10.8.0.4    | 10.8.0.4/32          |

Add a peer without dropping existing connections:

```bash
sudo wg set wg0 peer <CLIENT_PUBLIC_KEY> allowed-ips 10.8.0.3/32
```

`wg set` changes only the running interface. Also add the same `[Peer]` block to `/etc/wireguard/wg0.conf` so it survives a restart.

Remove a peer:

```bash
sudo wg set wg0 peer <CLIENT_PUBLIC_KEY> remove
```

Next: [Troubleshooting](05-troubleshooting.md)
