# The Greenholt Phish – Email Investigation

## 1. Overview

In this TryHackMe practical, I took on the role of a Level 1 SOC Analyst investigating a suspicious email reported by a sales executive at Greenholt PLC.

The email was supposedly sent by a known customer, but the employee noticed that something was not right. It contained a generic greeting, an unexpected money transfer notification and an attachment that the employee was not expecting.

My task was to investigate the email, check its headers, examine the sender's authentication records and analyse the attachment to find out whether it was legitimate or malicious.

## 2. Initial Email Investigation

I started by opening the email in Thunderbird and checking the sender details, subject and message content.

### 2.1 Email Details

| Item | Finding |
|---|---|
| Sender Display Name | Mr. James Jackson |
| Sender Email | `info@mutawamarine.com` |
| Reply-To Address | `info.mutawamarine@mail.com` |
| Recipient | `webmaster@redacted.org` |
| Transfer Reference Number | `09674321` |
| Transaction Amount | 149,650 |
| Attachment | `SWT_#09674321____PDF__.CAB` |

### 2.2 Checking the Sender and Email Content

The display name was Mr. James Jackson, and the sender address was `info@mutawamarine.com`.

I also noticed that the Reply-To address was different:

`info.mutawamarine@mail.com`

This caught my attention because the two addresses used different domains.

The email also started with a generic greeting instead of addressing the recipient personally. It claimed that a payment had been transferred through SWIFT and included a payment receipt as an attachment.

The employee had not expected this message, so I treated the payment request and attachment as suspicious.

### 2.3 Checking the Subject

The subject line included the transfer reference number:

`09674321`

It also included the recipient's email address. I recorded the reference number as part of the investigation.

## 3. Email Header Analysis

### 3.1 Inspecting the Email Source

To get more information about the email, I opened it in Thunderbird and selected:

`View → Message Source`

I checked the original headers to find the sender details, Return-Path, Reply-To address, originating IP information and email authentication results.

### 3.2 Header Findings

| Header Information | Finding |
|---|---|
| Return-Path | `info@mutawamarine.com` |
| Sender Address | `info@mutawamarine.com` |
| Reply-To | `info.mutawamarine@mail.com` |
| Receiving Mail Server | `atlas125.free.mail.bfl.yahoo.com` |
| X-Atlas-Received IP | `10.201.192.162` |
| SPF Result | Fail |
| DMARC Result | Unknown |

One thing I noticed was that the Reply-To address used a different domain from the sender address.

The original headers also showed an SPF failure and an unknown DMARC result. These findings made me question the authenticity of the email and encouraged me to investigate the domain further.

### 3.3 Originating IP and WHOIS Lookup

I continued examining the message source to identify the originating IP address.

I then used a WHOIS lookup to check the organisation associated with the IP address.

**WHOIS result:**
- Organisation: HostPapa

This gave me additional information about the infrastructure associated with the email. However, the hosting provider's name alone does not prove that the provider or its services were involved in malicious activity.

## 4. SPF and DMARC Investigation

I wanted to understand the email authentication settings of the domain found in the Return-Path field.

