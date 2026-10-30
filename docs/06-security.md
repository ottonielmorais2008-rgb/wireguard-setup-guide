# 6. Security checklist

- [ ] Private keys and `.conf` files have permissions `600` and are never committed to Git
- [ ] Each device has its own key pair
- [ ] Each peer's `AllowedIPs` on the server is a single `/32`
- [ ] Only the ports you need (`51820/udp`, SSH) are open
- [ ] Remove peers for lost or retired devices with `wg set wg0 peer <key> remove`
- [ ] Keep the server updated: `sudo apt update && sudo apt upgrade`

Back to the [start](../README.md)
