---
title: "Chapter 3 - Analyzing and Managing Networks"
subject: "Linux Networking / Cybersecurity Basics"
unit: "Unit 3"
source: "Handwritten study notes"
tags:
  - linux
  - networking
  - cybersecurity
  - ifconfig
  - iwconfig
  - dns
  - dig
  - obsidian-notes
---

# Chapter 3 — Analyzing and Managing Networks

> [!summary]
> A good hacker, cybersecurity learner, or network administrator must understand how a system connects to a network, how to inspect network interfaces, how to change IP/DNS settings, and how to troubleshoot traffic problems.

These notes are typed from the uploaded handwritten pages and expanded in a detailed, beginner-friendly way.

---

## Original Study Note Pages

Use these images as visual reference inside Obsidian.

![[assets/page-01-ifconfig-overview.jpeg]]

![[assets/page-02-mtu-ipv4-netmask-mac.jpeg]]

![[assets/page-03-rx-tx-errors-commands.jpeg]]

![[assets/page-04-enable-disable-arp-promisc.jpeg]]

![[assets/page-05-iwconfig-fields.jpeg]]

![[assets/page-06-wireless-dns-intro.jpeg]]

![[assets/page-07-dig-records.jpeg]]

![[assets/page-08-resolv-hosts.jpeg]]

---

# 1. Why Network Analysis Matters

A network is the connection between devices so that they can exchange information.

In cybersecurity and Linux administration, you must know:

- How your system is connected.
- Which interface is active.
- What IP address your system has.
- What MAC address your interface uses.
- Whether packets are being sent or received.
- Whether DNS is working correctly.
- Whether wireless connection settings are correct.

> [!tip] Sticky Note
> Before changing any network setting, first observe the current state. Use commands like `ifconfig`, `ifconfig -a`, `iwconfig`, `ip link show`, and `dig`.

---

# 2. Analyzing Networks with `ifconfig`

## 2.1 Meaning of `ifconfig`

`ifconfig` means **interface configuration**.

It is a Linux/Unix command used to:

- View network interfaces.
- Enable interfaces.
- Disable interfaces.
- Configure IP addresses.
- Configure netmask and broadcast address.
- View MAC address.
- View packet statistics.
- Check errors and dropped packets.

> [!note]
> Modern Linux systems commonly use the `ip` command, but `ifconfig` is still used in many Linux beginner books, Kali Linux tutorials, and cybersecurity labs.

---

## 2.2 Basic Syntax of `ifconfig`

```bash
ifconfig [options]
```

```bash
ifconfig [interface]
```

```bash
ifconfig [interface] [options]
```

```bash
ifconfig [interface] [address] [options]
```

### Examples

```bash
ifconfig
```

Shows only active network interfaces.

```bash
ifconfig -a
```

Shows all interfaces, including disabled/down interfaces.

```bash
ifconfig eth0
```

Shows information only for the `eth0` interface.

---

# 3. Understanding Network Interfaces

A **network interface** is the connection point between your operating system and a network.

It can be:

- A physical Ethernet card.
- A wireless adapter.
- A loopback interface.
- A virtual adapter created by Docker, VPN, or virtualization software.

---

## 3.1 Common Interface Names

| Interface | Meaning |
|---|---|
| `lo` | Loopback interface. It is your own system talking to itself. Usually uses `127.0.0.1`. |
| `eth0` | First Ethernet interface. This is old/classic Linux naming. |
| `wlan0` | Wireless network interface. |
| `enp0s3` | Ethernet interface using newer predictable Linux naming. Common in VirtualBox. |
| `ens33` | Common VMware Ethernet interface name. |
| `docker0` | Docker virtual network bridge. |
| `tun0` | VPN tunnel interface. |
| `virbr0` | Virtual bridge used by virtualization tools. |

> [!tip] Sticky Note
> Interface names may change depending on whether you are using Kali, Ubuntu, VMware, VirtualBox, Docker, or a VPN.

