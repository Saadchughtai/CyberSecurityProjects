
````markdown
# SYN Flood Attack Analysis

A cybersecurity incident analysis project completed as part of the **Google Cybersecurity Professional Certificate**. This activity focuses on identifying a **TCP SYN flood denial-of-service (DoS) attack**, understanding how it disrupts normal TCP connections, and explaining how the attack affected website availability.

## Features

- Network attack identification
- TCP three-way handshake analysis
- SYN flood investigation
- Wireshark/TCP traffic analysis
- Web server availability analysis
- Firewall mitigation
- Incident response
- Root cause analysis
- Network security recommendations

## Incident Scenario

A travel agency used its website to advertise vacation packages and promotions. Employees regularly accessed the website to help customers find suitable travel deals.

One afternoon, the network monitoring system generated an automated alert indicating a problem with the web server.

When the website was tested, the browser returned:

```text
connection timeout
````

A packet capture was then reviewed, and a large number of **TCP SYN packets** were observed coming from an unfamiliar IP address.

The web server became overwhelmed by the high number of incoming connection requests and was no longer able to respond normally to legitimate users.

## Attack Identified

The incident was identified as a:

```text
TCP SYN Flood DoS Attack
```

A SYN flood attack takes advantage of the TCP three-way handshake.

A normal TCP connection works like this:

```text
Client            Server
  |                  |
  |------ SYN ------>|
  |<--- SYN-ACK -----|
  |------ ACK ------>|
  |                  |
Connection Established
```

During a SYN flood attack, the attacker sends a large number of SYN packets but does not complete the handshake.

```text
Attacker          Server
  |                  |
  |------ SYN ------>|
  |<--- SYN-ACK -----|
  |                  |
  |------ SYN ------>|
  |<--- SYN-ACK -----|
  |                  |
  |------ SYN ------>|
  |<--- SYN-ACK -----|
  |                  |
Connections remain incomplete
```

The server keeps resources reserved while waiting for the final ACK packets. When too many connections remain half-open, the server may run out of resources.

## Key Findings

* A large number of TCP SYN packets were sent to the server
* The requests came from an unfamiliar IP address
* The server responded with SYN-ACK packets
* Many TCP connections were not completed
* Server resources became overloaded
* Legitimate connections began to fail
* Users experienced connection timeouts
* Website availability was affected

## How the Attack Affected the Website

The SYN flood caused the server to use resources for incomplete TCP connections.

As the number of half-open connections increased, fewer resources were available for legitimate users.

This resulted in:

* Slow website response times
* Failed TCP connections
* Browser connection timeouts
* Temporary website unavailability
* Reduced access for employees and customers

## Business Impact

The attack interrupted access to an important business service.

Possible consequences included:

* Employees being unable to access travel deals
* Customers being unable to browse promotions
* Reduced productivity
* Lost sales opportunities
* Poor customer experience
* Possible revenue loss
* Damage to the company's reputation
* Increased workload for the IT and security teams

## Incident Response

The web server was temporarily taken offline so that it could recover and return to normal operation.

The firewall was also configured to block the IP address responsible for the abnormal number of SYN requests.

```text
Malicious IP
     |
     | SYN Flood
     v
  Firewall
     |
     | Blocked
     X
 Web Server
```

Blocking the IP address provided short-term protection, but it is not a complete solution because attackers can change or spoof source IP addresses.

## Recommended Security Improvements

To reduce the risk of future SYN flood attacks, the organization could use:

* SYN cookies
* Rate limiting
* Firewall connection limits
* Intrusion detection systems
* Intrusion prevention systems
* DDoS protection services
* Network traffic monitoring
* Automated alerts for unusual SYN traffic
* Load balancing
* Continuous server and network monitoring

## Tools & Technologies

* Wireshark
* TCP/IP
* TCP SYN
* HTTP/HTTPS
* Firewalls
* Network monitoring
* Packet analysis

## Skills Practiced

* Network traffic analysis
* Packet inspection
* TCP handshake analysis
* DoS attack identification
* SYN flood analysis
* Web server troubleshooting
* Incident response
* Firewall security concepts
* Root cause analysis
* Cybersecurity incident reporting

## Learning Objectives

* Understand the TCP three-way handshake
* Recognize the characteristics of a SYN flood attack
* Identify abnormal TCP traffic patterns
* Understand how DoS attacks affect server availability
* Analyze incomplete TCP connections
* Understand the business impact of network attacks
* Recommend security controls to reduce future risk

## Attack Flow

```text
Attacker
   |
   | Large number of SYN packets
   v
Web Server
   |
   | Sends SYN-ACK responses
   v
Connections remain incomplete
   |
   | Server resources are consumed
   v
Legitimate users cannot connect
   |
   v
Website becomes slow or unavailable
```

## DoS vs DDoS

A **Denial-of-Service (DoS)** attack normally uses one source to overwhelm a system.

A **Distributed Denial-of-Service (DDoS)** attack uses multiple systems or devices to generate attack traffic.

In this scenario, the traffic was described as coming from an unfamiliar IP address, so the available evidence most directly supports a **SYN flood DoS attack**.

## Project Structure

```text
syn-flood-attack-analysis/
├── README.md
├── reports/
│   └── cybersecurity-incident-report.pdf
├── logs/
│   └── wireshark-tcp-http-log
└── screenshots/
```

## Screenshots

Add screenshots of the Wireshark traffic, incident report, or analysis here.

Example:

```html
<img width="1200" alt="Wireshark SYN flood analysis" src="YOUR-GITHUB-IMAGE-LINK" />
```

## Portfolio Summary

**SYN Flood Attack Analysis — Google Cybersecurity Professional Certificate**

Completed a hands-on cybersecurity incident analysis activity using Wireshark network traffic to investigate a simulated website availability issue. Identified a TCP SYN flood DoS attack, analyzed incomplete TCP handshakes, explained how server resources became overloaded, and documented the impact on legitimate users. Recommended firewall controls, SYN protection, rate limiting, and network monitoring as possible security improvements.

## Certificate

This project was completed as part of the **Google Cybersecurity Professional Certificate** and demonstrates practical knowledge of TCP/IP, network attacks, incident response, packet analysis, and network security.

## Disclaimer

This project is based on a simulated cybersecurity training scenario. The organization, network traffic, and incident details are part of an educational activity and do not represent a real production security incident.

## License

This project is for educational, portfolio, and learning purposes.

```

A good GitHub repository name would be:

**`syn-flood-attack-analysis`**

And the description:

**Cybersecurity incident analysis project using Wireshark to investigate a TCP SYN flood DoS attack as part of the Google Cybersecurity Professional Certificate.**
```
