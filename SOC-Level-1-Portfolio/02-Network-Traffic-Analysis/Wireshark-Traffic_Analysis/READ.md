# Wireshark: Traffic Analysis

## About This Room

In this room, I used Wireshark to investigate different types of network traffic and learn how to identify suspicious activity.

I worked with several PCAP files and explored network activities such as Nmap scans, ARP spoofing, host identification, FTP traffic, suspicious HTTP requests and encrypted HTTPS traffic.

I also practised using Wireshark filters to find specific packets, investigate unusual behaviour and create firewall rules.

This room gave me more hands-on experience with network traffic analysis and helped me understand how Wireshark can be used during security investigations.

## Tools Used

- Wireshark
- TryHackMe AttackBox
- Nmap
- CyberChef
- PCAP files
- TLS key log file

## What I Learned

- Identifying TCP and UDP scans.
- Investigating ARP spoofing and Man-in-the-Middle attacks.
- Identifying network devices and users using DHCP, NetBIOS and Kerberos.
- Analysing FTP traffic and login activity.
- Finding unusual HTTP User-Agent values.
- Investigating a Log4j attack attempt.
- Decrypting HTTPS traffic using a TLS key log file.
- Inspecting HTTP/2 packets.
- Generating firewall rules using Wireshark.

---

## My Practical Work

### 1. Nmap Scan Analysis

I started by investigating different types of Nmap scans in Wireshark. I used display filters to find scanning activity and understand how devices respond when ports are open or closed.

