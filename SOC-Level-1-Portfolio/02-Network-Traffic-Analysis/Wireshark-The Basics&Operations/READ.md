# Wireshark: Packet Operations

## About This Room

In this room, I learned how to use Wireshark to analyse network traffic and investigate packets.

I worked with a PCAP file called `Exercise.pcapng` and used different options in Wireshark to find information about IP addresses, network conversations, DNS requests and HTTP traffic.

I also practised basic and advanced display filters, used filter buttons and explored Wireshark profiles.

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
- How to use basic and advanced display filters.
- How to filter IP, TCP and UDP traffic.
- How to use operators such as `contains`, `matches` and `in`.
- How to use Wireshark bookmarks and filter buttons.
- How to create and change Wireshark profiles.

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

### 3. Basic Wireshark Display Filters

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
|---|---|---:|
| Total IP packets | IP statistics | 81,420 |
| Packets with TTL less than 10 | `ip.ttl < 10` | 66 |
| Packets using TCP port 4444 | `tcp.port == 4444` | 632 |
| HTTP GET requests to port 80 | `http.request.method == "GET" && tcp.port == 80` | 527 |
| DNS A record responses | `dns.qry.type == 1 && dns.flags.response == 1` | 51 |

### 5. Advanced Filtering

I also learned how to use advanced Wireshark filters to search for specific information in packets.

Some of the operators and functions I practised were:

| Operator or function | What it does |
|---|---|
| `contains` | Searches for specific text in a field |
| `matches` | Searches using a regular expression |
| `in` | Checks whether a value is within a list |
| `upper()` | Changes text to uppercase |
| `lower()` | Changes text to lowercase |
| `string()` | Converts a value into a string |

**Examples of advanced filters:**

```text
http.server contains "Apache"
http.host matches "\.(php|html)"
tcp.port in {80 443 8080}
upper(http.server) contains "APACHE"
lower(http.server) contains "apache"
string(frame.number) matches "[13579]$"
```

I used these filters to practise searching for specific server names, hostnames, port numbers and packet numbers.

### 6. Advanced Filtering Tasks and Results

I applied the advanced filters to answer the room's questions.

| Investigation | Filter or action | Result |
|---|---|---:|
| Microsoft IIS packets that did not originate from port 80 | `http.server contains "IIS" && !tcp.srcport == 80` | 21 packets |
| Microsoft IIS packets with version 7.5 | `http.server contains "IIS" && http.server matches "7.5"` | 71 packets |
| Packets using ports 3333, 4444 or 9999 | `tcp.port in {3333 4444 9999}` | 2,235 packets |
| Packets with even TTL values | `string(ip.ttl) matches "[24680]$"` | 77,289 packets |
| Packets with bad TCP checksums | `tcp.checksum.status == bad` | 34,185 packets |

### 7. Wireshark Profiles and Filter Buttons

I learned that Wireshark allows users to save filters as bookmarks and buttons so they can be used again without typing the same filter each time.

I also explored Wireshark profiles, which allow different settings to be saved for different types of investigations.

For the checksum task, I opened **Edit → Configuration Profiles** and selected the existing **Checksum Control** profile. I then used this filter:

```text
tcp.checksum.status == bad
```

Wireshark displayed 34,185 packets with bad TCP checksums.

I also clicked the existing filter button called **gif/jpeg with http-200** to apply its saved filter.

| Task | Result |
|---|---|
| Packets displayed using `gif/jpeg with http-200` | To be added |

## Screenshots

The screenshots below show my practical work in Wireshark, including network statistics, display filters and packet analysis.

<img width="2078" height="1673" alt="image" src="https://github.com/user-attachments/assets/639fa36a-b30c-490c-a012-490ab97ccb7a" />

<img width="2081" height="1676" alt="image" src="https://github.com/user-attachments/assets/0a14a068-984c-439a-8ff9-37c85a305880" />

<img width="2084" height="1679" alt="image" src="https://github.com/user-attachments/assets/42113477-c217-4450-b198-695c595e160a" />

<img width="2086" height="1676" alt="image" src="https://github.com/user-attachments/assets/bd86543e-fd3a-42dd-8e60-7143c35f3208" />

<img width="2088" height="1677" alt="image" src="https://github.com/user-attachments/assets/efb28ce8-23d0-4454-bb3c-aeb2649648d8" />