---

# 4. Interface Flags

Flags describe the current state and capabilities of a network interface.

| Flag | Meaning |
|---|---|
| `UP` | Interface is enabled. |
| `DOWN` | Interface is disabled. |
| `RUNNING` | Interface has an active connection. |
| `BROADCAST` | Interface supports broadcast communication. |
| `LOOPBACK` | Internal system interface. |
| `MULTICAST` | Interface supports multicast communication. |
| `PROMISC` | Promiscuous mode is enabled. |
| `POINTOPOINT` | Point-to-point link, common in VPN/PPP connections. |

## 4.1 Example

If `ifconfig` shows:

```text
UP BROADCAST RUNNING MULTICAST
```

It means:

- The interface is enabled.
- It supports broadcast.
- It has an active link.
- It supports multicast traffic.

---

# 5. MTU — Maximum Transmission Unit

## 5.1 Meaning of MTU

MTU stands for **Maximum Transmission Unit**.

It means the maximum packet size that can be sent through a network interface without fragmentation.

> [!important]
> Fragmentation means breaking a large packet into smaller pieces so it can travel across a network.

---

## 5.2 Common MTU Values

| MTU Value | Usage |
|---|---|
| `1500` | Standard Ethernet MTU. Most common default value. |
| `9000` | Jumbo frames. Used in high-performance networks. |
| `1400` or lower | Often used in VPN/tunnel networks. |

## 5.3 Check MTU

```bash
ifconfig eth0
```

or:

```bash
ifconfig eth0 | grep mtu
```

## 5.4 Change MTU

```bash
sudo ifconfig eth0 mtu 1400
```

## 5.5 Why MTU Matters

MTU mismatch can cause:

- Slow website loading.
- VPN problems.
- Packet fragmentation.
- Connection drops.
- Some websites working while others fail.

> [!tip] Sticky Note
> If a VPN or tunnel connection is slow or unstable, MTU mismatch is one possible reason.

---

# 6. IPv4 Address

## 6.1 What is an IPv4 Address?

An IPv4 address identifies a device on a network.

Example:

```text
192.168.1.25
```

This address can be divided into:

- **Network portion**
- **Host portion**

---

## 6.2 Example Breakdown

For:

```text
IP Address: 192.168.1.25
Netmask:    255.255.255.0
```

The network is:

```text
192.168.1.0
```

The host/device is:

```text
25
```

So:

```text
192.168.1 = Network portion
25        = Host/device portion
```

---

## 6.3 169.254.x.x Address

If an IP looks like:

```text
169.254.x.x
```

it usually means the system failed to get an IP from DHCP.

This is called a **link-local address**.

> [!warning]
> If your system gets `169.254.x.x`, check DHCP, Wi-Fi, Ethernet cable, router, or adapter state.

---

# 7. Netmask

## 7.1 What is a Netmask?

A netmask tells which part of an IP address belongs to the network.

Example:

```text
255.255.255.0
```

This means the first three parts identify the network.

Example:

```text
IP Address: 192.168.1.25
Netmask:    255.255.255.0
Network:    192.168.1.0
```

## 7.2 Usable Host Range

For:

```text
192.168.1.0/24
```

Usually:

```text
Network address:   192.168.1.0
Usable hosts:      192.168.1.1 - 192.168.1.254
Broadcast address: 192.168.1.255
```

> [!note]
> The network address and broadcast address are normally not assigned to normal devices.

---

# 8. Broadcast Address

## 8.1 What is a Broadcast Address?

A broadcast address is used to send data to all devices in the local network.

Example:

```text
192.168.1.255
```

For the network:

```text
192.168.1.0/24
```

the broadcast address is usually:

```text
192.168.1.255
```

## 8.2 Use of Broadcast

Broadcast communication is used when a device needs to reach all devices in the local network.

Example:

- ARP discovery
- DHCP requests
- Local network service discovery

---

# 9. MAC Address

## 9.1 What is a MAC Address?

