SYN Flood Attack Analysis

A beginner-friendly cybersecurity incident analysis project completed as part of the Google Cybersecurity Professional Certificate. This activity focuses on identifying and explaining a TCP SYN flood denial-of-service (DoS) attack that caused a company website to become slow and eventually unavailable.

Features

Network attack identification

TCP three-way handshake analysis

SYN flood investigation

Wireshark/TCP log interpretation

Web server availability analysis

Firewall mitigation

Incident response documentation

Root cause analysis

Network security recommendations

Incident Scenario

A travel agency relied on its website to advertise sales and vacation packages. Employees regularly used the site to find offers for customers.

One afternoon, the monitoring system generated an automated alert indicating a problem with the web server. When the website was tested, the browser returned a:

connection timeout

A packet capture showed a large number of TCP SYN requests being sent to the web server from an unfamiliar IP address.

The server became overwhelmed by the volume of incoming connection requests and was no longer able to respond normally to legitimate users.

Attack Identified

The incident was identified as a:

TCP SYN Flood DoS Attack

A SYN flood abuses the TCP three-way handshake by sending a large number of SYN connection requests without properly completing the connection process.

A normal TCP handshake works like this:

Client            Server
  |                  |
  |------ SYN ------>|
  |<--- SYN-ACK -----|
  |------ ACK ------>|
  |                  |
Connection Established

During a SYN flood attack:

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
Many connections remain incomplete

The server continues reserving resources for these half-open connections while waiting for final ACK packets that may never arrive.

Key Findings

A large number of TCP SYN packets were sent to the web server

The traffic came from an unfamiliar source

The web server attempted to respond to the connection requests

Many TCP connections were left incomplete

Server resources became overloaded

Legitimate connections began to fail

Employees and customers experienced connection timeouts

Website availability was negatively affected

How the Attack Affected the Website

The SYN flood consumed the web server's available connection resources.

Because the server had to keep track of many incomplete TCP connections, it had fewer resources available for legitimate users.

This caused:

Slow website response times

Failed TCP connections

Browser connection timeouts

Temporary website unavailability

Reduced access for employees and customers

Business Impact

The attack negatively affected the organization by interrupting access to an important business service.

Potential consequences included:

Employees being unable to access travel deals

Customers being unable to browse promotions

Reduced productivity

Lost sales opportunities

Poor customer experience

Possible revenue loss

Damage to the company's reputation

Increased workload for IT and security teams

Incident Response

The server was temporarily taken offline so that it could recover and return to a normal operating state.

The firewall was also configured to block the IP address that was generating the abnormal number of SYN requests.

Malicious IP
     |
     | SYN Flood
     v
  Firewall
     |
     | Blocked
     X
 Web Server

Blocking the IP address provided short-term protection, but it is not a complete solution because an attacker may use another IP address or spoof source addresses.

Recommended Security Improvements

To reduce the risk of future SYN flood attacks, the organization could implement:

SYN cookies

Rate limiting

Firewall connection limits

Intrusion detection and prevention systems

DDoS protection services

Network traffic monitoring

Automated alerting for abnormal SYN traffic

Load balancing

Web application and network security monitoring

Tools & Technologies

Wireshark

TCP/IP

TCP SYN

HTTP/HTTPS

Firewalls

Network monitoring

Packet analysis

Skills Practiced

Network traffic analysis

Packet inspection

TCP handshake analysis

DoS attack identification

SYN flood analysis

Web server troubleshooting

Incident response

Firewall configuration concepts

Root cause analysis

Cybersecurity incident reporting

Learning Objectives

Understand how the TCP three-way handshake works

Recognize the characteristics of a SYN flood attack

Identify abnormal TCP traffic patterns

Understand how DoS attacks affect server availability

Analyze how incomplete TCP connections consume resources

Explain the business impact of a network attack

Recommend basic controls to reduce future risk

Attack Flow

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
Website slows down or times out

DoS vs DDoS

A Denial-of-Service (DoS) attack typically comes from a single source.

A Distributed Denial-of-Service (DDoS) attack uses multiple systems or sources to generate attack traffic.

In this activity, the scenario describes a large number of SYN requests coming from an unfamiliar IP address, so the evidence most directly supports a SYN flood DoS attack.

Project Structure

syn-flood-attack-analysis/
├── README.md
├── reports/
│   └── cybersecurity-incident-report.pdf
├── logs/
│   └── wireshark-tcp-http-log
└── screenshots/

Screenshots

Add screenshots of your Wireshark log, incident report, or analysis here.

Example:

<img width="1200" alt="Wireshark SYN flood analysis" src="YOUR-GITHUB-IMAGE-LINK" />

Certificate

This project was completed as part of the Google Cybersecurity Professional Certificate and demonstrates practical experience with network attack analysis, TCP/IP concepts, incident response, and network security.

Disclaimer

This project is based on a simulated cybersecurity training scenario. The network activity, organization, and incident details are part of an educational exercise and do not represent a real production security incident.

License

This project is for educational, portfolio, and learning purposes.
