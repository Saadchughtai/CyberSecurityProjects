# Warzone 1 – IDS/IPS_Alert_Investigation

## About This Room

In this TryHackMe room, I investigated an IDS/IPS alert to decide whether it was a true positive or a false positive.

The room gave me an alert related to possible malware Command and Control (C2) activity. I used different tools to investigate the alert, identify suspicious IP addresses, check threat intelligence information and analyse the network traffic.

This practical exercise helped me understand how a SOC analyst can investigate a network alert and collect evidence to understand what happened.

**Platform:** TryHackMe  
**Room:** Warzone 1  
**Difficulty:** Not specified  
**Status:** Completed  
**Tools Used:** Brim, VirusTotal, Wireshark, NetworkMiner and CyberChef

## Learning Objectives

- Investigate IDS/IPS alerts using Brim.
- Identify suspicious source and destination IP addresses.
- Use VirusTotal to check IP addresses and domains.
- Investigate malware Command and Control activity.
- Analyse HTTP traffic and user-agent strings.
- Identify other IP addresses involved in an attack.
- Use NetworkMiner to find downloaded files.
- Use Wireshark to inspect HTTP streams and downloaded file information.

---

# Task 1 – Investigating the Network Alert

## 1. Reviewing Alerts in Brim

I started the lab by opening the provided `zone1.pcap` file in Brim.

After loading the file, I checked the dashboard and noticed several IDS/IPS alerts. Some alerts were highlighted in red, while others were orange.

To view all the alerts together, I used the following filter:

```text
Event_type == "alert"
```

This helped me narrow down the results and review the alerts generated from the network traffic.

I then looked for the alert related to **Malware Command and Control Activity Detected**.

**Finding:**

| Item | Result |
|---|---|
| Alert signature | ET Malware MirrorBlast CnC Activity M3 |

**Screenshots:**

<img width="1864" height="1681" alt="image" src="https://github.com/user-attachments/assets/526048cd-6dfd-44f1-99c6-3bc55845e411" />


- Filter showing all alert events.
  <img width="1865" height="1681" alt="image" src="https://github.com/user-attachments/assets/570435aa-6bca-47f8-a0d4-fbd87b0c94a4" />

- Alert signature for the Malware Command and Control activity.
  <img width="1861" height="1683" alt="image" src="https://github.com/user-attachments/assets/0924d487-baa8-4d97-a805-630d040f5b55" />
  <img width="1857" height="1679" alt="image" src="https://github.com/user-attachments/assets/f8e8e335-e62c-46b8-987f-3257f2ed0d8e" />


## 2. Identifying the Source IP Address

After finding the correct alert, I checked its details in Brim to identify the source IP address.

The source IP address helped me understand where the suspicious traffic was coming from.

I copied the IP address and used CyberChef to defang it before documenting it.

**Finding:**

| Item | Result |
|---|---|
| Source IP address | [Add defanged source IP] |

**Screenshots:**

- Brim alert details showing the source IP.
  <img width="1863" height="1675" alt="image" src="https://github.com/user-attachments/assets/52f3b284-b245-4b93-bdc1-c4d8aac00296" />

- CyberChef showing the defanged IP address.
  <img width="2931" height="1686" alt="image" src="https://github.com/user-attachments/assets/d69abe47-6b49-4a34-a949-3dbfe1dfd217" />

## 3. Identifying the Destination IP Address

Next, I checked the destination IP address in the same alert.

It was important to identify both the source and destination addresses so I could understand the direction of the network communication.

I also defanged the destination IP before adding it to my investigation notes.

**Finding:**

| Item | Result |
|---|---|
| Destination IP address | [Add defanged destination IP] |

**Screenshot:** Brim alert details showing the destination IP address.
<img width="1859" height="1681" alt="image" src="https://github.com/user-attachments/assets/5be8d9cb-2b32-4412-a357-2a4b9e0f5d52" />
<img width="2927" height="1679" alt="image" src="https://github.com/user-attachments/assets/d019c3b0-e98f-4fa2-bc69-485c74ac9acf" />


## 4. Checking the IP Address Using VirusTotal

After collecting the IP information, I used VirusTotal to investigate the suspicious address and check its threat intelligence information.

I reviewed the Community section and found that the activity was associated with the **MirrorBlast / TA505** threat group.