MAC stands for **Media Access Control**.

It is the physical hardware address of a network interface.

Example:

```text
08:00:27:ab:cd:ef
```

## 9.2 Use of MAC Address

MAC address is used for:

1. Device identification.
2. Packet delivery inside a local network.
3. LAN troubleshooting.
4. ARP communication.

## 9.3 IP Address vs MAC Address

| Feature | IP Address | MAC Address |
|---|---|---|
| Layer | Network layer | Data link layer |
| Example | `192.168.1.25` | `08:00:27:ab:cd:ef` |
| Purpose | Reach devices across networks | Identify device on local network |
| Can change? | Yes | Usually fixed, but can be spoofed |

---

# 10. RX and TX Packets

## 10.1 Meaning

| Term | Meaning |
|---|---|
| `RX` | Received packets |
| `TX` | Transmitted packets |

Example:

```text
RX packets 10500
TX packets 8630
```

## 10.2 How to Understand RX/TX

If `RX` is increasing:

- Your system is receiving network traffic.

If `TX` is increasing:

- Your system is sending network traffic.

If both are increasing normally:

- The interface is actively communicating.

---

# 11. Errors and Dropped Packets

## 11.1 Meaning

| Field | Meaning |
|---|---|
| `errors` | Bad/corrupted packets or link errors |
| `dropped` | Packets discarded by the system |
| `overruns` | Interface buffer overflow |
| `frame` | Frame-level errors |
| `carrier` | Physical link problems |
| `collisions` | Ethernet collision counter |

Example:

```text
RX errors 0 dropped 0
TX errors 0 dropped 0
```

This is a healthy output.

## 11.2 What Causes Errors/Dropped Packets?

Errors and dropped packets can happen due to:

1. Weak Wi-Fi signal.
2. Bad Ethernet cable.
3. Driver issues.
4. Network congestion.
5. Hardware problems.
6. MTU mismatch.

> [!tip] Troubleshooting Tip
> If errors or dropped packets continuously increase, check cable, Wi-Fi signal, adapter driver, MTU, and router/switch health.

---

# 12. Basic `ifconfig` Commands

## 12.1 Show Active Interfaces

```bash
ifconfig
```

Shows active interfaces only.

## 12.2 Show All Interfaces

```bash
ifconfig -a
```

Shows all interfaces, including down/disabled interfaces.

## 12.3 Show One Interface

```bash
ifconfig eth0
```

```bash
ifconfig wlan0
```

```bash
ifconfig lo
```

## 12.4 Short Summary View

```bash
ifconfig -s
```

Shows a short summary similar to:

```bash
netstat -i
```

## 12.5 Verbose Output

```bash
ifconfig -v
```

Shows more detailed information, useful for checking errors.

---

# 13. Enabling and Disabling Interfaces

## 13.1 Enable Interface

```bash
sudo ifconfig eth0 up
```

This turns the interface on.

## 13.2 Disable Interface

```bash
sudo ifconfig eth0 down
```

This turns the interface off.

> [!warning]
> If you disable the active network interface, you may lose internet connection.

---

# 14. Assigning an IP Address

## 14.1 Assign IP Address

```bash
sudo ifconfig eth0 192.168.1.50
```

This assigns an IP address to `eth0`.

## 14.2 Assign IP Address with Netmask and Broadcast

```bash
sudo ifconfig eth0 192.168.1.50 netmask 255.255.255.0 broadcast 192.168.1.255
```

Meaning:

- `192.168.1.50` = IP address
- `255.255.255.0` = netmask
- `192.168.1.255` = broadcast address

> [!note]
> These changes are usually temporary. They may reset after reboot or network service restart.

---

# 15. Getting a New IP from DHCP

## 15.1 What is DHCP?

DHCP stands for **Dynamic Host Configuration Protocol**.

It automatically assigns:

- IP address
- Netmask
- Gateway
- DNS server

## 15.2 Request New IP Address