I used [Dmarcian](https://dmarcian.com/) to check the SPF and DMARC records for:

`mutawamarine.com`

### 4.1 SPF Record Check

I used Dmarcian to look up the SPF record published for the domain.

**Full SPF record:**

`v=spf1 include:spf.protection.outlook.com -all`

The SPF record identifies which mail servers are authorised to send emails on behalf of a domain.

The original email headers showed an SPF failure. This was an important finding because the message did not pass the SPF authentication check.

### 4.2 DMARC Record Check

I also checked the DMARC record for the same domain.

**Full DMARC record:**

`v=DMARC1; p=quarantine; fo=1`

My understanding of this record:

- `v=DMARC1` identifies the DMARC version.
- `p=quarantine` tells receiving mail servers to treat emails that fail DMARC as suspicious and potentially place them in the spam or quarantine folder.
- `fo=1` requests failure reports when either SPF or DKIM authentication fails to align.

This helped me understand the domain's published email authentication policy and compare it with the results found in the original email headers.

## 5. Malicious Attachment Analysis

After checking the email content and headers, I moved on to investigating the attachment.

The attachment filename was:

`SWT_#09674321____PDF__.CAB`

Because the email contained an unexpected attachment, I decided to examine the file before opening it.

### 5.1 Downloading the Attachment

I downloaded the attachment into the TryHackMe virtual environment.

After downloading it, I opened the terminal and used `ls` to check the available files. I then changed the directory to the Desktop, where I had saved the attachment.

### 5.2 Calculating the SHA256 Hash

To identify the file and prepare it for further investigation, I used the following command:

```bash
sha256sum 'SWT_#09674321____PDF__.CAB'
```

The command generated a SHA256 hash for the attachment.

**SHA256 hash:**

`2e91c533615a9bb8929ac4bb76707b2444597ce063d84a4b33525e25074fff3f`

I used this hash to search for the file on VirusTotal.

### 5.3 Investigating the File with VirusTotal

After calculating the hash, I opened [VirusTotal](https://www.virustotal.com/) and searched for the SHA256 value.

The results provided more information about the file and its reputation.

Some of the details I noticed were:

| Information | Result |
|---|---|
| Community Score | 49 out of 64 |
| Popular Threat Label | `svgtrojan.msil/loki` |
| Threat Categories | Trojan, Ransomware |
| File Type | RAR |
| File Size | `[Insert exact size in KB]` |

The results raised serious concerns about the attachment. The community score and threat labels indicated that the file had been flagged as suspicious by multiple contributors or detection sources.

I also noticed that the filename ended in `.CAB`, but VirusTotal identified the file type as RAR. This was another reason not to trust the file based only on its name or extension.

I did not need to execute the attachment to investigate it. Searching for its hash on VirusTotal allowed me to review the available threat information.

## 6. Suspicious Indicators Identified

During the investigation, I found several details that made the email suspicious:

1. **Generic greeting:** The email did not address the recipient personally.
2. **Unexpected payment request:** It claimed that a money transfer had been completed through SWIFT.
3. **Different Reply-To address:** The reply address used a different domain from the sender address.
4. **SPF failure:** The original email headers showed that SPF authentication failed.
5. **DMARC information:** The original headers reported DMARC as unknown.
6. **Unexpected attachment:** The email included a file named `SWT_#09674321____PDF__.CAB`.
7. **Suspicious file reputation:** VirusTotal returned a community score of 49 out of 64 and threat labels associated with Trojan and Ransomware.
8. **File type mismatch:** The filename ended in `.CAB`, while VirusTotal identified the file type as RAR.

## 7. Tools Used

| Tool | Purpose |
|---|---|
| Thunderbird | To inspect the email and original headers. |
| WHOIS Lookup | To identify the organisation associated with the originating IP. |
| Dmarcian | To check SPF and DMARC records. |
| Linux Terminal | To locate the downloaded attachment. |
| `sha256sum` | To calculate the SHA256 hash of the attachment. |
| VirusTotal | To investigate the file hash, reputation and threat information. |

## 8. My Assessment

Based on the evidence I collected, I assessed this email as a phishing attempt and the attachment as malicious.

The unexpected payment notification, generic greeting, different Reply-To address and SPF failure all raised concerns about the authenticity of the message.

The attachment investigation provided further evidence. VirusTotal showed a community score of 49 out of 64 and threat labels associated with Trojan and Ransomware.

The email also appeared to use a financial request to encourage the recipient to trust the payment notification and its attachment.

This behaviour could be consistent with spear-phishing or a business email compromise attempt. However, I would need more evidence to confirm the exact attack technique.

## 9. What I Learned

This practical helped me understand how different investigation techniques can be used together when analysing suspicious emails.

I gained hands-on experience in:

- Inspecting email headers in Thunderbird.
- Checking sender and Reply-To addresses.
- Investigating IP ownership using WHOIS.
- Checking SPF and DMARC records using Dmarcian.
- Locating suspicious attachments in a Linux environment.
- Generating SHA256 hashes using the terminal.
- Searching file hashes using VirusTotal.
- Identifying suspicious file types and threat labels.
- Recording evidence and documenting investigation findings.

One important thing I learned is that a file should not be trusted just because its name makes it look like a normal document. Checking its actual file type and hash can reveal information that is not obvious from the filename.

## 10. Evidence

The following screenshots show the steps I completed during the investigation.

### Evidence 1 – Original Suspicious Email
<img width="2080" height="1676" alt="image" src="https://github.com/user-attachments/assets/071977f7-77ea-4734-9e34-9cc688de2897" />


### Evidence 2 – Email Header Analysis
<img width="2075" height="432" alt="image" src="https://github.com/user-attachments/assets/ddeef8b4-784a-499d-9f06-9687c13e6f4b" />
<img width="2084" height="1675" alt="image" src="https://github.com/user-attachments/assets/476d4279-b488-4355-b95b-ef01433f8690" />


### Evidence 3 – WHOIS Investigation
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/297dfeb4-a8eb-4e7c-8e96-a929bfc3850f" />


### Evidence 4 – SPF and DMARC Records

<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/143110fc-cc1c-4b5f-8a7f-82a5d96c4177" />

<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/013d9ecb-24ec-4e0e-a8bb-59a91ce1c2cc" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/ba79e535-4d1e-442c-bf28-458a0c0ce927" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/36e7248a-4a5f-4c5e-9b28-a8d5e22d2aa1" />




### Evidence 5 – SHA256 Hash Calculation
Step by step process.
<img width="2078" height="1673" alt="image" src="https://github.com/user-attachments/assets/802360c9-35ea-4814-9a16-8af399608d69" />
<img width="2080" height="1674" alt="image" src="https://github.com/user-attachments/assets/28f1a9ee-5c97-4ce0-a1f7-3aaf61f25421" />
<img width="2080" height="1676" alt="image" src="https://github.com/user-attachments/assets/2c92301f-89ac-4386-ad64-3128ea2b2120" />
<img width="2084" height="1682" alt="image" src="https://github.com/user-attachments/assets/8cddbb3b-9d88-42ac-b118-4caa3bca2182" />




### Evidence 6 – VirusTotal Investigation
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/ec2da3fa-86ff-4977-ac1c-4993774d8c24" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/2e6313df-5349-4b98-90d5-b96265b64682" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/252d6004-1e4c-4744-9125-9da435121a54" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/c9553654-b1b9-413e-838b-b30c28f5b968" />



### Additional Evidence  – Email Investigation


## 11. Conclusion

This practical gave me the opportunity to investigate a suspicious email from the perspective of a Level 1 SOC Analyst.

I started by checking the email content and sender details. I then examined the original headers, investigated the originating IP, checked the SPF and DMARC records and calculated the attachment's SHA256 hash.

Finally, I used VirusTotal to investigate the attachment and found several suspicious indicators.

Based on the evidence, I concluded that the email was a phishing attempt and that the attachment should be treated as malicious.

This exercise helped me improve my understanding of email investigations, authentication checks, threat intelligence and malware identification.
