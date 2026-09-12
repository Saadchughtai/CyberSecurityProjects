
# Network Traffic Analysis

A beginner-friendly cybersecurity network analysis project completed as part of the **Google Cybersecurity Professional Certificate**. This project focuses on analyzing DNS and ICMP traffic using `tcpdump` to investigate why users were unable to access a website.

## Features

- Network traffic analysis with tcpdump
- DNS traffic investigation
- UDP port analysis
- ICMP error interpretation
- Source and destination IP identification
- Port and protocol identification
- Incident investigation
- Root cause analysis
- Network troubleshooting

## Incident Scenario

Users reported that they were unable to access `www.yummyrecipesforme.com` and received a:

```text
destination port unreachable
````

error.

To investigate the issue, network traffic was analyzed using `tcpdump`.

The client computer attempted to send a DNS request to the DNS server using UDP port `53`.

Instead of receiving a DNS response, the client received an ICMP error:

```text
udp port 53 unreachable
```

This prevented the browser from resolving the website's domain name into an IP address and therefore prevented the HTTPS connection from starting.

## Network Details

| Component         | Details                   |
| ----------------- | ------------------------- |
| Client IP         | `192.51.100.15`           |
| DNS Server IP     | `203.0.113.2`             |
| Affected Protocol | DNS over UDP              |
| Affected Port     | UDP `53`                  |
| Error Protocol    | ICMP                      |
| Error Message     | `udp port 53 unreachable` |
| Incident Time     | `13:24:32.192571`         |

## Traffic Flow

Expected traffic:

```text
Client
  |
  | DNS Query - UDP Port 53
  v
DNS Server
  |
  | DNS Response
  v
Client
  |
  | HTTPS Request - Port 443
  v
Web Server
```

Observed traffic:

```text
Client
  |
  | DNS Query - UDP Port 53
  v
DNS Server
  |
  | ICMP Error
  | "udp port 53 unreachable"
  v
Client
```

## Key Findings

* The affected service was **DNS**
* DNS requests were sent using **UDP port 53**
* The client IP address was `192.51.100.15`
* The DNS server IP address was `203.0.113.2`
* ICMP returned a `udp port 53 unreachable` error
* The DNS request failed multiple times
* The browser could not resolve the website domain name
* The HTTPS connection could not begin because DNS resolution failed first
* Port `443` was not identified as the original point of failure

## Possible Cause

A likely cause of the incident was that the DNS service was unavailable or was not listening on UDP port `53`.

Other possible causes include:

* Firewall blocking UDP port 53
* DNS server outage
* DNS service misconfiguration
* DNS service failure
* Network configuration issue

The available packet data does not provide enough evidence to confirm whether the incident was caused by a malicious attack.

## Recommended Troubleshooting

Further investigation should include:

* Verify that the DNS service is running
* Confirm that UDP port 53 is listening
* Review firewall rules
* Check DNS server configuration
* Review DNS server logs
* Test DNS resolution
* Check network connectivity between the client and DNS server

Useful commands may include:

```bash
nslookup www.yummyrecipesforme.com
```

```bash
dig www.yummyrecipesforme.com
```

```bash
sudo ss -ulnp | grep ':53'
```

```bash
sudo iptables -L -n -v
```

## Tools & Technologies

* tcpdump
* DNS
* UDP
* ICMP
* HTTPS
* TCP/IP
* Linux networking

## Skills Practiced

* Network traffic analysis
* Packet analysis
* DNS troubleshooting
* ICMP error analysis
* Port identification
* Protocol analysis
* Incident investigation
* Root cause analysis
* Network troubleshooting
* Cybersecurity incident documentation

## Learning Objectives

* Understand how DNS traffic works
* Analyze network packets using tcpdump
* Identify source and destination IP addresses
* Recognize common network ports
* Understand ICMP error messages
* Identify where network communication fails
* Distinguish DNS failures from HTTPS failures
* Develop possible root causes based on packet evidence
* Document findings from a cybersecurity investigation

## Project Structure

```text
network-traffic-analysis/
├── README.md
├── Cybersecurity-incident-report-network-traffic-analysis.pdf
└── screenshots/
```

## Screenshots

Add screenshots of the tcpdump log or completed incident report here.

Example:

```html
<img width="1200" alt="tcpdump network traffic analysis" src="YOUR-GITHUB-IMAGE-LINK" />
```

## Certificate

This project was completed as part of the **Google Cybersecurity Professional Certificate** and demonstrates practical experience with network traffic analysis and cybersecurity incident investigation.

## Disclaimer

This project is based on a simulated cybersecurity training scenario. The IP addresses, domain name, and incident details are part of the educational activity and do not represent a real production security incident.

## License

This project is for educational, portfolio, and learning purposes.

```

For the GitHub repository name, I recommend:

**`network-traffic-analysis`**

And for the short GitHub description:

> **Cybersecurity network traffic analysis project using tcpdump to investigate DNS, UDP port 53, and ICMP errors as part of the Google Cybersecurity Professional Certificate.**
```