One of the filters I used was:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0 && tcp.window_size > 1024
```

I spent some time understanding what each part of the filter does:

- `tcp.flags.syn == 1` finds packets with the SYN flag set.
- `tcp.flags.ack == 0` finds packets without the ACK flag set.
- `tcp.window_size > 1024` finds packets with a TCP window size greater than 1024.

I also looked up TCP window size to understand how it controls the amount of data that can be sent before an acknowledgement is needed.

**My findings**

| Investigation | Filter used | Result |
|---|---|---:|
| Total TCP Connect scans | `tcp.flags.syn == 1 && tcp.flags.ack == 0 && tcp.window_size > 1024` | 1,000 |
| UDP closed-port messages | `icmp.type == 3 && icmp.code == 3` | 1,083 |
| Open UDP port in the 55–70 range | `udp.port in {55 .. 70}` | 68 |

For the UDP closed-port question, I first filtered for UDP traffic and noticed ICMP Destination Unreachable messages.

I opened one of the packets and checked its ICMP details. I learned that type 3 means Destination Unreachable and code 3 means Port Unreachable.

I found that ports 67 and 69 were unreachable, while port 68 was open.

**Screenshots**

Add your screenshots here showing the Nmap scan filters, packet details and results.


<img width="965" height="580" alt="image" src="https://github.com/user-attachments/assets/6f7c5633-e22e-42fa-91e9-44996e3f9b83" />

<img width="955" height="320" alt="image" src="https://github.com/user-attachments/assets/0e8b8c07-7f3f-4d08-b4f3-15afcac707a8" />

<img width="1800" height="1676" alt="image" src="https://github.com/user-attachments/assets/c8d5ad54-e182-45ea-8da1-6a98e99e609a" />



### 2. ARP Spoofing and Man-in-the-Middle Investigation

Next, I learned about ARP poisoning, also called ARP spoofing.

ARP helps devices on a local network find the MAC address connected to an IP address. I learned that because ARP does not have built-in authentication, an attacker can send fake ARP messages to make devices associate an IP address with the wrong MAC address.

This can allow an attacker to intercept network traffic through a Man-in-the-Middle (MITM) attack.

**Filters I practised**

```text
arp
arp.opcode == 1
arp.opcode == 2
arp.dst.hw_mac == 00:00:00:00:00:00
arp.duplicate-address-detected or arp.duplicate-address-frame
```

I learned that:

- `arp.opcode == 1` shows ARP requests.
- `arp.opcode == 2` shows ARP replies.
- Duplicate address warnings can help identify possible ARP spoofing.

**What I found**

While investigating the capture, I noticed that two different MAC addresses were claiming the same IP address, `192.168.1.1`, which appeared to be the gateway.

I also found one MAC address sending multiple ARP requests to different IP addresses.

| Item | My finding |
|---|---|
| Suspected attacker's MAC address | `00:0c:29:e2:18:b4` |
| Suspected attacker's IP address | `192.168.1.25` |
| Possible gateway IP address | `192.168.1.1` |
| Gateway MAC address | `50:78:b3:f3:cd:f4` |
| Victim IP address | `192.168.1.12` |
| Victim MAC address | `00:0c:29:98:c7:a8` |

I added the MAC addresses as columns in Wireshark to make it easier to follow the traffic.

I noticed that the suspected attacker's MAC address appeared as the destination of the HTTP packets. This was an important clue that traffic was being redirected through the attacker's device.

**My results**

| Investigation | Result |
|---|---:|
| Number of ARP requests from the suspected attacker | 284 |
| Number of HTTP packets received by the attacker | 90 |

To find the ARP request count, I started with:

```text
arp.dst.hw_mac == 00:00:00:00:00:00
```

After identifying the suspected attacker's MAC address, I added:

```text
&& arp.opcode == 1
```

For the HTTP packets, I applied a filter using the attacker's MAC address and added `&& http`.

**Screenshots**

Add screenshots showing the ARP requests, MAC addresses, suspicious traffic and HTTP packet count.


(<img width="1800" height="1671" alt="image" src="https://github.com/user-attachments/assets/35b63048-6eea-4ca3-bf35-8bebfe4d88c6" />
)
<img width="1803" height="1679" alt="image" src="https://github.com/user-attachments/assets/8cb25c51-df53-4732-9139-3b3be9c938bb" />
<img width="1802" height="1675" alt="image" src="https://github.com/user-attachments/assets/3761c8e4-4137-47a3-a1e3-05176bc322b5" />
<img width="1801" height="1677" alt="image" src="https://github.com/user-attachments/assets/4c76ed12-3354-4c21-86eb-2ee568933740" />
<img width="1797" height="1678" alt="image" src="https://github.com/user-attachments/assets/19374000-1929-44ac-84fe-3b96820f8892" />
<img width="1800" height="1672" alt="image" src="https://github.com/user-attachments/assets/a49aab34-3eee-4528-b64e-ab1a591e3855" />
<img width="1804" height="1679" alt="image" src="https://github.com/user-attachments/assets/9f4f564b-d8c6-4952-ab66-a746b02d3e30" />

<img width="1805" height="1678" alt="image" src="https://github.com/user-attachments/assets/d2bfdbc3-eab9-4243-9194-4a0f96d5de1d" />
<img width="1800" height="1680" alt="image" src="https://github.com/user-attachments/assets/3fc59e41-dcf1-4584-98ba-7cf2569b6921" />

<img width="1801" height="1677" alt="image" src="https://github.com/user-attachments/assets/801f8ea9-fcc1-4a50-a8af-eb47e2b7fbb4" />
<img width="1798" height="1680" alt="image" src="https://github.com/user-attachments/assets/6bd0c305-f0f2-4f02-9623-2b60af6f81de" />



### 3. Identifying Hosts and Users

In this task, I learned how DHCP, NetBIOS (NBNS) and Kerberos traffic can help identify devices and users on a network.

I found this useful because knowing which device or user is involved can help narrow down where an investigation should start.

**DHCP analysis**

DHCP automatically assigns IP addresses and other network settings to devices.

I practised these filters:

```text
dhcp
bootp
dhcp.option.dhcp == 3
dhcp.option.dhcp == 5
dhcp.option.dhcp == 6
dhcp.option.hostname contains "keyword"
dhcp.option.domain_name contains "keyword"
```

I learned that:

- DHCP Request packets can contain hostname information.
- DHCP ACK packets show accepted requests.
- DHCP NAK packets show denied requests.
- Option 12 contains the hostname.
- Option 50 contains the requested IP address.
- Option 61 contains the client's MAC address.

**NetBIOS analysis**

I also explored NetBIOS Name Service traffic, which can help identify hostnames on a network.

```text
nbns
nbns.name contains "keyword"
```

I learned that NBNS query information can contain details such as the hostname and IP address.

**Kerberos analysis**

Kerberos is used for authentication in Microsoft Windows domain environments.

I practised these filters:

```text
kerberos
kerberos.CNameString contains "keyword"
kerberos.CNameString and !(kerberos.CNameString contains "$")
kerberos.pvno == 5
kerberos.realm contains ".org"
kerberos.SNameString == "krbtg"
```

I learned that the `CNameString` field can contain a username or hostname. Names ending in `$` can represent computer accounts rather than normal user accounts.

**My investigation results**

| Investigation | Result |
|---|---|
| MAC address of Galaxy A30 | Correct answer submitted |
| NetBIOS registration requests from LIVALJM | Correct answer submitted |
| Host that requested `172.16.13.85` | Correct answer submitted |
| IP address of user `u5` | Correct answer submitted |
| Hostname of the available host in Kerberos packets | Correct answer submitted |

**Screenshots**

Add screenshots showing your DHCP, NBNS and Kerberos filters and the packet details used to identify the hosts and users.



<img width="1799" height="1679" alt="image" src="https://github.com/user-attachments/assets/83138e08-e92b-4dfb-a9ef-ba165d783407" />
<img width="1807" height="1675" alt="image" src="https://github.com/user-attachments/assets/13731aa0-82e8-40a1-a235-219b4f786383" />
<img width="1795" height="1681" alt="image" src="https://github.com/user-attachments/assets/5d0f2a23-ac14-498c-aee7-68ed44818275" />
<img width="1804" height="1677" alt="image" src="https://github.com/user-attachments/assets/86e82298-c091-40c6-b575-2548f9ca303c" />
<img width="2924" height="1685" alt="image" src="https://github.com/user-attachments/assets/eb3ecd1b-869d-4ff9-9f31-78fe640cb637" />
<img width="1809" height="1691" alt="image" src="https://github.com/user-attachments/assets/ac8fd849-4820-4dc8-a17e-a82f38a88ab1" />



### 4. Cleartext Protocol Analysis

This task helped me understand why using unencrypted protocols can create security risks.

When information is sent without encryption, it may be possible to read it directly from captured network traffic.

#### FTP analysis

FTP is used to transfer files between devices. However, it does not encrypt information by default, which means usernames, passwords and file activity may be visible in a packet capture.

I practised these filters:

```text
ftp
ftp.response.code == 211
ftp.response.code == 227
ftp.response.code == 230
ftp.request.command == "USER"
ftp.request.command == "PASS"
ftp.response.code == 530
```

I also learned about some FTP response codes and commands.

| Code or command | Meaning |
|---|---|
| 211 | System status |
| 220 | Service ready |
| 227 | Entering passive mode |
| 230 | User login successful |
| 331 | Username accepted |
| 430 | Invalid username or password |
| 530 | Login failed |
| USER | Username command |
| PASS | Password command |
| CWD | Change working directory |
| LIST | List directory contents |

I used the FTP capture to investigate login attempts, file access and file upload activity.

**My FTP investigation**

| Question | Result |
|---|---|
| Number of incorrect login attempts | Correct answer submitted |
| Size of the file accessed by the FTP account | Correct answer submitted |
| Filename of the document uploaded by the adversary | Correct answer submitted |
| Command used to change the uploaded file's permissions | Correct answer submitted |

#### HTTP User-Agent analysis

I then investigated HTTP User-Agent values.

I used:

```text
http.user_agent
```

I added the User-Agent field as a column in Wireshark and checked the different values to identify unusual tools and browser information.

I found six User-Agent types that the room identified as anomalous:

1. Chrome 8.0.552.237 on Windows.
2. Nmap Scripting Engine.
3. Wfuzz 2.4.
4. sqlmap 1.4.
5. A User-Agent containing a Log4j JNDI LDAP payload.
6. Firefox 100.0 on Ubuntu.

**Total anomalous User-Agent types: 6**

#### Investigating the Log4j activity

I used the following filter to find the suspicious User-Agent:

```text
http.user_agent contains "$" or http.user_agent contains "=="
```

This helped me locate packet number **444**.

I inspected the packet and decoded the Base64 command. The decoded command showed an attempt to:

- Download a script called `lh.sh` using `wget`.
- Change the script's permissions using `chmod +x`.
- Execute the downloaded script.

This helped me understand how a suspicious User-Agent can contain information related to an attack attempt.

| Investigation | Result |
|---|---|
| Number of anomalous User-Agent types | 6 |
| Packet number showing the Log4j attack starting phase | 444 |
| IP address contacted by the adversary | Correct answer submitted |

**Screenshots**

Add screenshots showing the FTP investigation, User-Agent column, suspicious Log4j packet and decoded command.


<img width="1801" height="1676" alt="image" src="https://github.com/user-attachments/assets/e041fdda-46d2-4965-b4ac-cca00a8a928a" />
<img width="1795" height="1680" alt="image" src="https://github.com/user-attachments/assets/7a49f21a-496c-48f6-97dd-2ca4f47ce296" />
<img width="1802" height="1678" alt="image" src="https://github.com/user-attachments/assets/86cb365a-a235-4155-a512-e7b8c2c4b7e6" />
<img width="1805" height="1676" alt="image" src="https://github.com/user-attachments/assets/3f9370c3-4fa8-4add-a1a8-c25347d514b5" />
<img width="1806" height="1677" alt="image" src="https://github.com/user-attachments/assets/fbf5b73d-60c3-40f9-9052-b0e68522212b" />
<img width="2920" height="322" alt="image" src="https://github.com/user-attachments/assets/85bda33f-94db-4075-b619-e80ac9895641" />
<img width="2935" height="1836" alt="image" src="https://github.com/user-attachments/assets/7cf2a02c-563e-49b1-b054-bf4c2a16ad6c" />







![HTTP User-Agent values](YOUR_IMAGE_LINK)
![Log4j packet](YOUR_IMAGE_LINK)
```

