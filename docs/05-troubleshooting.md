# 5. Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `wg-quick up` fails with "iptables: command not found" | `iptables` not installed (Ubuntu 24.04 minimal) | `sudo apt install -y iptables` |
| No "latest handshake" in `sudo wg show` | UDP 51820 blocked | Open the port in UFW, the cloud firewall and/or the router |
| Handshake OK but no internet on client | IP forwarding or NAT missing | Check `sysctl net.ipv4.ip_forward` is `1` and the `PostUp` interface name is correct |
| Handshake OK, internet fails with UFW on | UFW drops forwarded packets | Set `DEFAULT_FORWARD_POLICY="ACCEPT"` and `sudo ufw reload` |
| Websites don't load, ping to IPs works | DNS | Set `DNS = 1.1.1.1` in the client `[Interface]` |
| Connection drops behind NAT | NAT timeout | Add `PersistentKeepalive = 25` on the client |
| `wg-quick up` fails with "resolvconf: command not found" | Missing DNS helper on Linux client | On Ubuntu with `systemd-resolved` (22.04): `sudo ln -s /usr/bin/resolvectl /usr/local/bin/resolvconf`. Otherwise `sudo apt install -y openresolv`, or remove the `DNS` line |

Useful commands:

```bash
sudo systemctl status wg-quick@wg0
sudo journalctl -u wg-quick@wg0 --no-pager | tail -n 50
sudo wg show
ip a show wg0
```

Next: [Security checklist](06-security.md)
