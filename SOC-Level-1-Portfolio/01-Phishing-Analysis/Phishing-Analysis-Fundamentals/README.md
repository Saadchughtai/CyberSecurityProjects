# Phishing Email Investigation — email3.eml

## 1. About This Investigation

As part of the Phishing Analysis Fundamentals room on TryHackMe, I investigated a suspicious email named `email3.eml` using the provided virtual machine.

The aim was to examine the email, understand who sent it, identify the organisation being impersonated, and inspect some of the technical information contained in its headers.

I used the email source and CyberChef to investigate the information and record my findings.

## 2. What I Found

During the investigation, I identified the following details:

| Item | Finding |
|---|---|
| Impersonated organisation | Home Depot |
| Sender address | `support@teckbe.com` |
| X-Originating-IP | `103[.]234[.]236[.]83` |
| Authentication-Results server | `atlas102.free.mail.gql.yahoo.com` |

One detail that caught my attention was the difference between the sender's display name and the actual email domain. The message displayed Home Depot as the sender, but the email address used the `teckbe.com` domain.

This was an indicator worth investigating further.

## 3. How I Investigated the Email

I followed these steps during the exercise:

1. Opened `email3.eml` in the TryHackMe virtual machine.
2. Inspected the original email message to understand its contents and structure.
3. Checked the sender information and identified the email address.
4. Examined the email headers and located the `X-Originating-IP` field.
5. Copied the IP address into CyberChef and used it to produce a defanged version.
6. Continued examining the headers and located the `Authentication-Results` field to identify the reporting mail server.

## 4. Tools I Used

| Tool | How I Used It |
|---|---|
| TryHackMe Virtual Machine | Accessed and investigated the provided email |
| Email Source Viewer | Inspected the original message and its headers |
| CyberChef | Defanged the originating IP address |

## 5. What I Learned

This exercise helped me understand why looking beyond the visible contents of an email is important during a phishing investigation.

I learned how to:

- Inspect the original source of an email.
- Identify differences between a sender's display name and actual email address.
- Locate useful information in email headers.
- Identify an originating IP address.
- Use CyberChef to defang an IP address.
- Identify the server named in an `Authentication-Results` header.

## 6. Investigation Evidence

Below are screenshots I captured during the investigation to show the steps I followed and the information I identified.

### 1. Email Source and Header Inspection

This screenshot shows the email I examined inside the virtual machine.

<img width="2084" height="1677" alt="Email source and header inspection" src="https://github.com/user-attachments/assets/fa2157d7-24f8-460e-9ed2-b745cf1a7d73" />

### 2. Identifying the Sender

Here I examined the sender details and identified the email address used in the message.

<img width="2086" height="1671" alt="Sender email identification" src="https://github.com/user-attachments/assets/7d899be6-eed6-48f8-974d-13cfae248533" />

### 3. Finding the X-Originating-IP

This screenshot shows where I located the `X-Originating-IP` field in the original email source.

<img width="2081" height="1670" alt="X-Originating-IP identification" src="https://github.com/user-attachments/assets/975d5cba-19a2-4522-889a-83fef86940cc" />

### 4. Defanging the IP Address with CyberChef

I used CyberChef to convert the IP address into a defanged format:

`103[.]234[.]236[.]83`

<img width="2940" height="1912" alt="CyberChef IP defanging process" src="https://github.com/user-attachments/assets/027eb63b-1ed0-4338-b26a-2178d601a7cc" />

## 7. Final Thoughts

This was a useful hands-on exercise because it gave me an opportunity to work with an actual email sample rather than just read about phishing techniques.

It helped me become more comfortable inspecting email headers, identifying suspicious sender information, and using CyberChef during an investigation.

These are skills I want to continue developing as I progress through the TryHackMe SOC Level 1 learning path.

---

**Platform:** TryHackMe  
**Learning Path:** SOC Level 1  
**Room:** Phishing Analysis Fundamentals

*This investigation was completed in an authorised training environment. The findings and screenshots document my own learning process.*
