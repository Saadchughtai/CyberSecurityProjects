# Snapped Phish-ing Line – TryHackMe Investigation

## About This Project

In this room, I investigated a phishing campaign targeting employees at SwiftSpend Financial.

The scenario involved several employees receiving suspicious emails. Some employees had already entered their login details on a phishing website and lost access to their accounts.

I worked through the investigation step by step to understand how the phishing emails were sent, how the fake website worked and how the attacker collected employee credentials.

This was a good opportunity for me to practise email analysis, Linux commands, URL investigation and basic threat intelligence.

## 1. Reviewing the Phishing Emails

I started by opening the `phish-emails` folder on the desktop. There were four email samples to review.

I first investigated the email with the subject **Quote for Services Rendered**. I opened the email headers and checked the sender, recipient and subject details.

I found the following information:

| Detail | Finding |
|---|---|
| Sender name | Group Marketing Online Accounts Payable |
| Sender email | `Accounts.Payable@groupmarketingonline.icu` |
| Recipient | William McClean |
| Recipient email | `william.mcclean@swiftspend.finance` |
| Subject | Quote for Services Rendered |

This helped me identify who received the email and the email address used by the attacker to send the phishing messages.

**Evidence:** 
<img width="2080" height="1676" alt="image" src="https://github.com/user-attachments/assets/ac5701d5-dc1c-45be-9144-faabb92626ca" />
<img width="1669" height="374" alt="image" src="https://github.com/user-attachments/assets/ae4bf7d5-3618-46d0-85b8-c5072d1af89b" />



## 2. Investigating the Email Attachment and URL

Next, I opened the email addressed to Zoe Duncan and investigated its attachment.

I saved the attachment in the TryHackMe virtual machine and used the Linux terminal to inspect the file contents. While checking the file, I found a URL pointing to:

`kennaroads.buzz`

I wanted to understand more about this URL, so I checked it using VirusTotal. I also used CyberChef to defang the URL so it would not be clickable when included in my notes.

When I opened the URL in the lab environment, I noticed that the website was designed to look like a Microsoft page.

This helped me understand how attackers can use familiar company branding to make phishing websites look more trustworthy.

**Evidence:** 
<img width="2077" height="1682" alt="image" src="https://github.com/user-attachments/assets/01ce6c40-9ca0-4609-af81-e009e5bdafc0" />
<img width="2080" height="1674" alt="image" src="https://github.com/user-attachments/assets/dc1e63f0-4cd4-412c-8aba-532528657007" />
<img width="2078" height="1674" alt="image" src="https://github.com/user-attachments/assets/0309a95e-9d12-461a-b0f1-ed11edfb6ada" />
<img width="2080" height="1679" alt="image" src="https://github.com/user-attachments/assets/a596a47b-e374-45d2-904c-546fa2c6a336" />
<img width="2078" height="1675" alt="image" src="https://github.com/user-attachments/assets/70490866-6096-4428-8b83-61dbf1f07a70" />
<img width="2081" height="1675" alt="image" src="https://github.com/user-attachments/assets/56cc6240-be54-4005-a3a9-30e9698edb6d" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/3e94468d-2970-4184-b994-a9f1302c8c6f" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/f84f3e05-3d19-4ab5-bdc2-5b967b6cd105" />




## 3. Checking the Hosts File and IP Address

I also wanted to understand how the phishing domain was mapped to an IP address.

I checked the hosts file using:

```bash
cat /etc/hosts
```

I found that the phishing domain was already mapped to an IP address.

After that, I checked the IP addresses of my investigation machine using:

```bash
ip a | grep 172
```

The IP address mapped to the phishing domain was different from the IP address of my investigation machine. This helped me understand that the phishing website was hosted on another system.

I also visited the domain without adding any extra path and noticed that it displayed a WordPress website.

**Evidence:** 
<img width="2077" height="1676" alt="image" src="https://github.com/user-attachments/assets/2a083bc1-2453-44d6-a605-0ab27ecfdd38" />
<img width="2077" height="1673" alt="image" src="https://github.com/user-attachments/assets/f92d40aa-6592-46dd-bbf9-85a20abb830a" />
<img width="2076" height="1677" alt="image" src="https://github.com/user-attachments/assets/1f62ccd6-87dc-4d14-a429-de75b5acb47e" />
<img width="2082" height="1673" alt="image" src="https://github.com/user-attachments/assets/e95d0ce2-3345-4e18-9ced-e8ea4760fb06" />
  



## 4. Finding the Exposed Archive

I continued investigating the website to see whether any files had been left publicly accessible.

I navigated to the `/data` directory and found an archive called:

`update365.zip`

I downloaded the archive into the virtual machine because I wanted to inspect what it contained.

I then used the following command to calculate its SHA-256 hash:

