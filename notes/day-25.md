
# Linux Networking Fundamentals: IPv4, IPv6, Routing & DNS

Today I learned the basics of Linux networking, including the TCP/IP model, IP addressing, subnetting, routing, DHCP, IPv6, and DNS. I also learned useful commands to inspect network interfaces and routing tables.

---

# TCP/IP Network Model

The TCP/IP model has four layers:

| Layer | Purpose | Examples |
|---|---|---|
| Application | Provides network services | SSH, HTTP, FTP, DNS |
| Transport | Communication between applications | TCP, UDP |
| Internet | IP addressing and routing | IPv4, IPv6 |
| Link | Communication over the local network | Ethernet, Wi-Fi |

### TCP vs UDP

- **TCP** → Connection-oriented, reliable, ordered delivery.
- **UDP** → Connectionless; delivery and ordering are not guaranteed.

An IP address and port identify a network endpoint.

Example: `192.168.1.10:22`

- `192.168.1.10` → IP address
- `22` → SSH port

---

# Network Interface Names

RHEL 7 and later commonly use predictable network interface names.

| Prefix | Meaning | Example |
|---|---|---|
| `en` | Ethernet | `eno1` |
| `wl` | Wireless LAN | `wlp4s0` |
| `ww` | Wireless WAN | `wwan0` |

Predictable names help keep interface names consistent across reboots.

---

# IPv4 Addressing and CIDR

IPv4 addresses are **32 bits**, divided into four 8-bit octets. Each octet ranges from `0` to `255`.

Example: `192.168.1.107/24`

- `/24` → 24 network bits.
- Remaining 8 bits → Host portion.
- Netmask → `255.255.255.0`.

### Example: `192.168.1.107/24`

| Type | Address |
|---|---|
| Network | `192.168.1.0` |
| Host | `192.168.1.107` |
| Usable range | `192.168.1.1 - 192.168.1.254` |
| Broadcast | `192.168.1.255` |
| Gateway (example) | `192.168.1.1` |

### Common Subnets

| CIDR | Netmask | Total Addresses |
|---|---|---:|
| `/16` | `255.255.0.0` | 65,536 |
| `/24` | `255.255.255.0` | 256 |
| `/28` | `255.255.255.240` | 16 |

**Remember:** A larger prefix means a smaller network.

These address counts include network and broadcast addresses for conventional IPv4 subnets.

---

# IPv4 Routing

A routing table tells Linux where to send packets based on their destination.

Example routes:

| Destination | Purpose |
|---|---|
| `192.168.5.0/24` | Local network |
| `172.17.0.0/16` | Another network |
| `0.0.0.0/0` | Default route |

### Longest Prefix Match

When multiple routes match a destination, the most specific matching prefix is normally selected.

- `/24` is more specific than `/16`.
- `0.0.0.0/0` is the IPv4 default route.

A **gateway** forwards packets to other networks.

---

# DHCP and Static IP

**DHCP (Dynamic Host Configuration Protocol)** automatically provides network settings, such as:

- IP address and subnet information.
- Default gateway.
- DNS server information.
- Address lease duration.

**Static IP configuration** is manually configured or reserved to provide a predictable address, often useful for servers.

---

# IPv6 Addressing

IPv6 uses **128-bit addresses**, written in hexadecimal and separated by colons.

Example:

`2001:db8:0:10::1`

### Shortening Rules

1. Leading zeros in a group can be removed.
2. One consecutive sequence of zero groups can be replaced with `::`.
3. `::` can appear only once in an address.

Example:

`2001:0db8:0000:0010:0000:0000:0000:0001`

Becomes:

`2001:db8:0:10::1`

The `2001:db8::/32` prefix is reserved for documentation and examples.

### IPv6 Subnets

A common IPv6 subnet prefix is `/64`.

Example: `2001:db8:abcd:1234::1/64`

- First 64 bits → Network prefix.
- Remaining 64 bits → Interface identifier.

---

# Important IPv6 Addresses

| Address or Prefix | Meaning |
|---|---|
| `::1` | Loopback (localhost) |
| `::` | Unspecified address |
| `::/0` | Default route |
| `fe80::/10` | Link-local unicast |
| `ff00::/8` | Multicast |
| `2000::/3` | Global unicast |
| `fc00::/7` | Unique local addresses |

### SLAAC and DHCPv6

- **SLAAC** → A host configures an IPv6 address using information advertised by a router.
- **DHCPv6** → A DHCPv6 server provides IPv6 configuration.

Both mechanisms can be used together.

### Broadcast vs Multicast

- **IPv4** supports broadcast and multicast.
- **IPv6** does not use broadcast; it uses multicast instead.

Example: `ff02::1` identifies all IPv6 nodes on the local link.

---

# Hostnames and DNS

**DNS (Domain Name System)** resolves hostnames to IP addresses and supports other types of DNS records.

Example:

`server.example.com` → `192.168.1.10`

### Important Files

- `/etc/hosts` → Stores manually defined hostname-to-IP mappings.
- `/etc/resolv.conf` → Contains DNS resolver configuration, such as nameservers.

These files may be managed automatically by network configuration services.

---

# Useful Linux Networking Commands

| Command | Purpose |
|---|---|
| `ip addr show` | Display IP addresses and interfaces |
| `ip -br addr` | Display a brief interface summary |
| `ip route` | Display IPv4 routes |
| `ip -6 route` | Display IPv6 routes |
| `ip route show default` | Display the IPv4 default route |
| `ping 192.168.1.1` | Test network reachability |
| `cat /etc/resolv.conf` | Display DNS resolver configuration |
| `hostname` | Display the system hostname |

Note: A failed ping does not always mean the destination is unreachable; ICMP traffic may be blocked.

---

# Quick Revision

- TCP/IP model → Application, Transport, Internet, Link.
- IPv4 → 32 bits.
- IPv6 → 128 bits.
- `/24` → `255.255.255.0`.
- Larger CIDR prefix → Smaller subnet.
- IPv4 default route → `0.0.0.0/0`.
- IPv6 default route → `::/0`.
- Common IPv6 subnet → `/64`.
- IPv6 loopback → `::1`.
- IPv6 link-local → `fe80::/10`.
- IPv6 multicast → `ff00::/8`.
- DNS → Resolves hostnames to IP addresses.
- `ip addr show` → Displays interfaces and IP addresses.
- `ip route` → Displays IPv4 routing information.

---

# Summary

- The TCP/IP model explains network communication through four layers.
- IPv4 uses 32-bit addresses; IPv6 uses 128-bit addresses.
- CIDR defines network prefixes and subnet sizes.
- Routing tables determine where packets are forwarded.
- DHCP provides dynamic configuration; static configuration provides predictable addresses.
- IPv6 supports SLAAC, DHCPv6, and multicast instead of broadcast.
- DNS resolves hostnames to IP addresses.
- Linux provides the `ip` command to inspect interfaces and routing tables.