This gave me more information about the possible malware activity and helped support the investigation.

**Finding:**

| Item | Result |
|---|---|
| Threat group | MirrorBlast / TA505 |

**Screenshot:** VirusTotal Community section showing the threat group attribution.
<img width="2928" height="1688" alt="image" src="https://github.com/user-attachments/assets/5e1bf0b5-e38d-4b01-8ee2-1d8388e128e5" />
<img width="2928" height="1678" alt="image" src="https://github.com/user-attachments/assets/650a2e03-1367-42fa-bd3b-b93fb8c80deb" />


## 5. Checking the Domain and Communicating Files

I then searched for the domain from the previous investigation in VirusTotal.

I opened the Communicating Files section and checked the file types associated with the domain.

The majority file type listed was:

**Finding:** Windows Installer

**Screenshot:** VirusTotal Communicating Files section showing the Windows Installer file type.
<img width="2928" height="1674" alt="image" src="https://github.com/user-attachments/assets/5c43b7d7-ad4d-4a5b-912b-8ae00c0ec08a" />


## 6. Investigating the User-Agent

Next, I inspected the web traffic associated with the flagged IP address.

For this part, I used Wireshark and filtered the traffic using the relevant IP address and HTTP traffic.

I checked the HTTP packet details to find the User-Agent string. This helped me identify the client information included in the web request.

**Finding:**

| Item | Result |
|---|---|
| User-Agent | [Add the User-Agent string] |

**Screenshot:** Wireshark showing the HTTP request and User-Agent field.
<img width="1860" height="1681" alt="image" src="https://github.com/user-attachments/assets/012809e0-fe8c-4204-b759-751837761cc7" />
<img width="1863" height="1680" alt="image" src="https://github.com/user-attachments/assets/da9b8ad4-e405-4c3d-8016-e033104776cc" />


## 7. Identifying Other IP Addresses Involved

I went back to Brim and filtered the traffic by HTTP.

I created a table showing only the source IP addresses, destination IP addresses and User-Agent values.

I used `uniq` to remove duplicate entries so I could review the unique IP addresses more easily.

After checking the results, I identified five IP addresses. One was already known from the earlier investigation. I then investigated the other suspicious addresses using VirusTotal.

The two additional IP addresses were flagged as malicious.

**Findings:**

| Item | Result |
|---|---|
| Additional IP address 1 | [Add defanged IP] |
| Additional IP address 2 | [Add defanged IP] |

**Screenshots:**

- Brim HTTP traffic filter.
- Table showing source IPs, destination IPs and User-Agents.
   <img width="1864" height="1678" alt="image" src="https://github.com/user-attachments/assets/b494cf0f-c154-473a-bb61-b2ea9e026f91" />
- Results after removing duplicates.
  <img width="1860" height="1683" alt="image" src="https://github.com/user-attachments/assets/f3c67be1-8a32-481d-8c1a-27a6df0b4c76" />
  <img width="1857" height="1681" alt="image" src="https://github.com/user-attachments/assets/9bf5807c-b2b4-4f90-bdc2-78244b09bac4" />

- VirusTotal results for the two additional IP addresses.
  <img width="2929" height="1677" alt="image" src="https://github.com/user-attachments/assets/5fbff0d3-56fb-40ad-be20-fc52689ef05d" />
  <img width="2931" height="1677" alt="image" src="https://github.com/user-attachments/assets/6098f0b8-938d-4d19-8e5f-26438d7843cb" />
  <img width="2930" height="1673" alt="image" src="https://github.com/user-attachments/assets/d11eec89-1b1f-47b1-a199-845e5f7b5db9" />
  <img width="2930" height="1673" alt="image" src="https://github.com/user-attachments/assets/d5a035bd-6fff-4164-b62e-c94e59995125" />

## 8. Identifying the Downloaded Files

After identifying the additional IP addresses, I used NetworkMiner to find the files downloaded during the suspicious network activity.

I opened the PCAP in NetworkMiner and went to the Files tab.

I used the IP address in the search or filter options to find files associated with each address.

I identified two downloaded files.

**Findings:**

| IP address order | Downloaded file |
|---|---|
| First IP address | `filter.msi` |
| Second IP address | `10opd3r_load.msi` |

**Screenshots:**

