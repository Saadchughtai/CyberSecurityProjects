# DNS & ICMP Network Traffic Analysis

A cybersecurity incident investigation project completed as part of the **Google Cybersecurity Professional Certificate**. This activity focused on analyzing network traffic with `tcpdump` to determine why users could not access a website.

## Project Overview

Users reported that they were unable to access `www.yummyrecipesforme.com` and received a **"destination port unreachable"** error.

To investigate the issue, I analyzed network traffic using `tcpdump`. The packet capture showed that the client was sending DNS requests over UDP port 53, but the DNS server returned an ICMP error stating:

```text
udp port 53 unreachable

This prevented the browser from resolving the website domain name into an IP address, so the browser could not continue to the HTTPS connection.

## Key Findings

* The affected service was DNS
* DNS requests used UDP port 53
* Client IP: `192.51.100.15`
* DNS server IP: `203.0.113.2`
* ICMP returned a `udp port 53 unreachable` error
* DNS resolution failed multiple times
* The website IP address could not be resolved
* HTTPS communication could not begin
* TCP port 443 was not the initial cause of the issue

## Network Flow

Expected traffic:

```text
Browser
   |
   | DNS Query - UDP Port 53
   v
DNS Server
   |
   | Returns IP Address
   v
Browser
   |
   | HTTPS Request - TCP Port 443
   v
Web Server
```

Observed traffic:

```text
Client: 192.51.100.15
   |
   | DNS Query - UDP Port 53
   v
DNS Server: 203.0.113.2
   |
   | ICMP Error
   | "udp port 53 unreachable"
   v
Client
```

## Root Cause Analysis

The most likely cause was that the DNS service was unavailable or was not listening on UDP port 53.

Other possible causes included:

* Firewall blocking UDP port 53
* DNS service misconfiguration
* DNS server outage
* DNS service failure
* Network access-control rules blocking DNS traffic
* Server-side configuration issues

The packet capture confirmed that UDP port 53 was unreachable, but more evidence would be required to determine the exact cause.

## Recommended Troubleshooting

Possible next steps include:

* Check that the DNS service is running
* Confirm that UDP port 53 is listening
* Review firewall rules
* Check DNS server configuration
* Review DNS and system logs
* Test DNS resolution
* Check network connectivity

Example commands:

```bash
sudo ss -ulnp | grep ':53'
```

```bash
sudo iptables -L -n -v
```

```bash
nslookup www.yummyrecipesforme.com
```

```bash
dig www.yummyrecipesforme.com
```

```bash
dig @203.0.113.2 www.yummyrecipesforme.com
```

## Tools & Technologies

* tcpdump
* DNS
* UDP
* ICMP
* HTTPS
* TCP/IP
* Linux networking

## Skills Demonstrated

* Network traffic analysis
* Packet analysis
* tcpdump log interpretation
* DNS troubleshooting
* UDP traffic analysis
* ICMP error interpretation
* Port and protocol identification
* Incident investigation
* Root cause analysis
* Network troubleshooting
* Cybersecurity incident reporting

## What I Learned

This project helped me understand the importance of following network communication step by step.

Although the website appeared to have an HTTPS problem, the actual issue happened before HTTPS could begin.

The browser first needed DNS to resolve the domain name into an IP address.

```text
Website access attempted
        ↓
DNS query sent over UDP port 53
        ↓
ICMP reports port 53 unreachable
        ↓
DNS resolution fails
        ↓
Website IP address is not obtained
        ↓
HTTPS connection cannot begin
```

I also learned how important it is to separate confirmed evidence from possible causes. The packet capture confirmed that UDP port 53 was unreachable, but further investigation would be needed to determine whether the cause was a firewall rule, DNS service failure, server outage, or another issue.

## Project Structure

```text
dns-icmp-network-traffic-analysis/
├── README.md
├── reports/
│   └── cybersecurity-incident-report.pdf
└── screenshots/
    └── tcpdump-analysis.png
```

## Certificate

This project was completed as part of the **Google Cybersecurity Professional Certificate** and demonstrates practical experience with network traffic analysis and cybersecurity incident investigation.

## Disclaimer

This project is based on a simulated cybersecurity training scenario. The IP addresses, domain name, packet data, and incident details are part of the educational activity and do not represent a real production security incident.

## License

This project is for educational, portfolio, and learning purposes.

```

For GitHub, use:

**Repository name:** `dns-icmp-network-traffic-analysis`

**Description:**  
`Cybersecurity network traffic analysis project using tcpdump to investigate DNS, UDP port 53, and ICMP port-unreachable errors.`

**Topics:**  
`cybersecurity` `network-security` `tcpdump` `dns` `icmp` `udp` `packet-analysis` `incident-response` `google-cybersecurity-certificate`
```