```bash
sha256sum update365.zip
```

**SHA-256 hash:** `ba3c15267393419eb08c7b2652b8b6b39b406f300ae8a18fee4d16b19ac9686`

This was useful practice in using Linux to calculate file hashes, which can help identify and investigate suspicious files.

**Evidence:** 
<img width="2077" height="1675" alt="image" src="https://github.com/user-attachments/assets/6ae678b3-3ca7-48a4-880f-397b19dd3b45" />
<img width="2084" height="1676" alt="image" src="https://github.com/user-attachments/assets/57014214-d948-4752-809a-472163d50fd2" />



## 5. Checking the Archive with VirusTotal

After calculating the hash, I searched for it on VirusTotal.

I checked the results to understand what type of threat was associated with the archive.

Apart from phishing, VirusTotal also showed the **Trojan** threat category.

I then opened the Details section and found that the archive contained 49 files.

My findings were:

- Archive name: `update365.zip`
- Additional threat category: Trojan
- Number of files: 49

This made me curious about what was inside the archive and how the phishing kit worked.

**Evidence:** 
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/42e8c6ff-0823-4336-9e29-be4d79230d9c" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/edb8de88-c4d6-4aac-b54c-89d5410fbb89" />



## 6. Checking the Credential Logs

Next, I investigated the log file inside the following directory:

`/data/Update365/`

While reviewing the logs, I found that one employee's email address appeared more than once.

The email address was:

`michael.ascot@swiftspend.finance`

This showed that the employee had submitted their credentials more than once through the phishing page.

It helped me understand how the attacker was collecting information from employees who interacted with the fake website.

**Evidence:** 
<img width="2077" height="1675" alt="image" src="https://github.com/user-attachments/assets/784d1eb4-0d6c-4fb0-a6fb-9beb088cea1f" />


## 7. Investigating the submit.php File

After downloading the archive, I extracted it using the Linux `unzip` command.

```bash
unzip update365.zip
```

I then searched for the `submit.php` file and opened it using:

```bash
cat submit.php
```

I wanted to understand what happened to the information entered by users on the phishing page.

By checking the file contents, I was able to find the email address configured to receive the captured credentials.

**Credential collection email:** `m3npat@yandex.com`

This was an interesting part of the investigation because it helped me understand how a phishing kit can collect and send stolen login details to an attacker.

**Evidence:** 
<img width="2076" height="1671" alt="image" src="https://github.com/user-attachments/assets/6600d12f-01ca-433c-bac9-4925ffc92d8e" />
<img width="2085" height="1674" alt="image" src="https://github.com/user-attachments/assets/c7200e40-4a4a-4dc6-b5c9-0abe3980633c" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/f2375a94-3feb-42aa-a796-a943fed0cb55" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/351f4ba9-729c-4424-9bdd-e2090a280891" />





## 8. Finding and Decoding the Flag

For the final task, I returned to the phishing URL and looked for the `flag.txt` file.

I used CyberChef to decode the content. I followed the Base64 decoding and reverse-text steps to reveal the flag.

The decoded flag was:

```text
THM{pL4y_w1Th_tH3_URL}
```

**Evidence:** 
<img width="2087" height="1670" alt="image" src="https://github.com/user-attachments/assets/01fb1788-9599-47fb-aee7-96c7d2e7ef8d" />
<img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/2009d8e3-7996-49cd-85cb-9f3040aea576" />





## Tools I Used

| Tool | What I used it for |
|---|---|
| Linux Terminal | Checking files, extracting the archive and calculating the SHA-256 hash |
| VirusTotal | Checking the phishing URL and archive hash |
| CyberChef | Defanging the URL and decoding the flag |
| Web Browser | Visiting and inspecting the phishing website |
| TryHackMe | Practising the investigation in a virtual lab |

## What I Learned

This room helped me understand phishing attacks in more detail. Before this investigation, I mainly understood phishing as suspicious emails containing fake links. Working through this room helped me see what can happen behind those links.

I learned how to:

- Review email headers and identify sender and recipient information.
- Investigate suspicious URLs and check their reputation.
- Defang URLs before including them in investigation notes.
- Use Linux commands to inspect files, extract ZIP archives and calculate hashes.
- Use VirusTotal to investigate suspicious files and URLs.
- Review log files to identify repeated credential submissions.
- Inspect PHP files to understand how a phishing kit collects credentials.
- Use CyberChef to decode information.

I also learned that investigating a phishing attack involves more than just looking at the email. It is important to follow the evidence, examine the files and understand how the different parts of the attack are connected.

Overall, this practical exercise helped me improve my investigation skills and build confidence using tools that are useful in a SOC Analyst role.

---

**Note:** This is a lab-based investigation completed on TryHackMe. All findings relate to the provided training environment.
