# Email Investigation — email3.eml

## 1. Investigation Overview

| Detail | Information |
|---|---|
| Platform | TryHackMe |
| Learning Path | SOC Level 1 |
| Module | Phishing Analysis |
| Investigation | Phishing Analysis Fundamentals |
| File Analysed | `email3.eml` |
| Investigation Type | Email Header Analysis |
| Status | Completed |

## 2. Investigation Findings

| Investigation Item | Finding |
|---|---|
| Impersonated Organization | Home Depot |
| Sender Email Address | `support@teckbe.com` |
| X-Originating-IP | `103[.]234[.]236[.]83` |
| Authentication-Results Server | `atlas102.free.mail.gql.yahoo.com` |

## 3. Investigation Methodology

1. Opened `email3.eml` inside the TryHackMe virtual machine.
2. Inspected the email source to examine its headers and sender information.
3. Identified the sender's email address.
4. Located the `X-Originating-IP` header in the email source.
5. Used CyberChef to defang the originating IP address.
6. Examined the `Authentication-Results` header to identify the mail server.

## 4. Tools Used

- **TryHackMe Virtual Machine:** Used to access and inspect the email file.
- **Email Source Viewer:** Used to examine the raw email headers.
- **CyberChef:** Used to defang the originating IP address.

## 5. Key Skills Demonstrated

- Email source inspection
- Email header analysis
- Sender identification
- IP address identification and defanging
- CyberChef usage
- Phishing investigation

## 6. Key Learning Outcomes

This investigation improved my understanding of email header analysis and the importance of examining email metadata during phishing investigations.

I gained practical experience identifying sender information, locating originating IP addresses, and examining authentication-related headers.

## 7. Evidence

Screenshots supporting this investigation:

### 1. Email source and header inspection


 <img width="2084" height="1677" alt="image" src="https://github.com/user-attachments/assets/fa2157d7-24f8-460e-9ed2-b745cf1a7d73" />



### 2. Sender email identification



  <img width="2086" height="1671" alt="image" src="https://github.com/user-attachments/assets/7d899be6-eed6-48f8-974d-13cfae248533" />



 ### 3. X-Originating-IP identification



  <img width="2081" height="1670" alt="image" src="https://github.com/user-attachments/assets/975d5cba-19a2-4522-889a-83fef86940cc" />



### 4. CyberChef IP defanging process



  <img width="2940" height="1912" alt="image" src="https://github.com/user-attachments/assets/027eb63b-1ed0-4338-b26a-2178d601a7cc" />


## 8. Conclusion

This exercise provided practical experience in examining suspicious email metadata and identifying information relevant to phishing investigations.

It reinforced the importance of analysing email headers as part of the initial investigation and triage process performed by SOC analysts.

---

**Training Platform:** TryHackMe — SOC Level 1

*This investigation was conducted in an authorised training environment. Findings are documented for educational and portfolio purposes.*
