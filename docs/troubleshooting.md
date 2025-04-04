# Troubleshooting Guide

| Issue                   | Solution                                              |
|-------------------------|-------------------------------------------------------|
| **Connection fails**    | Open UDP port `51820` on the server firewall.         |
| **No internet access**  | Enable IP forwarding: `sysctl -p`.                    |
| **Key mismatch**        | Regenerate and replace keys.                          |
| **QR code won’t scan**  | Ensure `client.conf` has no typos.                    |
| **Slow speeds**         | Check server bandwidth or ISP throttling.             |
