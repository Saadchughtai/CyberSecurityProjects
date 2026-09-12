DNS & ICMP Network Traffic Analysis

Cybersecurity Incident Investigation | tcpdump | DNS | UDP | ICMP

Project Overview

This project documents a simulated cybersecurity incident investigation completed as part of the Google Cybersecurity Professional Certificate. The activity focused on network traffic analysis and determining why users could not access a website after receiving a "destination port unreachable" error.

Using tcpdump packet-capture data, I analyzed DNS, UDP, and ICMP traffic to identify the affected service, interpret the network error, and develop likely root-cause hypotheses and recommended troubleshooting actions.

Note: This was a hands-on certificate lab completed for skills development and portfolio demonstration. It was not a production incident.

Recruiter Snapshot

Role demonstrated: Cybersecurity / SOC Analyst
Primary tool: tcpdump
Protocols analyzed: DNS, UDP, ICMP, HTTPS
Affected service: DNS
Affected port: UDP/53
Key finding: DNS queries could not reach a listening DNS service, preventing hostname resolution and stopping the browser from progressing to the HTTPS connection.

Scenario

Users reported that they were unable to access:

www.yummyrecipesforme.com

When attempting to load the website, they received the error:

destination port unreachable

To reproduce and investigate the issue, I analyzed network traffic generated while attempting to load the webpage.

The expected connection flow was:

User Browser
    |
    | DNS query over UDP/53
    v
DNS Server
    |
    | Returns website IP address
    v
User Browser
    |
    | HTTPS request over TCP/443
    v
Web Server

However, the DNS lookup failed before the browser could establish the HTTPS connection.

Investigation Objectives

The investigation focused on answering the following questions:

Which network protocol or service was affected?

What did the packet capture reveal about the failed connection?

Which source and destination systems were involved?

Which port was unreachable?

Why was the website inaccessible?

What were the most likely causes?

What should be investigated next?

Tools and Technologies

Technology

Purpose

tcpdump

Captured and analyzed network packets

DNS

Resolves domain names to IP addresses

UDP

Transport protocol used for the DNS query in this scenario

ICMP

Returned the network error message

HTTPS

Intended application-layer connection after successful DNS resolution

TCP/IP

Framework used to interpret packet flow and communication

Incident Timeline

The first observed packet activity occurred at approximately:

13:24:32.192571 — 1:24 p.m.

At this point, the client attempted to send a DNS query for the website.

Systems Involved

System

IP Address

Role

Client workstation

192.51.100.15

Sent the DNS request

DNS server

203.0.113.2

Intended to process the DNS query

Destination service

UDP/53

DNS service

Packet Analysis

The client workstation attempted to resolve the website domain name by sending a DNS request to the DNS server.

Expected behavior

192.51.100.15
    |
    | UDP DNS query
    | Destination port: 53
    v
203.0.113.2
    |
    | DNS response containing IP address
    v
192.51.100.15

Observed behavior

192.51.100.15
    |
    | UDP DNS query to port 53
    v
203.0.113.2
    |
    | ICMP error:
    | "udp port 53 unreachable"
    v
192.51.100.15

Instead of receiving a DNS response, the client received an ICMP destination/port unreachable message.

The same failure occurred multiple times, indicating that the problem was persistent rather than a single dropped packet.

Key Findings

1. DNS was the affected service

The browser needed to resolve www.yummyrecipesforme.com to an IP address before it could connect to the website.

Because DNS resolution failed, the browser could not determine the destination IP address of the web server.

2. UDP port 53 was unreachable

The packet capture showed the error:

udp port 53 unreachable

UDP port 53 is commonly used by DNS.

This indicated that the DNS request reached a point where the requested UDP service was unavailable or inaccessible.

3. ICMP reported the failure

ICMP was not the original service being requested. Instead, ICMP was used to communicate the network error back to the client.

This distinction is important:

DNS query        -> UDP/53
Error response   -> ICMP

4. HTTPS was not the initial failure point

Although the user was trying to visit a secure website, the browser could not proceed to the HTTPS stage because DNS resolution failed first.

Therefore, the evidence did not indicate that TCP port 443 was the primary affected service.

Root-Cause Analysis

Based on the packet evidence, the most likely cause was that the DNS service on the destination server was unavailable or not listening on UDP port 53.

Other reasonable possibilities include:

A firewall rule blocking UDP port 53

The DNS service being stopped or misconfigured

A DNS server outage

A network access-control rule preventing DNS communication

A service crash or other server-side failure

A security incident affecting DNS availability

