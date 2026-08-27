 # Task 1: Scan Local Network for Open Ports

**Internship:** Cyber Security Internship — Elevate Labs (MSME, Govt. of India)
**Author:** Shailendra Mourya (cybershailendra)

## Objective
Discover open ports on devices in the local network (VMWare) to understand network exposure using Nmap.

## Tools Used
- Nmap 7.99
- Kali Linux (VMware)

## Environment
- Attacker machine: `192.168.59.128` (Kali, eth0)
- Gateway: `192.168.59.2`
- Subnet scanned: `192.168.59.128/24`

## Steps Performed

### 1. Identify local IP and subnet
```
ip addr
ip route
```
Confirmed local IP `192.168.59.128/24` and default gateway `192.168.59.2`.

### 2. Host discovery (ping scan)
```
sudo nmap -sn 192.168.59.128/24
```
**Output:**
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-27 16:06 +0530
Nmap scan report for 192.168.59.1
Host is up (0.00099s latency).
MAC Address: 00:50:56:C0:00:08 (VMware)
Nmap scan report for 192.168.59.2
Host is up (0.00022s latency).
MAC Address: 00:50:56:E7:96:63 (VMware)
Nmap scan report for 192.168.59.254
Host is up (0.00015s latency).
MAC Address: 00:50:56:FB:9B:54 (VMware)
Nmap scan report for 192.168.59.128
Host is up.
Nmap done: 256 IP addresses (4 hosts up) scanned in 3.01 seconds
```
Found 4 live hosts: `.1`, `.2`, `.128`, `.254`.

### 3. TCP SYN scan
```
sudo nmap -sS 192.168.59.128/24
```
**Output:**
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-27 16:07 +0530
Nmap scan report for 192.168.59.1
Host is up (0.014s latency).
All 1000 scanned ports on 192.168.59.1 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:C0:00:08 (VMware)

Nmap scan report for 192.168.59.2
Host is up (0.00018s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: 00:50:56:E7:96:63 (VMware)

Nmap scan report for 192.168.59.254
Host is up (0.00016s latency).
All 1000 scanned ports on 192.168.59.254 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:FB:9B:54 (VMware)

Nmap scan report for 192.168.59.128
Host is up.
All 1000 scanned ports on 192.168.59.128 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)

Nmap done: 256 IP addresses (4 hosts up) scanned in 210.69 seconds
```
Result: only `192.168.59.2` had an open port — **53/tcp (domain)**. Rest were filtered/no-response.

### 4. Service/version detection
```
sudo nmap -sV -sS 192.168.59.128/24
```
**Output:**
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-27 16:11/16:13 +0530
Nmap scan report for 192.168.59.1
Host is up (0.0029s-0.0051s latency).
All 1000 scanned ports on 192.168.59.1 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:C0:00:08 (VMware)

Nmap scan report for 192.168.59.2
Host is up (0.00024s-0.00025s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
53/tcp open  domain  dnsmasq 2.51
MAC Address: 00:50:56:E7:96:63 (VMware)

Nmap scan report for 192.168.59.254
Host is up (0.00031s-0.00032s latency).
All 1000 scanned ports on 192.168.59.254 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:FB:9B:54 (VMware)

Nmap scan report for 192.168.59.128
Host is up.
All 1000 scanned ports on 192.168.59.128 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 256 IP addresses (4 hosts up) scanned in 217.14-217.24 seconds
```
Identified service on port 53 as `dnsmasq 2.51`.

### 5. Targeted port range scan
```
sudo nmap -p 1-1000 192.168.59.128/24 -T3
```
**Output:**
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-27 16:29 +0530
Nmap scan report for 192.168.59.1
Host is up (0.010s latency).
All 1000 scanned ports on 192.168.59.1 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:C0:00:08 (VMware)

Nmap scan report for 192.168.59.2
Host is up (0.00024s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: 00:50:56:E7:96:63 (VMware)

Nmap scan report for 192.168.59.254
Host is up (0.00044s latency).
All 1000 scanned ports on 192.168.59.254 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:FB:9B:54 (VMware)
```
Confirmed same results with a faster timing template.

### 6. Final scan saved to file
```
nmap -sS 192.168.59.128/24 -oN scan.txt
```
**Output:**
```
Starting Nmap 7.99 ( https://nmap.org ) at 2026-08-27 16:31 +0530
Nmap scan report for 192.168.59.1
Host is up (0.0011s latency).
All 1000 scanned ports on 192.168.59.1 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:C0:00:08 (VMware)

Nmap scan report for 192.168.59.2
Host is up (0.00024s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: 00:50:56:E7:96:63 (VMware)

Nmap scan report for 192.168.59.254
Host is up (0.00029s latency).
All 1000 scanned ports on 192.168.59.254 are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 00:50:56:FB:9B:54 (VMware)
```

### 7. External target scan 
```
nmap -sV -p 1-1000 -sS -oN scan.txt testaspnet.vulnweb.com
```
**Output (`scan.txt`):**
```
# Nmap 7.99 scan  initiated Thu Aug 27 16:33:47 2026 as: /usr/lib/nmap/nmap -sV -p 1-1000 -sS -oN  scan.txt testaspnet.vulnweb.com
Nmap scan report for  testaspnet.vulnweb.com (44.238.29.244)
Host is up (0.017s latency).
Other addresses for  testaspnet.vulnweb.com  (not scanned):  64:ff9b::2cee:1df4
rDNS record for 44.238.29.244: ec2-44-238-29-244.us-west-2.compute.amazonaws.com
All 1000 scanned  ports on testaspnet.vulnweb.com (44.238.29.244) are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)

Service  detection performed. Please report any incorrect results  at https://nmap.org/submit/ .
# Nmap done at Thu Aug 27 16:34:39 2026 -- 1 IP address (1 host up) scanned in 52.17 seconds
```
All 1000 scanned ports were  filtered (no response) — host likely firewalled against unsolicited scans.

## Results Summary

| Host | Status | Open Ports | Service |
|---|---|---|---|
| 192.168.59.1 | Up | None (filtered) | Gateway/router |
| 192.168.59.2 | Up | 53/tcp | dnsmasq 2.51 (DNS) |
| 192.168.59.128 | Up | None (filtered) | Kali (self) |
| 192.168.59.254 | Up | None (filtered) | — |
| testaspnet.vulnweb.com | Up | None (filtered) | — |

## Security Observations
- Only one device () exposed a service — DNS (dnsmasq), commonly the VM host/DHCP server.
- Other hosts responded to ping but showed all ports filtered, indicating a host-based firewall or restrictive network ACLs.
- No unnecessary open ports found on the scanned subnet — low attack surface.

## Files in this Repo
- `scan.txt` — raw Nmap output
- `screenshots/` — terminal screenshots of each scan step
- `README.md` — this file
