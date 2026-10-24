# Cisco Packet Tracer Lab: Basic Router Configuration & Security

A Packet Tracer Physical Mode (PTPM) simulation project demonstrating initial ISR router setup, dual-stack (IPv4/IPv6) addressing, SSH remote access hardening, and command-line verification techniques.

---

## 📌 Topology & Objectives

- **Topology:** Star / Physical Rack & Stack Setup
- **Core Devices:**
  - **Router:** `R1` (Cisco 4321 ISR)
  - **Switch:** Cisco Catalyst 2960
  - **End Devices:** `PC-A` (Workstation), `Server`

### Key Objectives:
1. **Cable Physical Topology:** Cable physical rack equipment and initialize basic terminal connections.
2. **Device Hardening:** Configure device hardening including password length enforcement, brute-force login blocking, encrypted local users, and SSH v2 access.
3. **Dual-Stack Addressing:** Implement dual-stack (IPv4 & IPv6) interface addressing and enable IPv6 routing.
4. **Verification & Testing:** Verify remote connectivity using SSH and evaluate system status using IOS filtering commands.

---

## 🗺️ IP Addressing Table

| Device | Interface | IPv4 Address | Subnet Mask | IPv6 Address / Prefix | Default Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **R1** | `G0/0/0` | `192.168.1.1` | `255.255.255.0` | `2001:DB8:ACAD:1::1/64` | N/A |
| **R1** | `Loopback0` | `10.10.1.1` | `255.255.255.255` | `2001:DB8:ACAD:FF::1/64` | N/A |
| **PC-A** | `NIC` | `192.168.1.3` | `255.255.255.0` | `2001:DB8:ACAD:1::3/64` | `192.168.1.1` |
| **Server** | `NIC` | `192.168.1.2` | `255.255.255.0` | `2001:DB8:ACAD:1::2/64` | `192.168.1.1` |

---

## ⚙️ Configuration Highlights

### 1. Hardening & Security Policy (`R1`)
Enforced password security policies, encrypted local administrator accounts, and configured brute-force prevention:

```cisco
hostname R1
ip domain-name ccna-lab.com
security passwords min-length 12
service password-encryption
login block-for 120 attempts 3 within 60
username SSHadmin secret 55Hadm!n2020
enable secret $cisco!PRIV*
banner motd # Unauthorized Access Prohibited! #
```

### 2. SSH Remote Access Setup
Key generation and restriction of line access strictly to SSH:

```cisco
crypto key generate rsa modulus 1024

line console 0
 password $cisco!!CON*
 exec-timeout 4 0
 login
exit

line vty 0 4
 password $cisco!!VTY*
 exec-timeout 4 0
 transport input ssh
 login local
exit
```

### 3. Dual-Stack Routing & Interfaces
Configured IPv4 and IPv6 on routed interfaces:

```cisco
ipv6 unicast-routing

interface GigabitEthernet0/0/0
 description Connection to Switch LAN
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:DB8:ACAD:1::1/64
 no shutdown
exit

interface Loopback0
 description Management Loopback Interface
 ip address 10.10.1.1 255.255.255.255
 ipv6 address 2001:DB8:ACAD:FF::1/64
 no shutdown
exit
```

---

## 🔍 Verification & Testing

- **SSH Connectivity:** Verified encrypted connection from `PC-A` to `R1`'s IPv4 and IPv6 loopback addresses via SSH using `SSHadmin`.
- **Ping Tests:** Confirmed end-to-end ICMP reachability across IPv4 and IPv6 configurations between `PC-A` and `Server`.
- **IOS Output Filtering:** Used output modifiers (pipes `|`) to verify specific active configuration parameters:

```cisco
R1# show version | include register
R1# show running-config | section vty
R1# show ip interface brief
```