# Phishing Emails in Action – Netflix Email Investigation

## 1. Overview

In this TryHackMe practical, I took on the role of a Level 1 SOC Analyst. My task was to investigate suspicious emails reported by users and identify possible signs of phishing.

For this investigation, I examined an email that appeared to come from Netflix. I checked the sender address, email subject, message content, email headers and the link included in the email.

I also used URLScan.io to investigate the shortened URL and check its reputation.

## 2. Email Investigation

### 2.1 Checking the Sender Address

The display name showed **Netflix**, but when I checked the actual sender address, I found:

`JGQ47wazXelxYVBrkeDg-JOg70DDQwWdR@JOg70DDQwWdR-yVkCaBkTNp.gogolecloud.com`

The sender address looked unusual and did not appear to belong to an official Netflix domain.

This made me suspicious because the display name alone cannot confirm who actually sent an email. It is important to check the real sender address rather than simply trusting the company name displayed in the inbox.

### 2.2 Checking the Email Subject

The subject of the email was:

`YourNetflixAccountisonHold`

I noticed that all the words were joined together without spaces. The formatting looked unusual and did not appear professional.

This was another indicator that made me look more closely at the email.

### 2.3 Checking the Email Body

When I examined the email body, I noticed the following:

- The message used Netflix branding and an HTML logo.
- It contained a button asking the recipient to update their account.
- A contact telephone number and physical address were included at the bottom of the email.

The Netflix logo could make the email look genuine to someone who did not check the sender details.

I was particularly interested in the account-update button because I wanted to find out where the link would take the recipient.

## 3. Email Header Analysis

### 3.1 Inspecting the Email Source

To investigate the email headers, I opened the message in Thunderbird and selected:

`View → Message Source`

I examined the message source to find additional information about the email, including the recipient, sender IP address and Return-Path.

### 3.2 Information Found

| Header Information | Finding |
|---|---|
| Intended Recipient | `redacted@yahoo.com` |
| Sender IP Address | `10.197.37.234` |
| Return-Path Domain | `etekno.xyz` |

One thing that caught my attention was the Return-Path domain, `etekno.xyz`. It was different from the Netflix branding used in the email, so I noted it as a domain of interest for further investigation.

I also found the IP address `10.197.37.234` in the email headers. This is a private IP address, so I cannot treat it as the public sending server's IP without further evidence.

## 4. Investigating the Suspicious Link

### 4.1 Extracting the URL

I checked the **UPDATE ACCOUNT NOW** button to identify the link behind it.

The shortened URL was:

`https://t.co/yuxfZm8kpg?amp=1`

The link used the `t.co` URL-shortening domain. Since shortened links hide the full destination, I decided to investigate it using URLScan.io.

### 4.2 URL Analysis Using URLScan.io

I submitted the shortened URL to [URLScan.io](https://urlscan.io/) to check its reputation and review the available scan information.

The results showed:

| Information | Result |
|---|---|
| Tool | URLScan.io |
| Hostname | `t.co` |
| Canonical URL | `t.co/yuxfZm8kpg` |
| Hostname Reputation | Potentially Malicious |
| Malicious Verdicts for Hostname | 156 |
| Malicious Verdicts for This URL | None reported |
| Number of URL Scans | 1 |

The result caught my attention because URLScan.io reported the hostname as potentially malicious, with 156 malicious verdicts.

However, the specific URL I submitted had no malicious verdicts reported in the scan results.

I understood that the hostname reputation alone was not enough to confirm that this particular link was malicious. I would need to investigate its redirect destination before reaching a final conclusion.

## 5. Suspicious Indicators Identified

During the investigation, I identified several details that raised suspicion:

1. **Unusual sender address:** The display name showed Netflix, but the actual email address used an unusual domain.
2. **Suspicious subject:** The words in the subject were joined together without spaces.
3. **Brand impersonation:** The email used Netflix branding and an HTML logo to appear genuine.
4. **Account-update request:** The message included a button asking the recipient to update their account.
5. **Different Return-Path domain:** The email headers contained `etekno.xyz`, which was different from the Netflix branding.
6. **Shortened URL:** The account-update button used a shortened link, hiding its full destination.
7. **URL reputation warning:** URLScan.io reported the `t.co` hostname as potentially malicious.

## 6. Tools Used

| Tool | How I Used It |
|---|---|
| Thunderbird | To open the email and inspect its original source and headers. |
| URLScan.io | To investigate the shortened URL and check its reputation. |

## 7. My Assessment

Based on the information I collected, I considered this email suspicious and a possible Netflix phishing attempt.

The unusual sender address, strange subject formatting, different Return-Path domain and account-update link all raised concerns.

The URLScan.io results provided additional information about the hostname's reputation. However, the specific URL had no malicious verdicts reported in the scan.

I have not confirmed the final redirect destination, so I cannot make a definite claim about the website behind the link.

## 8. What I Learned

This investigation helped me understand the importance of checking an email carefully rather than trusting its display name or branding.

I gained practical experience in:

- Checking the actual sender email address.
- Inspecting email source and headers in Thunderbird.
- Identifying suspicious domains using the Return-Path field.
- Extracting shortened URLs from email content.
- Using URLScan.io to investigate suspicious links.
- Recording findings and assessing different phishing indicators.

I also learned that a single warning or suspicious detail does not automatically prove an email is malicious. It is important to consider all available evidence before reaching a conclusion.

## 9. Evidence

The following screenshots show the steps I completed during the investigation.

### Evidence 1 – Suspicious Netflix Email

Screenshot showing the Netflix display name, sender address, subject and email content.

<img width="2087" height="1677" alt="Suspicious Netflix email" src="https://github.com/user-attachments/assets/e1b99cab-1016-4f94-8640-103aa8891356" />

### Evidence 2 – Email Header Analysis

Screenshots showing the email source and relevant header information.

<img width="2075" height="1681" alt="Email header investigation" src="https://github.com/user-attachments/assets/a3ce823b-7a9e-42d8-86e0-3330c833e04c" />

<img width="2083" height="1681" alt="Email header details" src="https://github.com/user-attachments/assets/e5fa7849-92c1-4a57-8a55-85d92ad85dc5" />

### Evidence 3 – Email Footer Inspection

Screenshot showing the contact details and address included at the bottom of the email.

<img width="2097" height="1674" alt="Email footer inspection" src="https://github.com/user-attachments/assets/e3ce1c6d-1d8e-4b3c-9337-e409cf83b566" />

### Evidence 4 – URLScan.io Investigation

Screenshot showing the URLScan.io results for the shortened link.

<img width="2940" height="1912" alt="URLScan.io results" src="https://github.com/user-attachments/assets/9c630231-dfb2-42ac-a72a-abf3df1ea3ec" />

### Evidence 5 – Additional Investigation Evidence

Additional screenshots collected during the investigation.

<img width="2089" height="1675" alt="Additional investigation evidence 1" src="https://github.com/user-attachments/assets/fe5cab84-d334-4efc-8c26-a9ac89c7e3b7" />

<img width="2089" height="1671" alt="Additional investigation evidence 2" src="https://github.com/user-attachments/assets/381dc666-c0ec-4b18-a8b8-f3201e1f98b9" />

## 10. Conclusion

This practical gave me a better understanding of how a Level 1 SOC Analyst investigates suspicious emails.

I was able to examine the sender details, review the email headers, identify a domain of interest and investigate a shortened URL using URLScan.io.

The exercise showed me why it is important to check different parts of an email before deciding whether it can be trusted.

It also helped me build confidence in using investigation tools and documenting findings in a clear way.