### 5. Decrypting HTTPS Traffic

In this task, I learned how HTTPS uses TLS encryption to protect web traffic.

At first, the captured traffic did not show all the website and data details because the communication was encrypted.

The room provided a key log file that I could use to decrypt the traffic in Wireshark.

**Filters I practised**

```text
http.request
tls
tls.handshake.type == 1
tls.handshake.type == 2
ssdp
```

I learned that:

- Client Hello uses `tls.handshake.type == 1`.
- Server Hello uses `tls.handshake.type == 2`.

**Steps I followed to decrypt the traffic**

1. I opened **Edit → Preferences** in Wireshark.
2. I selected **Protocols → TLS**.
3. I browsed to `KeysLogFile.txt`.
4. I selected the key log file and applied the settings.
5. I checked the packets again to view the decrypted traffic.

After adding the key log file, I could inspect the HTTP/2 information and other details that were not visible in the encrypted capture.

**My findings**

| Investigation | Result |
|---|---|
| Client Hello frame sent to `accounts.google.com` | 16 |
| Number of HTTP/2 packets after decryption | 115 |
| Authority header in frame 322 | Correct answer submitted |
| Flag found in the decrypted traffic | `FLAG{THM-PACKETMASTER}` |

For frame 322, I opened the HTTP/2 packet details and checked the authority header. I then used CyberChef to defang the address.

