# simulated-security-incident-login-after-hours
Simulated investigation of a suspicious login outside working hours. This project demonstrates the incident analysis process, including log review, IP reputation checks, and security recommendations.

## Context
This project simulates a typical security incident that a SOC Analyst might face: a suspicious login performed outside of regular working hours using a privileged account.

The goal is to demonstrate how I would investigate this type of alert using free online tools, logical reasoning, and structured documentation.

On **Sunday, June 22nd, 2025**, at **03:17 AM**, a successful login was registered under the user account `admin_hr` (from the Human Resources department). This activity was flagged as suspicious because it is known:


---

## Simulated Logs

```txt
2025-06-22 03:17:02 - LOGIN - User: admin_hr - IP: 202.88.34.101 - Status: SUCCESS
2025-06-22 03:17:22 - FILE_ACCESS - User: admin_hr - File: /hr/confidential/employee_salaries.xlsx - Action: OPEN
2025-06-22 03:17:45 - FILE_ACCESS - User: admin_hr - File: /hr/confidential/employee_salaries.xlsx - Action: DOWNLOAD
2025-06-22 03:18:05 - NETWORK - IP: 202.88.34.101 - Port: 443 - Action: OUTBOUND to dropbox.com
2025-06-22 03:18:16 - LOGOUT - User: admin_hr - IP: 202.88.34.101

Note: These logs are fictional and created for educational purposes only!

```
---

## First Impressions on the issue

- The company operates from **Monday to Friday, 09:00 to 17:00**.
- The company operates **only** within Europe.
- HR accounts are **not expected** to be active during off-hours or weekends. 
- The login occurred **on a Sunday at 03:17 AM**, far outside of normal working hours.
- No remote work authorization was reported from the company for that weekend.


According to the logs, the `admin_hr` user:
- Opened a **confidential Excel spreadsheet** containing employee salary information.
- Accessed and downloaded the file.
- Immediately afterward, the system registered an **outbound connection to dropbox.com**, indicating a possible file transfer.
- Finally, the user **logged out** shortly after.

---

##  Investigation Steps

To further investigate my assumptions of a possible unauthorized access and breach of information from the company, I used some online tools.

### 1. Geo-IP Lookup
- Tool used: [ipinfo.io](https://ipinfo.io)
- IP: `202.88.34.101`
- Location: Beijing, China
- This location is inconsistent with the company’s expected access regions (knowing that the Company only operates within Europe).
- The access occurred outside of working hours and came from an unfamiliar country.


![Result from the IP search](/Screenshots/Screenshot_IP_address.jpg)



### 2. IP Reputation Check
- Tool used: [abuseipdb.com](https://www.abuseipdb.com)
- Result: The IP is not flagged as malicious according to AbuseIPDB. However, this does not necessarily mean it came from a reliable source or a legitimate user.
- There is no prior record of international logins associated with this account.  
- The employee linked to the account denied accessing the system at the reported time.  
- No remote work authorization was recorded for that weekend.


![Result from the Reputation check](/Screenshots/Screenshot_AbuseIPDB.jpg)



My initial hypothesis remains unchanged: this login is considered unauthorized and remains a strong indicator of potential compromise.


### 3. Account Usage History
- `admin_hr` has no history of access outside business hours.
- Usual logins occur from the internal network during normal business hours.

### 4. MFA Verification
- MFA was technically enabled for this account.
- However, the logs suggest that MFA was either misconfigured or bypassed.

### 5. User Confirmation
- The HR team also confirmed that **no one accessed the system** at that time.
- They also confirmed the file in question is confidential and access is highly restricted.

---

##  Findings

- The login is highly suspicious and likely unauthorized.
- A confidential HR file was opened and downloaded.
- There was a potential exfiltration of data to a third-party cloud service (Dropbox).
- The account’s MFA was not properly protecting the login.
This indicates a potential data breach or credential compromise.

---

##  Recommended Actions

- **Block the external IP address** immediately.
- **Reset the password** of `admin_hr` immediately.
- **Audit all user accounts** with elevated privileges.
- **Reconfigure MFA** and enforce strict policies.
- **Search Dropbox for any unauthorized uploads** using DLP.
- **Conduct a full forensic audit** of the HR system.
- **Notify management and follow the incident response plan**.
- Raise awareness internally about access control best practices.

---

## Key Takeaways

- This project represents my first attempt at documenting a full incident response flow from scratch.
- Learned to simulate a real-world SOC scenario based on simple log analysis.
- Applied free tools to assess IP reputation and geolocation to help me investigate this case.
- Practiced structuring a clear incident investigation.
- Strengthened my ability to write technical documentation for security reports.


---

##  Tools Used

- [ipinfo.io](https://ipinfo.io)
- [abuseipdb.com](https://www.abuseipdb.com)
- GitHub + Markdown