```bash
sudo dhclient eth0
```

This asks the DHCP server/router for a new IP address.

## 15.3 Verify

```bash
ifconfig eth0
```

```bash
ping -c 4 192.168.1.1
```

```bash
ping -c 4 8.8.8.8
```

```bash
dig google.com
```

---

# 16. ARP

## 16.1 Meaning of ARP

ARP stands for **Address Resolution Protocol**.

It maps IP addresses to MAC addresses.

Example:

```text
192.168.1.1 -> aa:bb:cc:dd:ee:ff
```

## 16.2 Enable ARP

```bash
sudo ifconfig eth0 arp
```

## 16.3 Disable ARP

```bash
sudo ifconfig eth0 -arp
```

> [!important]
> ARP is very important in LAN communication. If ARP is disabled or attacked, local network communication can fail.

---

# 17. Promiscuous Mode

## 17.1 Meaning

Promiscuous mode allows a network interface to receive all packets it can see, not only packets addressed to its own MAC address.

This is useful for:

- Packet capturing
- Wireshark
- Sniffing in authorized labs
- Network monitoring

## 17.2 Enable Promiscuous Mode

```bash
sudo ifconfig eth0 promisc
```

## 17.3 Disable Promiscuous Mode

```bash
sudo ifconfig eth0 -promisc
```

> [!warning]
> Use promiscuous mode only on networks you own or are authorized to test.

---

# 18. Multicast

## 18.1 Meaning

Multicast sends data to a selected group of devices instead of one device or all devices.

## 18.2 Enable Multicast

```bash
sudo ifconfig eth0 multicast
```

## 18.3 Disable Multicast

```bash
sudo ifconfig eth0 -multicast
```

---

# 19. Changing MAC Address

## 19.1 Method Using `ifconfig`

```bash
sudo ifconfig eth0 down
```

```bash
sudo ifconfig eth0 hw ether 00:11:22:33:44:55
```

```bash
sudo ifconfig eth0 up
```

## 19.2 Method Using `macchanger`

Install if needed:

```bash
sudo apt install macchanger
```

Change MAC automatically:

```bash
sudo macchanger -r eth0
```

Restore original MAC:

```bash
sudo macchanger -p eth0
```

Set a specific MAC:

```bash
sudo macchanger -m 00:11:22:33:44:55 eth0
```

> [!tip]
> `macchanger -r` gives a random MAC address. `macchanger -p` restores the permanent/original MAC.

---

# 20. Rename Interface

## 20.1 Rename `eth0` to `my0`

```bash
sudo ifconfig eth0 down
```

```bash
sudo ifconfig eth0 name my0
```

```bash
sudo ifconfig my0 up
```

> [!note]
> Interface renaming may not be permanent depending on your Linux distribution and network manager.

---

# 21. Set TX Queue Length

## 21.1 Command

```bash
sudo ifconfig eth0 txqueuelen 1000
```

## 21.2 Meaning

TX queue length controls how many packets can wait in the transmit queue.

If the value is too small:

- Packets may drop during heavy traffic.

If the value is too large:

- Latency may increase in some situations.

---

# 22. IPv6 Commands

## 22.1 Add IPv6 Address

```bash
sudo ifconfig eth0 add 2001:db8::1/64
```

## 22.2 Delete IPv6 Address

```bash
sudo ifconfig eth0 del 2001:db8::1/64
```

---

# 23. Checking Wireless Network Devices with `iwconfig`

## 23.1 Meaning of `iwconfig`

`iwconfig` is used to view and configure wireless network interfaces.

It shows wireless settings such as:

- SSID/ESSID
- Mode
- Frequency
- Channel
- Signal strength
- Bit rate
- Transmit power
- Encryption status
- Power management

> [!note]
> Modern systems may use `iw`, but `iwconfig` is still common in older Linux networking lessons.

---

## 23.2 Basic Commands

```bash
iwconfig
```

Shows wireless interfaces.

