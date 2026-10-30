# 1. Requirements

- An Ubuntu 22.04 or 24.04 server (VM, cloud VPS or home server) with `sudo` access
- A public IP or a domain name pointing to the server
- UDP port `51820` reachable from the internet (open it in the cloud firewall or forward it on your router)
- A client: a Linux machine or a Windows PC

Find the name of the server's public network interface, you will need it later:

```bash
ip -o -4 route show to default | awk '{print $5}'
```

The output is usually something like `eth0`, `ens3` or `enp0s3`. This guide uses `eth0`; replace it with yours.

Next: [Server setup](02-server-setup.md)
