# NetworkMiner – TryHackMe

## About This Room

In this room, I learned how to use NetworkMiner to investigate network traffic and analyse PCAP files.

NetworkMiner is an open-source Network Forensic Analysis Tool (NFAT). It can help identify hosts, sessions, operating systems, credentials, files and other information from captured network traffic.

I used NetworkMiner to investigate different PCAP files and extract useful information from them.

**TryHackMe Room:** [NetworkMiner](https://tryhackme.com/room/networkminer)

**Status:** Completed

## Tools Used

- NetworkMiner
- TryHackMe AttackBox / Ubuntu lab environment
- PCAP files
- Windows environment

## What I Learned

During this room, I learned how to:

- Open and investigate PCAP files using NetworkMiner.
- Identify hosts and their MAC and IP addresses.
- Check the number of frames and packets.
- Identify web server banners and operating systems.
- Extract usernames and passwords from captured traffic.
- Investigate files, images and network parameters.
- Search for keywords and messages.
- Identify possible network anomalies.
- Examine network sessions and transferred bytes.

---

## Task 1 – Introduction to NetworkMiner

### 1.1 Investigating mx-3.pcap

I started by navigating to the Desktop and opening the Exercise Files folder. I selected `mx-3.pcap` and loaded it into NetworkMiner.

After the PCAP file was loaded successfully, I used the Frames section to check the total number of frames.

**Findings:**

| Question | Answer |
|---|---|
| Total number of frames | 460 |
| Number of IP addresses using the same MAC address as host 145.253.2.203 | 2 |
| Packets sent by 65.208.228.223 | 72 |
| Web server banner for 65.208.228.223 | Apache |

To find the IP addresses using the same MAC address, I checked the Hosts tab and compared the MAC addresses.

I also selected host `65.208.228.223` to view its details. From the host information, I found the packet count and Apache web server banner.

**Screenshots:**

- PCAP file loaded in NetworkMiner.
  <img width="1801" height="1676" alt="image" src="https://github.com/user-attachments/assets/5cf34401-f0ce-4da1-9bef-090e1b11000a" />
  <img width="1802" height="1677" alt="image" src="https://github.com/user-attachments/assets/f1bf4776-099d-4cf9-8fc8-1381c7cfa1c6" />
  <img width="1801" height="1676" alt="image" src="https://github.com/user-attachments/assets/1362f1f5-3fb7-45e2-87e1-97fc5f88119d" />
  <img width="1804" height="1681" alt="image" src="https://github.com/user-attachments/assets/fffb9496-bb68-4b57-989a-1f3766be72d2" />
- Total frames displayed.
  <img width="1798" height="1678" alt="image" src="https://github.com/user-attachments/assets/ac4f2102-5a58-402e-a532-d448ea211ffe" />
- Hosts sharing the same MAC address.
  <img width="1802" height="1678" alt="image" src="https://github.com/user-attachments/assets/1433d91d-a24e-4038-b263-183475912c16" />
  <img width="1801" height="1679" alt="image" src="https://github.com/user-attachments/assets/84c3d182-5880-4948-9207-316de3a256d4" />
- Host packet information.
  <img width="1801" height="1679" alt="image" src="https://github.com/user-attachments/assets/dd26e461-21ba-4dfd-baaa-dc5691280819" />
  <img width="1800" height="1676" alt="image" src="https://github.com/user-attachments/assets/5be0dd68-4f39-4570-b853-9260f3824f1b" />

- Apache web server banner.
-  <img width="1800" height="1676" alt="image" src="https://github.com/user-attachments/assets/5be0dd68-4f39-4570-b853-9260f3824f1b" />
   <img width="589" height="369" alt="image" src="https://github.com/user-attachments/assets/044d5f08-f5b5-45d2-8d01-f62b94f9b599" />


### 1.2 Investigating mx-4.pcap

Next, I opened `mx-4.pcap` in NetworkMiner.

I used the Credentials tab to investigate the extracted login information for the host `02694W-WIN10`.

**Findings:**

| Question | Answer |
|---|---|
| Extracted username for 02694W-WIN10 | Not recorded in my notes |
| Extracted password | Not recorded in my notes |

I found the username and password in the Credentials tab.

**Screenshots:**

- mx-4.pcap loaded in NetworkMiner.
  <img width="1801" height="1676" alt="image" src="https://github.com/user-attachments/assets/76408418-959b-472d-97f3-42129de334e7" />

- Credentials tab showing the extracted information.
  <img width="1798" height="1680" alt="image" src="https://github.com/user-attachments/assets/62d0cc5c-8263-4034-83cd-7d51268a8adf" />
  <img width="1805" height="1677" alt="image" src="https://github.com/user-attachments/assets/45a37efb-c98c-4dad-8550-559f50116597" />


### 1.3 Investigating mx-7.pcap

For this investigation, I used the search and filtering features in NetworkMiner.

I searched for the specified frame numbers and checked the associated file details.

For some questions, the room provided an alternative frame number to check if no result was found for the first one.

**Findings:**

| Question | Answer |
|---|---|
| Linux distribution mentioned in the file associated with frame 63602 (alternative frame 63075) | CentOS |
| Name and surname mentioned in the file associated with frame 76469 (alternative frame 75942) | Ned Flanders |
| Frame number of the possible TLS anomaly | Not recorded in my notes |

For the Linux distribution, I could not find an associated file using frame 63602, so I checked frame 63075 instead. I opened the file details and found CentOS.

I followed the same approach for the name and surname question. I checked the alternative frame and found Ned Flanders in the associated file details.

I also visited the Anomalies tab to investigate the possible TLS anomaly.

**Screenshots:**

- Frame search for 63602 and 63075.
  <img width="1805" height="1676" alt="image" src="https://github.com/user-attachments/assets/e52aad68-221b-4899-a2e6-e58c69817f6b" />

- File details showing CentOS.
 <img width="1806" height="1678" alt="image" src="https://github.com/user-attachments/assets/72914078-429d-4289-9659-8240a3c9083b" />

- File details showing Ned Flanders.
  <img width="1798" height="1675" alt="image" src="https://github.com/user-attachments/assets/d50378df-811b-4ce2-867d-91ef722325be" />

- Anomalies tab showing the TLS anomaly.
  <img width="1802" height="1682" alt="image" src="https://github.com/user-attachments/assets/6a974b01-4cb4-447e-9414-2e5ae8793a79" />
  <img width="1805" height="1677" alt="image" src="https://github.com/user-attachments/assets/d6baf47f-0369-4d88-8983-19d694d36464" />


### 1.4 Investigating mx-9

I loaded the `mx-9` PCAP file and used the Keywords section to search for the email subject and the person's name.

**Findings:**

| Question | Answer |
|---|---|
| Platform that sent the email with a subject starting with "You have more..." | Facebook |
| Email address of Branson Matheson | branson@sandsite.org |

I searched for the phrase "You have more..." using the keyword filter and found the related email information.

I then searched for Branson Matheson and checked the extracted details under the attributes and values.

**Screenshots:**

- Keyword search for the email subject.
  <img width="1806" height="1679" alt="image" src="https://github.com/user-attachments/assets/5b2ccc9e-c97c-46dd-b077-08756771080d" />

- Extracted email information showing Facebook.
  <img width="1799" height="1685" alt="image" src="https://github.com/user-attachments/assets/8161be2c-0cd6-4992-b92a-ddaa40fac5bb" />

- Search results for Branson Matheson.
- <img width="1801" height="1673" alt="image" src="https://github.com/user-attachments/assets/a7d51bcb-6765-457f-af3f-e0fce41b7536" />

- Details showing the email address.
  <img width="1798" height="1680" alt="image" src="https://github.com/user-attachments/assets/1dd34d28-54c6-462e-92fa-e0ee4fd7e3fd" />

---




## Task 3 – NetworkMiner Exercises

For the practical exercises, I investigated additional PCAP files and used the Hosts, Sessions, Frames, Parameters and Files tabs to find the required information.

### 3.1 Investigating case1.pcap

I opened `case1.pcap` from the Exercise Files folder.

#### Identifying the operating system

I selected host `131.151.37.122` from the Hosts tab and expanded its details.

I scrolled down to the operating system information to identify the full OS name.

**Finding:**

| Question | Answer |
|---|---|
| Full OS name of host 131.151.37.122 | Windows - Windows NT 4 |

#### Investigating communication through port 1065

I investigated the communication between:

- Client: `131.151.32.91`
- Server: `131.151.37.122`
- Port: `1065`

I selected the host and checked its incoming sessions. I found two sessions and examined the session using port 1065.

**Finding:**

| Question | Answer |
|---|---|
| Bytes sent by the client through port 1065 | 192 bytes |

#### Investigating communication through port 143

I investigated communication between:

- Client: `131.151.32.21`
- Server: `131.151.37.122`
- Port: `143`

I checked the outgoing session details and found the number of bytes sent back by the server.

**Finding:**

| Question | Answer |
|---|---|
| Bytes sent back by server 131.151.37.122 through port 143 | 20,769 bytes |

#### Checking the TCP sequence number

For this question, I used the older version of NetworkMiner because it provided the frame details I needed.

I opened the Frames tab, selected frame number 9 and expanded the TCP information to find the sequence number.

**Finding:**

| Question | Answer |
|---|---|
| Sequence number of frame 9 | `2AD77400` |

#### Identifying content types

I opened the Parameters tab and filtered the results using the content type information.

**Finding:**

| Question | Answer |
|---|---|
| Number of detected content types | 2 |

#### Identifying the USB product brand

I went to the Files tab and searched for USB-related files.

I then opened the extracted USB stream and inspected the USB history text file to find the product brand.

**Finding:**

| Question | Answer |
|---|---|
| USB product brand name | Not recorded in my notes |

#### Identifying the phone model

I first checked the Images tab and searched through the extracted images to identify the phone image.

After finding the image, I noted its filename. I then went to the Files tab and searched for "lumia".

I found two related files. I checked the file details of the relevant phone model file to identify the device.

**Finding:**

| Question | Answer |
|---|---|
| Phone model | Not recorded in my notes |

**Screenshots:**

- Host details showing Windows NT 4.
  <img width="1798" height="1682" alt="image" src="https://github.com/user-attachments/assets/fdc711eb-0bb2-4ee6-9816-c0bcfa6f071a" />
  <img width="1799" height="1679" alt="image" src="https://github.com/user-attachments/assets/9ba66253-9790-437d-953b-5f53287fe6fc" />

- Session details for port 1065.
  <img width="1803" height="1680" alt="image" src="https://github.com/user-attachments/assets/9a52ebe9-9598-4388-9567-39936c97417c" />

- Session details for port 143.
  <img width="1800" height="1679" alt="image" src="https://github.com/user-attachments/assets/a19c089a-7a9b-4ab1-90c6-07d4c41698c1" />
- Frame 9 TCP sequence number.
  <img width="1807" height="1680" alt="image" src="https://github.com/user-attachments/assets/43a5d1ff-a51e-472b-be1a-a6161d616e95" />
  <img width="1803" height="1679" alt="image" src="https://github.com/user-attachments/assets/b7cb1fec-98eb-4005-b5b0-bc3a67c4a476" />

- Parameters tab showing two content types.
  <img width="1805" height="1677" alt="image" src="https://github.com/user-attachments/assets/60c32ad6-3b24-48fa-9552-c43c61507f15" />

- USB stream and history text file.
  <img width="1808" height="1683" alt="image" src="https://github.com/user-attachments/assets/9dbd987a-e747-4219-a26f-662f63155aff" />
  <img width="1801" height="1682" alt="image" src="https://github.com/user-attachments/assets/8ef0c0a3-3dab-46c3-b693-e872c942f2af" />
  <img width="1805" height="1680" alt="image" src="https://github.com/user-attachments/assets/53638441-ce50-499e-a6cd-0a43f5399dc5" />
  <img width="1803" height="1678" alt="image" src="https://github.com/user-attachments/assets/5b837bd0-cc70-417c-ad55-7917b6c16495" />
  <img width="1803" height="1685" alt="image" src="https://github.com/user-attachments/assets/2ed2f844-8585-4eec-99e0-0eac6fac40f0" />
  <img width="1806" height="1681" alt="image" src="https://github.com/user-attachments/assets/e03bc30b-75f8-4dea-83b6-6a84d3f1ddbd" />

- Image showing the phone.
  <img width="1809" height="1679" alt="image" src="https://github.com/user-attachments/assets/15698f75-89da-481f-a0a3-8bd5abf2c7c3" />
  <img width="1803" height="1675" alt="image" src="https://github.com/user-attachments/assets/1947f75f-60a9-4af6-9109-5fd52fab6bfe" />
  <img width="1802" height="1679" alt="image" src="https://github.com/user-attachments/assets/62a0fd25-3f6b-4240-8c72-f7ac275053fd" />



---

## Key Skills Developed

By completing this room, I gained practical experience in:

- PCAP investigation using NetworkMiner.
- Host and MAC address analysis.
- Identifying operating systems and web server banners.
- Extracting network credentials.
- Investigating files and images from network traffic.
- Searching extracted keywords and messages.
- Reviewing network sessions and transferred bytes.
- Examining TCP frame information.
- Identifying possible network anomalies.

## Conclusion

This room helped me understand how NetworkMiner can be used for network forensics and PCAP investigation.

I learned how to move between different tabs and extract useful information from captured network traffic. I also practised investigating hosts, sessions, files, credentials and messages.

This practical experience has helped me improve my network investigation skills and understand how NetworkMiner can support a SOC analyst when investigating suspicious network activity.

## TryHackMe Room

[NetworkMiner – TryHackMe](https://tryhackme.com/room/networkminer)

**Room Status:** Completed