```bash
iwconfig wlan0
```

Shows information about `wlan0`.

```bash
ip link show
```

Can be used to check wireless interface names.

---

# 24. Important `iwconfig` Fields

| Field | Meaning |
|---|---|
| `wlan0` | Wireless network interface name |
| `IEEE 802.11` | Wi-Fi standard |
| `ESSID` | Wi-Fi network name |
| `Mode` | Wireless operating mode |
| `Frequency` | Frequency of wireless adapter |
| `Access Point` | MAC address of router/access point |
| `Bit Rate` | Current wireless link speed |
| `Tx-Power` | Transmit power |
| `Encryption key` | Shows whether encryption is enabled |
| `Power Management` | Battery saving feature for Wi-Fi |
| `Signal level` | Signal strength |

---

# 25. Wireless Modes

| Mode | Meaning |
|---|---|
| `Managed` | Normal mode. Connected to a Wi-Fi router/access point. |
| `Monitor` | Packet capturing mode for wireless traffic. |
| `Ad-Hoc` | Device-to-device wireless communication. |
| `Master` | Device acts like an access point. |

> [!important]
> Monitor mode is commonly used in wireless security testing, but only use it in authorized labs.

---

# 26. Frequency

Frequency shows which wireless band/channel your adapter is using.

Common bands:

- `2.4 GHz`
- `5 GHz`
- `6 GHz`

Example:

```text
Frequency: 2.437 GHz
```

This often corresponds to Wi-Fi channel 6.

---

# 27. Access Point

Access point shows the MAC address of the router/access point your device is connected to.

Example:

```text
Access Point: AA:BB:CC:DD:EE:FF
```

---

# 28. Bit Rate

Bit rate shows the current wireless link speed between your device and router.

Example:

```text
Bit Rate=54 Mb/s
```

Low bit rate may indicate:

- Weak signal.
- Interference.
- Distance from router.
- Old Wi-Fi standard.
- Power saving mode.

---

# 29. Tx-Power

Tx-Power means transmission power.

Higher power can improve range, but it depends on:

- Country regulations.
- Driver support.
- Hardware support.

Example:

```text
Tx-Power=20 dBm
```

---

# 30. Encryption Key

Encryption key field shows whether Wi-Fi encryption is enabled.

Example:

```text
Encryption key:on
```

or:

```text
Encryption key:off
```

> [!warning]
> Open Wi-Fi networks without encryption are risky because traffic can be exposed.

---

# 31. Power Management

Power management saves battery by reducing wireless activity.

But sometimes it may cause:

- Slow Wi-Fi.
- Drops.
- Delayed response.

Disable power management temporarily:

```bash
sudo iwconfig wlan0 power off
```

Enable power management:

```bash
sudo iwconfig wlan0 power on
```

---

# 32. Signal Strength

Signal strength is usually shown in negative dBm.

Examples:

| Signal | Quality |
|---|---|
| `-30 dBm` | Excellent |
| `-50 dBm` | Good |
| `-67 dBm` | Usable |
| `-80 dBm` | Weak |
| `-90 dBm` | Very poor |

> [!tip]
> Closer to zero is better. `-35 dBm` is better than `-75 dBm`.

---

# 33. Useful `iwconfig` Commands

## 33.1 Change Wireless Mode

```bash
sudo iwconfig wlan0 mode managed
```

```bash
sudo iwconfig wlan0 mode monitor
```

## 33.2 Set Wi-Fi Network Name

```bash
sudo iwconfig wlan0 essid "Home_WiFi"
```

## 33.3 Set Channel

```bash
sudo iwconfig wlan0 channel 6
```

## 33.4 Set Frequency

```bash
sudo iwconfig wlan0 freq 2.437G
```

## 33.5 Set Bit Rate Manually

```bash
sudo iwconfig wlan0 rate 54M
```

## 33.6 Set Bit Rate Automatically

```bash
sudo iwconfig wlan0 rate auto
```