<img width="2081" height="1674" alt="image" src="https://github.com/user-attachments/assets/c8f89739-4b04-4861-8023-b3e35776423a" />

<img width="2082" height="1682" alt="image" src="https://github.com/user-attachments/assets/1fce94a1-bf75-45a7-8e5d-9096a3506296" />

<img width="2079" height="1676" alt="image" src="https://github.com/user-attachments/assets/b77205c0-8d0c-415f-b6e6-986595b2d617" />

<img width="2080" height="1675" alt="image" src="https://github.com/user-attachments/assets/53b32ed1-65c2-40be-8bbd-96513b3a8f52" />

<img width="2083" height="1674" alt="image" src="https://github.com/user-attachments/assets/cd1c52d9-b26a-408d-9494-5e7d28f1caf7" />

<img width="2078" height="1670" alt="image" src="https://github.com/user-attachments/assets/1f23a986-6f71-4950-81ab-7cd9a25702f3" />

<img width="2088" height="1671" alt="image" src="https://github.com/user-attachments/assets/219ca36a-7c0e-45cb-9152-dd4d8fbd625d" />

<img width="2086" height="1675" alt="image" src="https://github.com/user-attachments/assets/de799a49-5d9b-4812-b624-90e1ba482007" />

<img width="2085" height="1681" alt="image" src="https://github.com/user-attachments/assets/c60d4c83-d11f-4180-ab30-89b221327306" />

<img width="2085" height="1678" alt="image" src="https://github.com/user-attachments/assets/5e645619-b87f-4fca-9fc8-43636bd8a98a" />

<img width="1496" height="1675" alt="image" src="https://github.com/user-attachments/assets/4c31d9a3-066e-4cc6-a5d8-3d13350bb7f1" />

<img width="1498" height="1673" alt="image" src="https://github.com/user-attachments/assets/0bb45e68-b359-4843-90dd-ee47ea3263bf" />

<img width="1801" height="1680" alt="image" src="https://github.com/user-attachments/assets/1cc05bc8-cf3a-453e-9c4e-7264e3be211d" />

<img width="1805" height="1679" alt="image" src="https://github.com/user-attachments/assets/50e1b252-eda2-425d-8562-0ce544f129b2" />

<img width="1803" height="1669" alt="image" src="https://github.com/user-attachments/assets/cb781845-a5a0-4d6b-9e40-f7b53aac4487" />
<img width="1804" height="1676" alt="image" src="https://github.com/user-attachments/assets/e25bfb0e-767e-4441-86e3-dba4903c4d5b" />

<img width="1800" height="1678" alt="image" src="https://github.com/user-attachments/assets/411ae2e8-588f-42f5-add2-30bfc76ac463" />
<img width="1799" height="1681" alt="image" src="https://github.com/user-attachments/assets/4a948269-78cb-44f9-9f03-4fcf32126a22" />
<img width="1800" height="1677" alt="image" src="https://github.com/user-attachments/assets/457bdac4-58d3-463b-9c4e-2a73cd5f938f" />
<img width="1806" height="1679" alt="image" src="https://github.com/user-attachments/assets/37623c74-875f-4d0b-a0f5-d69d3cc50e37" />

<img width="1803" height="1676" alt="image" src="https://github.com/user-attachments/assets/2953dbd5-d556-4eed-8995-873469236f41" />

<img width="1803" height="1680" alt="image" src="https://github.com/user-attachments/assets/1c589dbb-232b-4b6c-94cb-f697f6315574" />
<img width="1801" height="1685" alt="image" src="https://github.com/user-attachments/assets/1c20c0a3-9236-4590-8986-35feec17aaf1" />




## What I Learned

This room gave me more practice using Wireshark and understanding network traffic.

I learned how to check conversations between devices, look at DNS and HTTP information, and use basic and advanced filters to find particular packets.

I also learned that bookmarks, filter buttons and profiles can make packet analysis easier and save time when working with large PCAP files.

## Conclusion

I found this room useful because it gave me hands-on practice with packet analysis. I am now more comfortable using Wireshark statistics, display filters and other features to investigate network traffic.

I will continue practising these skills as I work towards becoming a SOC Analyst.

**Platform:** TryHackMe

**Room:** [Wireshark: Packet Operations](https://tryhackme.com/room/wiresharkpacketoperations)

**Status:** Completed