- NetworkMiner Files tab.
  <img width="1861" height="1682" alt="image" src="https://github.com/user-attachments/assets/fff21f8c-4d97-43f6-877f-a31406e51f87" />
- File details showing `filter.msi`.
- File details showing `10opd3r_load.msi`.

  <img width="1866" height="1676" alt="image" src="https://github.com/user-attachments/assets/e3de757e-c619-457e-b557-e38faa302f94" />
  <img width="1859" height="1679" alt="image" src="https://github.com/user-attachments/assets/d766fdbe-3a84-4098-a36c-df936c173d54" />


## 9. Inspecting the First Downloaded File

I used Wireshark to investigate the traffic associated with the first downloaded file, `filter.msi`.

I searched for the relevant malicious IP address and followed the HTTP stream between the compromised host and the server.

I inspected the HTTP communication to find the directory path and the two files saved in that location.

**Finding:**

| Item | Result |
|---|---|
| Directory path and two filenames | [Add the full path and both filenames] |

**Screenshot:** Wireshark Follow HTTP Stream showing the directory path and filenames for the first downloaded file.

## 10. Inspecting the Second Downloaded File

I repeated the same process for the second downloaded file, `10opd3r_load.msi`.

I searched for the relevant IP address in Wireshark, followed the HTTP stream and inspected the traffic to identify the directory path and the two files saved there.

**Finding:**

| Item | Result |
|---|---|
| Directory path and two filenames | [Add the full path and both filenames] |

**Screenshot:** Wireshark Follow HTTP Stream showing the directory path and filenames for the second downloaded file.
  
<img width="2929" height="1679" alt="image" src="https://github.com/user-attachments/assets/212dbaf7-3285-48ed-b538-658f35bd4a1a" />
<img width="2927" height="1676" alt="image" src="https://github.com/user-attachments/assets/25f68eec-d0c9-4ef0-8689-7b56f2d95a50" />
<img width="2926" height="1680" alt="image" src="https://github.com/user-attachments/assets/9f77f74f-262a-4a3a-936e-dca732acbd14" />
<img width="2926" height="1677" alt="image" src="https://github.com/user-attachments/assets/53d8b325-ce5f-4c3e-b932-8119c7819ea3" />
<img width="2931" height="1674" alt="image" src="https://github.com/user-attachments/assets/fa756291-30b1-45b8-ba90-ad6b8d38cbe7" />
<img width="2931" height="1675" alt="image" src="https://github.com/user-attachments/assets/da0a3c44-9b1b-4772-8568-e408afb37a65" />
<img width="2928" height="1679" alt="image" src="https://github.com/user-attachments/assets/9e1ec877-e8c4-4646-8313-f83240c6fc4b" />
<img width="2932" height="1679" alt="image" src="https://github.com/user-attachments/assets/66355735-6a25-4b18-9c89-d64145c464f4" />

# Tools and Skills Practised

During this room, I used the following tools:

| Tool | How I used it |
|---|---|
| Brim | Reviewed IDS/IPS alerts and investigated network traffic. |
| VirusTotal | Checked IP addresses and domains using threat intelligence information. |
| Wireshark | Inspected HTTP traffic, User-Agent strings and HTTP streams. |
| NetworkMiner | Identified downloaded files and investigated their network details. |
| CyberChef | Defanged IP addresses before documenting them. |

## Skills Developed

- IDS/IPS alert investigation and triage.
- Network traffic analysis.
- Identifying suspicious IP addresses and domains.
- Threat intelligence investigation.
- Recognising possible Command and Control activity.
- HTTP and User-Agent analysis.
- Extracting files from PCAP traffic.
- Following HTTP streams in Wireshark.
- Using multiple tools to investigate a security alert.

# Conclusion

Completing Warzone 1 gave me practical experience investigating a network alert from start to finish.

I used Brim to identify the alert and investigate the related IP addresses. I then used VirusTotal to check the threat intelligence information, Wireshark to inspect the HTTP traffic, and NetworkMiner to identify downloaded files.

This room helped me understand how different tools can be used together during a SOC investigation. It also improved my confidence in analysing network traffic, checking suspicious activity and collecting evidence before making a decision about an alert.

**TryHackMe Room:** [Warzone 1](https://tryhackme.com/room/warzone1)

**Status: Completed**