## 33.7 Set Transmit Power

```bash
sudo iwconfig wlan0 txpower 20
```

---

# 34. Checking Wireless Signal and Nearby Networks

## 34.1 Check Signal

```bash
iwconfig wlan0 | grep -i signal
```

## 34.2 Watch Signal Live

```bash
watch -n 1 iwconfig wlan0
```

This refreshes the output every second.

## 34.3 Scan Nearby Networks

```bash
sudo iwlist wlan0 scan
```

## 34.4 Filter Network Names

```bash
sudo iwlist wlan0 scan | grep ESSID
```

---

# 35. Changing Your Network Information

## 35.1 Change IP Address

```bash
sudo ifconfig eth0 192.168.181.115
```

## 35.2 Change Netmask

```bash
sudo ifconfig eth0 netmask 255.255.255.0
```

## 35.3 Change Broadcast Address

```bash
sudo ifconfig eth0 broadcast 192.168.181.255
```

---

# 36. Spoofing Your MAC Address

## 36.1 Why MAC Spoofing is Useful

MAC spoofing means changing the MAC address shown by your interface.

It can be useful for:

- Privacy in lab environments.
- Testing network filters.
- Learning how device identification works.

> [!warning]
> Use MAC spoofing only on your own devices and authorized networks.

## 36.2 Commands

```bash
sudo ifconfig eth0 down
```

```bash
sudo ifconfig eth0 hw ether 00:11:22:33:44:55
```

```bash
sudo ifconfig eth0 up
```

---

# 37. Assigning New IP Address from DHCP Server

Linux has a DHCP client that runs in the background.

The DHCP server assigns IP information automatically.

Command:

```bash
sudo dhclient eth0
```

If DHCP works, your interface will receive:

- IP address
- Netmask
- Gateway
- DNS resolver

---

# 38. Manipulating the Domain Name System

## 38.1 What is DNS?

DNS stands for **Domain Name System**.

DNS translates human-readable domain names into machine-readable IP addresses.

Example:

```text
example.com -> 93.184.216.34
```

Without DNS, users would need to remember IP addresses instead of names.

---

## 38.2 Why DNS is Important in Cybersecurity

DNS records can reveal:

- Hostnames
- Mail servers
- Name servers
- Subdomains
- Internal naming patterns
- Network structure

> [!note]
> During authorized reconnaissance, DNS information helps security learners understand how a target network is organized.

---

# 39. Examining DNS with `dig`

## 39.1 Meaning of `dig`

`dig` stands for **Domain Information Groper**.

It is a command-line tool used to query DNS servers and examine DNS records.

---

## 39.2 Basic Syntax

```bash
dig [@dns-server] domain-name [record-type] [options]
```

## 39.3 Basic Example

```bash
dig example.com
```

This asks:

> What IP address does `example.com` point to?

---

# 40. Important `dig` Output Fields

## 40.1 Header

The header contains:

- Opcode
- Query status
- Query ID
- Warning/status information

## 40.2 Important Status Values

| Status | Meaning |
|---|---|
| `NOERROR` | Query worked successfully. |
| `NXDOMAIN` | Domain does not exist. |
| `SERVFAIL` | DNS server failed. |
| `REFUSED` | DNS server refused the query. |

---

## 40.3 Flags

Common flags:

| Flag | Meaning |
|---|---|
| `qr` | Query response |
| `rd` | Recursion desired |
| `ra` | Recursion available |
| `aa` | Authoritative answer |

---

## 40.4 Question Section

Shows what you asked.

Example:

```text
;google.com. IN A
```

## 40.5 Answer Section

Shows the DNS answer.

Example:

```text
google.com. 300 IN A 142.250.xxx.xxx
```

## 40.6 Authority Section

Shows which DNS servers are responsible for the domain.

## 40.7 Additional Section

Shows extra useful information such as IP addresses of name servers.

## 40.8 Server Section

Shows which DNS server answered your query.

---

# 41. Common DNS Record Types