The packet capture alone is not sufficient to prove that the outage was caused by a malicious attack. Additional server, firewall, and security logs would be required before making that conclusion.

Recommended Next Steps

DNS service validation

Confirm that the DNS service is running and actively listening on UDP port 53.

Example Linux checks:

sudo ss -ulnp | grep ':53'

or:

sudo netstat -ulnp | grep ':53'

Firewall review

Inspect firewall rules for UDP port 53.

Example:

sudo iptables -L -n -v

or, on systems using UFW:

sudo ufw status

DNS testing

Use DNS utilities to test whether the server responds correctly:

nslookup www.yummyrecipesforme.com

dig www.yummyrecipesforme.com

A specific DNS server could also be tested directly:

dig @203.0.113.2 www.yummyrecipesforme.com

Service logs

Review DNS service logs for:

Startup failures

Configuration errors

Unexpected shutdowns

Resource exhaustion

Repeated connection attempts

Indicators of malicious activity

Network validation

Test connectivity between the client and DNS server and confirm that routing and security controls permit the required traffic.

Incident Summary

Category

Finding

Incident type

Network/DNS availability issue

User symptom

Website inaccessible

Observed error

Destination port unreachable

Client IP

192.51.100.15

DNS server IP

203.0.113.2

Affected protocol

DNS over UDP

Affected port

UDP/53

Error protocol

ICMP

Key ICMP message

udp port 53 unreachable

Impact

Domain name could not be resolved

HTTPS reached?

No; DNS failed first

Likely cause

DNS service unavailable/not listening, or UDP/53 blocked

Skills Demonstrated

Network packet analysis

tcpdump log interpretation

TCP/IP troubleshooting

DNS troubleshooting

UDP traffic analysis

ICMP error interpretation

Port and protocol identification

Incident triage

Root-cause hypothesis development

Evidence-based cybersecurity reporting

Distinguishing symptoms from root cause

Developing remediation and troubleshooting steps

What I Learned

This project reinforced an important troubleshooting principle: follow the network connection in the order it occurs.

A browser may appear to have an HTTPS problem because a website will not load, but HTTPS cannot begin until DNS successfully resolves the domain name.

By analyzing the packet sequence rather than focusing only on the visible browser error, I identified the actual failure point as the DNS service on UDP port 53.

I also learned the importance of separating confirmed evidence from hypotheses. The packet capture confirmed that UDP port 53 was unreachable, but it did not prove whether the cause was a firewall rule, a stopped service, a server failure, or an attack.

Security Analyst Takeaway

A strong incident investigation should answer four questions:

What happened?
    ↓
Where did the communication fail?
    ↓
What evidence proves the failure point?
    ↓
What should be checked next?

In this incident:

Website failed to load
        ↓
DNS resolution failed
        ↓
ICMP reported UDP port 53 unreachable
        ↓
Investigate DNS service + firewall + server logs

Suggested Repository Structure

dns-icmp-network-traffic-analysis/
│
├── README.md
├── reports/
│   └── cybersecurity-incident-report.pdf
├── screenshots/
│   └── tcpdump-analysis.png
└── notes/
    └── investigation-notes.md

Resume / Portfolio Description

DNS & ICMP Network Traffic Analysis — Google Cybersecurity Professional Certificate Lab

Completed a hands-on network traffic analysis activity as part of the Google Cybersecurity Professional Certificate. Analyzed simulated tcpdump data to investigate a website availability incident, identified failed DNS queries over UDP port 53, and interpreted ICMP "port unreachable" responses. Determined that DNS resolution was preventing the browser from progressing to HTTPS, documented the systems and protocols involved, developed root-cause hypotheses, and proposed DNS service, firewall, and logging checks for further investigation.

Interview Talking Points

If asked about this project in an interview, I would explain:

I investigated a simulated website-access incident using tcpdump. The browser appeared unable to reach the website, but packet analysis showed that the failure occurred before HTTPS. The client sent DNS queries over UDP port 53 to the DNS server and received ICMP "port unreachable" responses. That prevented domain-name resolution, so the browser never reached the HTTPS connection stage. I identified DNS as the affected service and recommended checking whether the DNS service was running, whether UDP/53 was blocked by a firewall, and whether server logs showed configuration issues or suspicious activity.

Project Context and Disclaimer

This repository documents a simulated cybersecurity training activity completed as part of the Google Cybersecurity Professional Certificate. It is included in my portfolio to demonstrate the network analysis and incident-investigation skills I practiced during the certificate program.

The IP addresses, domain names, packet data, and incident details come from the training scenario. This repository does not represent a claim that I responded to a real production incident, and it is not presented as original Google course material.