To find the flag, I used the Wireshark search option and searched for `flag{`. This helped me locate the relevant packet.

**Screenshots**

Add screenshots showing the Client Hello packet, TLS key log configuration, decrypted HTTP/2 packets, frame 322 and the flag.


<img width="1806" height="1680" alt="image" src="https://github.com/user-attachments/assets/3dcd24bc-ea00-4d7d-b309-a84f92de30e0" />
<img width="1803" height="1677" alt="image" src="https://github.com/user-attachments/assets/af63048c-67bc-4092-a1a4-fb14847e54b5" />
<img width="1802" height="1678" alt="image" src="https://github.com/user-attachments/assets/d42dbbf3-2dba-4494-a276-82323237b7e3" />
<img width="1807" height="1673" alt="image" src="https://github.com/user-attachments/assets/45083361-8e08-4a55-9c86-28cab69476f2" />
<img width="1802" height="1677" alt="image" src="https://github.com/user-attachments/assets/fed3ccb3-dd3b-4997-844b-3296559fde76" />
<img width="1803" height="1677" alt="image" src="https://github.com/user-attachments/assets/b7b78166-3061-4806-9873-0f272cd3e53d" />
<img width="1806" height="1678" alt="image" src="https://github.com/user-attachments/assets/e58285c9-f56f-4642-a157-69561bb4eb82" />
<img width="1803" height="1678" alt="image" src="https://github.com/user-attachments/assets/85676452-c511-49ed-bf6b-bb03eb5d7b2d" />