| Record Type | Meaning |
|---|---|
| `A` | Maps domain name to IPv4 address. |
| `AAAA` | Maps domain name to IPv6 address. |
| `CNAME` | Alias for another domain. |
| `MX` | Mail server record. |
| `NS` | Name server record. |
| `TXT` | Text records, often used for verification/security policies. |
| `SOA` | Start of authority. |

---

# 42. Useful `dig` Commands

## 42.1 Find IPv4 Address

```bash
dig google.com A
```

## 42.2 Find IPv6 Address

```bash
dig google.com AAAA
```

## 42.3 Trace DNS Resolution Path

```bash
dig google.com +trace
```

This shows the DNS resolution path from root servers down to the answer.

## 42.4 Query Using Specific DNS Server

```bash
dig @8.8.8.8 example.com
```

This asks Google DNS server for the answer.

## 42.5 Check Mail Servers

```bash
dig google.com MX
```

## 42.6 Check Name Servers

```bash
dig google.com NS
```

---

# 43. Changing Your DNS Server

## 43.1 `/etc/resolv.conf`

In Linux, DNS server settings are often stored in:

```text
/etc/resolv.conf
```

View it:

```bash
cat /etc/resolv.conf
```

Edit it:

```bash
sudo nano /etc/resolv.conf
```

Add:

```text
nameserver 8.8.8.8
```

Save and exit.

---

## 43.2 Using Command Line

Normal redirection may fail with sudo:

