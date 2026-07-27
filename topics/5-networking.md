# 5. Networking

**OSI (7):** Physical → Data Link → Network → Transport → Session → Presentation → Application
**TCP/IP (4):** Link → Internet → Transport → Application

Layer → protocol/device:
- Physical: cables, hubs, repeaters
- Data Link: MAC, switches, bridges, Ethernet, ARP
- Network: IP, ICMP, routers
- Transport: TCP, UDP
- Application: HTTP, FTP, SMTP, DNS, SSH, DHCP

**TCP vs UDP**

| | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed, retransmits | Best-effort |
| Ordering | Yes | No |
| Speed | Slower | Faster |
| Header | 20 bytes | 8 bytes |
| Use | HTTP, email, file transfer | DNS, video streaming, gaming, VoIP |

- **TCP 3-way handshake:** SYN → SYN-ACK → ACK. Teardown is 4-way (FIN/ACK ×2).
- Flow control = sliding window. Congestion control = slow start, AIMD.

**Common ports:** 20/21 FTP, 22 SSH, 23 Telnet, 25 SMTP, 53 DNS, 80 HTTP, 110 POP3, 143 IMAP, 443 HTTPS, 3306 MySQL, 5432 PostgreSQL, 6379 Redis, 27017 MongoDB

**DNS** — resolves domain to IP. Order: browser cache → OS cache → resolver → root → TLD → authoritative. Records: A (IPv4), AAAA (IPv6), CNAME (alias), MX (mail), TXT, NS.

**Other**
- IPv4 = 32-bit, IPv6 = 128-bit. Private ranges: 10.x, 172.16–31.x, 192.168.x.
- Subnet mask splits network vs host portion. CIDR /24 = 256 addresses (254 usable).
- DHCP assigns IPs dynamically. NAT maps private to public addresses. ARP maps IP → MAC.
- Hub (layer 1, broadcasts) vs switch (layer 2, MAC-based) vs router (layer 3, IP-based).
- Latency = delay; bandwidth = capacity; throughput = actual rate.
