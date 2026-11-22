# Wireshark: Packet Operations

## About This Room

In this room, I learned how to use Wireshark to analyse network traffic and investigate packets.

I worked with a PCAP file called `Exercise.pcapng` and used different options in Wireshark to find information about IP addresses, network conversations, DNS requests and HTTP traffic.

This room helped me get more comfortable with packet analysis and using filters to find specific information.

## Tools Used

- Wireshark
- TryHackMe AttackBox
- PCAP file (`Exercise.pcapng`)

## What I Learned

- How to check network traffic using Wireshark.
- How to view IP addresses and network conversations.
- How to use the Statistics menu.
- How to check DNS and HTTP traffic.
- How to use display filters to find specific packets.
- How to filter IP, TCP and UDP traffic.

## My Practical Work

### 1. Network Traffic Investigation

I opened the `Exercise.pcapng` file in Wireshark and explored the Statistics menu to find information about the captured traffic.

My answers:

| Investigation | Result |
|---|---|
| IP address of hostname starting with bbc | `199.232.24.81` |
| Number of IPv4 conversations | 435 |
| Bytes transferred from Micro-St MAC address | 7,474 |
| IP addresses linked with Kansas City | 4 |
| IP address linked with Blicnet | `188.246.82.7` |

### 2. IPv4, DNS and HTTP Statistics

I used the Statistics menu to check IPv4, DNS and HTTP information.

My findings:

| Question | Answer |
|---|---|
| Most used IPv4 destination address | `10.100.1.33` |
| Maximum DNS request-response time | `0.467897` seconds |

### 3. Wireshark Display Filters

I practised different filters to find specific packets in the capture.

**IP filters**

```text
ip
ip.addr == 10.10.10.111
ip.addr == 10.10.10.0/24
ip.src == 10.10.10.111
ip.dst == 10.10.10.111
```

**TCP and UDP filters**

```text
tcp.port == 80
udp.port == 53
tcp.srcport == 1234
udp.srcport == 1234
tcp.dstport == 80
udp.dstport == 5353
```

**HTTP and DNS filters**

```text
http
dns
http.response.code == 200
dns.flags.response == 0
http.request.method == "GET"
http.request.method == "POST"
dns.flags.response == 1
dns.qry.type == 1
```

### 4. Packet Filtering Results

I used different display filters to answer questions about the captured packets.

| Investigation | Filter used | Result |
|---|---|---|
| Total IP packets | IP statistics | 81,420 |
| Packets with TTL less than 10 | `ip.ttl < 10` | 66 |
| Packets using TCP port 4444 | `tcp.port == 4444` | 632 |
| HTTP GET requests to port 80 | `http.request.method == "GET" && tcp.port == 80` | 527 |
| DNS A record responses | `dns.qry.type == 1 && dns.flags.response == 1` | 51 |

### Screenshots

I will add screenshots of my practical work here, including the Wireshark statistics and filters I used to find the answers.

### What I Learned

This room gave me more practice using Wireshark and understanding network traffic.

I learned how to check conversations between devices, look at DNS and HTTP information and use filters to find particular packets.

I also learned that filters make it much easier to investigate large PCAP files instead of checking every packet manually.

### Conclusion

I found this room useful because it gave me hands-on practice with packet analysis. I am now more comfortable using Wireshark statistics and display filters to investigate network traffic.

I will continue practising these skills as I work towards becoming a SOC Analyst.

**Platform:** TryHackMe

**Room:** [Wireshark: Packet Operations](https://tryhackme.com/room/wiresharkpacketoperations)

**Status:** Completed