```bash
sudo echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Better method:

```bash
sudo sh -c 'echo "nameserver 8.8.8.8" > /etc/resolv.conf'
```

## 43.3 Multiple DNS Servers

You can add multiple DNS servers:

```text
nameserver 8.8.8.8
nameserver 1.1.1.1
```

> [!warning]
> On some systems, `/etc/resolv.conf` is automatically rewritten by NetworkManager, systemd-resolved, or DHCP.

---

# 44. Mapping Your Own IP Address

## 44.1 What Does Mapping Mean?

Mapping means giving a readable name to an IP address.

Example:

```text
192.168.1.25 target
```

Now instead of typing:

```bash
ping 192.168.1.25
```

you can type:

```bash
ping target
```

---

## 44.2 `/etc/hosts`

Local hostname mappings are stored in:

```text
/etc/hosts
```

Edit it:

```bash
sudo nano /etc/hosts
```

Add:

```text
192.168.1.25 target
```

Save the file.

Test:

```bash
ping target
```

---

## 44.3 Localhost Mapping

Common default mapping:

```text
127.0.0.1 localhost
```

You can also add a local lab name:

```text
127.0.0.1 mybox.lab
```

Then you can access:

```text
http://mybox.lab
```

instead of:

```text
http://127.0.0.1
```

> [!important]
> `/etc/hosts` affects only your own computer. It does not change DNS for other people.

---

# 45. Full Command Revision Sheet

## 45.1 Interface Checking

```bash
ifconfig
ifconfig -a
ifconfig eth0
ifconfig wlan0
ifconfig lo
ifconfig -s
ifconfig -v
ip link show
```

## 45.2 Interface Control

```bash
sudo ifconfig eth0 up
sudo ifconfig eth0 down
```

## 45.3 IP Configuration

```bash
sudo ifconfig eth0 192.168.1.50
sudo ifconfig eth0 192.168.1.50 netmask 255.255.255.0
sudo ifconfig eth0 192.168.1.50 netmask 255.255.255.0 broadcast 192.168.1.255
```

## 45.4 MTU

```bash
ifconfig eth0 | grep mtu
sudo ifconfig eth0 mtu 1400
```

## 45.5 DHCP

```bash
sudo dhclient eth0
```

## 45.6 ARP

```bash
sudo ifconfig eth0 arp
sudo ifconfig eth0 -arp
```

## 45.7 Promiscuous Mode

```bash
sudo ifconfig eth0 promisc
sudo ifconfig eth0 -promisc
```

## 45.8 Multicast

```bash
sudo ifconfig eth0 multicast
sudo ifconfig eth0 -multicast
```

## 45.9 MAC Address Change

```bash
sudo ifconfig eth0 down
sudo ifconfig eth0 hw ether 00:11:22:33:44:55
sudo ifconfig eth0 up
```

## 45.10 Wireless

```bash
iwconfig
iwconfig wlan0
sudo iwconfig wlan0 mode managed
sudo iwconfig wlan0 mode monitor
sudo iwconfig wlan0 essid "Home_WiFi"
sudo iwconfig wlan0 channel 6
sudo iwconfig wlan0 freq 2.437G
sudo iwconfig wlan0 rate 54M
sudo iwconfig wlan0 rate auto
sudo iwconfig wlan0 txpower 20
sudo iwconfig wlan0 power off
iwconfig wlan0 | grep -i signal
watch -n 1 iwconfig wlan0
sudo iwlist wlan0 scan
sudo iwlist wlan0 scan | grep ESSID
```

## 45.11 DNS

```bash
dig example.com
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com NS
dig google.com +trace
dig @8.8.8.8 example.com
cat /etc/resolv.conf
sudo nano /etc/resolv.conf
sudo nano /etc/hosts
```

---

# 46. Troubleshooting Flow

When internet or network connection fails, follow this order:

```text
1. Check interface
2. Check IP address
3. Check gateway/router
4. Check internet IP connectivity
5. Check DNS
6. Check application/browser
```

## 46.1 Commands

```bash
ifconfig -a
```

```bash
sudo dhclient eth0
```

```bash
ping -c 4 192.168.1.1
```

```bash
ping -c 4 8.8.8.8
```

```bash
dig google.com
```

---

# 47. Quick Memory Points

> [!tip] Remember
> `ifconfig` = interface configuration.

> [!tip] Remember
> `lo` = loopback = your own system.

> [!tip] Remember
> `eth0` = Ethernet, `wlan0` = wireless.

> [!tip] Remember
> `RX` = received packets, `TX` = transmitted packets.

> [!tip] Remember
> `MTU` controls maximum packet size before fragmentation.

> [!tip] Remember
> `DNS` maps names to IP addresses.

> [!tip] Remember
> `/etc/hosts` is local name mapping. `/etc/resolv.conf` stores DNS resolver information.

---

# 48. Practice Tasks

## Task 1 — Interface Audit

Run:

```bash
ifconfig -a
```

Write down:

- Interface names
- IP addresses
- MAC addresses
- MTU
- Flags
- RX/TX packet count

## Task 2 — Wireless Audit

Run:

```bash
iwconfig
```

Write down:

- ESSID
- Mode
- Frequency
- Access point
- Bit rate
- Tx-Power
- Signal level

## Task 3 — DNS Audit

Run:

```bash
dig google.com A
dig google.com MX
dig google.com NS
```

Write down:

- Answer section
- DNS server used
- Query status

## Task 4 — Local Host Mapping

Edit:

```bash
sudo nano /etc/hosts
```

Add:

```text
127.0.0.1 mylab.local
```

Test:

```bash
ping mylab.local
```

---

# 49. Final Summary

This chapter teaches how to inspect and manage Linux networking.

Main tools:

- `ifconfig` for network interface inspection/configuration.
- `iwconfig` for wireless interface checking/configuration.
- `dhclient` for DHCP IP assignment.
- `dig` for DNS examination.
- `/etc/resolv.conf` for DNS resolver settings.
- `/etc/hosts` for local IP-to-name mapping.

Main concepts:

- Interface
- IP address
- Netmask
- Broadcast
- MAC address
- MTU
- RX/TX packets
- Errors/dropped packets
- Wireless modes
- DNS records
- Local host mapping

> [!success]
> If you understand these commands and outputs, you can start troubleshooting Linux network problems like a real system administrator and cybersecurity learner.