### 6. Bonus: Creating Firewall Rules

In the final task, I learned that Wireshark can generate firewall ACL rules using information from selected packets.

I opened **Tools → Firewall ACL Rules** and selected the IPFirewall (`ipfw`) format.

Wireshark also supports other firewall formats, including:

- Netfilter (iptables)
- Cisco IOS
- IP Filter
- Packet Filter (pf)
- Windows Firewall

**My firewall rule findings**

| Task | Generated rule |
|---|---|
| Deny source IPv4 address from packet 99 | `deny ip from 10.121.70.151 to any in` |
| Allow destination MAC address from packet 231 | `allow MAC 00:d0:59:aa:af:80 any in` |

For the first question, I selected packet 99 and generated an IPFirewall rule to deny the source IPv4 address.

For the second question, I selected packet 231 and generated the rule to allow the destination MAC address.

I found this task interesting because it showed me how information collected during packet analysis can be used to prepare firewall rules.

**Screenshots**

Add screenshots showing packet 99, packet 231 and the generated IPFirewall rules.

<img width="1801" height="1678" alt="image" src="https://github.com/user-attachments/assets/cd10f511-3388-4044-ad17-4065316e5991" />
<img width="1814" height="1677" alt="image" src="https://github.com/user-attachments/assets/64e8deb6-a6b4-4273-bd66-da7da4c1e4b6" />
<img width="1805" height="1681" alt="image" src="https://github.com/user-attachments/assets/9bc094e5-dc16-4ac2-84a6-a1e117601db4" />
<img width="1809" height="1680" alt="image" src="https://github.com/user-attachments/assets/281b4069-3372-48c8-af14-22f88640762c" />
<img width="1806" height="1681" alt="image" src="https://github.com/user-attachments/assets/c829f776-54aa-49bd-b4d2-c0e638270833" />




## What I Learned from This Room

This room gave me good hands-on practice with Wireshark and helped me understand how different types of network activity can be investigated.

I learned how to identify scanning activity, follow suspicious ARP traffic, find information about devices and users, and inspect unencrypted network communication.

I also practised identifying unusual User-Agent values, investigating a Log4j attack attempt and decrypting HTTPS traffic using a key log file.

The ARP spoofing task was one of the most interesting parts for me because I could follow the suspicious MAC address and see how it was connected to the HTTP traffic.

I also found the HTTPS task useful because it helped me understand how Wireshark can be used to inspect encrypted traffic when the correct session keys are available.

## Conclusion

I found this room useful because it gave me practical experience with different areas of network traffic analysis.

I am now more comfortable using Wireshark filters, checking packet details and investigating unusual network activity.

These are skills I want to keep improving as I work towards a SOC Analyst role. I plan to continue practising with other network analysis tools such as NetworkMiner, Snort and Zeek.

**Platform:** TryHackMe

**Room:** [Wireshark: Traffic Analysis](https://tryhackme.com/room/wiresharktrafficanalysis)

**Status:** Completed
