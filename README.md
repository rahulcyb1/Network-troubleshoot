# Windows Network Connectivity Troubleshooting Lab

## Project Overview

The scenario involves a user reporting that their Windows 11 workstation cannot access the internet.

The objective was to identify the connectivity problem through systematic troubleshooting using Windows CMD, PowerShell, IP configuration analysis, DNS diagnostics and firewall inspection.

## Lab Environment

| Component | Configuration |
|---|---|
| Virtualization | Oracle VirtualBox |
| Firewall | pfSense |
| Client Operating System | Windows 11 |
| Server Operating System | Windows Server 2022 |
| Domain | soc.lab |
| Client IPv4 | 192.168.1.106 |
| Domain Controller / DNS | 192.168.1.101 |
| Default Gateway | 192.168.1.1 |
| Subnet Mask | 255.255.255.0 |

## Network Troubleshooting Scenario

A user reported that they were unable to access the internet from their domain-joined Windows 11 workstation.

I approached the issue using a structured troubleshooting methodology.

## Troubleshooting Process

### 1. Local TCP/IP Stack Verification

Command:

```cmd
ping 127.0.0.1
```

Result: Successful.

Observation: The local TCP/IP loopback test completed successfully.

### 2. Default Gateway Connectivity

Command:

```cmd
ping 192.168.1.1
```

Result: Successful.

Observation: The workstation was able to communicate with its default gateway (pfSense LAN interface).

### 3. Domain Controller Connectivity

Command:

```cmd
ping 192.168.1.101
```

Result: Success.

Observation: The workstation received ICMP replies from the Domain Controller/DNS server.

### 4. External IP Connectivity

Command:

```cmd
ping 8.8.8.8
```

Result: Request timed out.

Observation: No ICMP replies were received from the external IP during testing.

### 5. IP Configuration Analysis

Command:

```cmd
ipconfig /all
```

Observed configuration:

- IPv4 Address: 192.168.1.106
- Subnet Mask: 255.255.255.0
- Default Gateway: 192.168.1.1
- DHCP Server: 192.168.1.1
- DNS Server: 192.168.1.101
- DNS Suffix: soc.lab

Observation: The workstation had an assigned IPv4 address, gateway and internal DNS server.

### 6. DNS Resolution Test with internal (DC) and external site

Command:

```cmd
nslookup soc-ad.soc.lab 192.168.1.101
nslookup google.com 192.168.1.101
```

Result: DNS resolution with internal Domain controller success but external site failed.

Observation: The workstation could not resolve the requested external hostname.

### 7. Firewall Rule Inspection

Inspected the relevant pfSense firewall rules and ACL configuration to check whether the workstation's IP address was explicitly blocked.

Result: No matching block rule was identified during the inspection.

## Findings and Analysis

The investigation identified the following:

- Local TCP/IP loopback test was successful.
- Default gateway connectivity was successful.
- Domain Controller ICMP connectivity failed.
- External ICMP connectivity failed.
- DNS resolution failed.
- No matching firewall block rule was identified.

The external network outage is a possible explanation for the external connectivity failure. 

## Conclusion

The workstation experienced an internet connectivity failure due to a possible network outage.

The troubleshooting process helped identify the failure symptoms and eliminate some initial possibilities.

The exact status of Domain Controller connectivity and the DNS forwarding path was not independently confirmed.

Further testing would be required after ISP service restoration to establish whether any additional local network or DNS issues remain.

## Tools and Commands Used

| Tool / Command | Purpose |
|---|---|
| `ping` | Test IP connectivity |
| `ipconfig /all` | Inspect network configuration |
| `nslookup` | Diagnose DNS resolution |
| pfSense Firewall | Inspect firewall rules |
| Windows CMD | Network diagnostics |
| PowerShell | Additional connectivity testing |

## Skills Demonstrated

- Basic network troubleshooting
- IPv4 addressing and subnetting
- TCP/IP diagnostics
- DNS troubleshooting
- Active Directory DNS awareness
- Firewall rule inspection
- Windows command-line tools
- Incident analysis and technical documentation

