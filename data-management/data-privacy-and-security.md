# Data privacy and security

### Introduction

Data privacy and security are foundational to maintaining the confidentiality, integrity, and availability of sensitive information in today's digital landscape. For organizations hosting AMRIT, implementing a robust security framework is essential to safeguard data from unauthorized access, breaches, and leaks. This document outlines key best practices and technologies that organizations should adopt to enhance data security and privacy when deploying AMRIT. These recommendations cover Virtual Private Network (VPN) usage, data replication, HTTPS implementation, Role-Based Access Control (RBAC), annual Vulnerability Assessment and Penetration Testing (VAPT), secure APIs, and server hardening with firewalls.

#### 1. VPN (Virtual Private Network)

A VPN secures data by creating an encrypted tunnel between a user’s device and a secure network. This ensures that sensitive information transmitted over the internet remains protected from interception.

AMRIT infrastructure hosted on PSMRI's on-premise servers uses SOPHOS VPN services that adhere to industry standards such as AES-256 encryption to ensure the confidentiality and integrity of data transmitted over public networks.

{% file src="../.gitbook/assets/sophos (1).png" %}

#### 2. Replication

Data replication ensures the availability and integrity of data by creating copies of critical information across multiple locations.

AMRIT  implements continuous replication for all critical systems and databases, ensuring that data is replicated and accessible at all times by maintaining replication servers.

#### 3. HTTPS Everywhere

Enforcing HTTPS ensures that all data transmitted between the user's browser and the server is encrypted, preventing eavesdropping and man-in-the-middle attacks. HTTPS uses SSL/TLS encryption to secure data during transmission, making it more difficult for attackers to intercept sensitive information.

All of our public-facing websites and applications enforce HTTPS by default. We regularly audit our certificates to ensure they are up-to-date and secure.![](file:///C:/Users/SRINIV~1/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

{% file src="../.gitbook/assets/SSL (2).png" %}

#### 4. RBAC (Role-Based Access Control)

RBAC restricts access to sensitive data based on user roles, ensuring that only authorized personnel have access to specific resources.

#### Key Points:

* **Access control:** Users are granted permissions based on their role, limiting access to information based on necessity.
* **Principle of least privilege:** This ensures that users only have access to the data necessary to perform their job functions.
* **Evidence:** AMRIT implements a strict RBAC policy and the IT team regularly reviews user roles and permissions to minimize exposure to sensitive data. Access control logs are maintained.

#### 5. VAPT (Vulnerability Assessment and Penetration Testing) – Annual Testing

VAPT helps identify and remediate security vulnerabilities before they can be exploited by attackers. Regular vulnerability scans identify weaknesses in the system that could be exploited. Penetration testing simulates attacks to identify vulnerabilities in applications, networks, and systems.

Our organization conducts VAPT annually, using certified third-party vendors to assess and test our systems. All identified vulnerabilities are addressed with high priority, and remediation actions are documented.

{% file src="../.gitbook/assets/Security Audit (VAPT) Clearance Certificate (CNPL23-24300823119).pdf" %}

#### 6. Secure APIs

Secure APIs are essential for protecting data and ensuring safe communication between applications.

**AMRIT achieves secure APIS through:**

* **Authentication and Authorization:** All AMRIT APIs must require secure authentication with JWT tokens and strict access controls are enforced.
* **Encryption:** All API traffic is encrypted using HTTPS to prevent interception of sensitive data.

AMRIT APIs follow industry-standard security practices, including secure tokens and proper logging of all API interactions to ensure secure data exchange between systems.

&#x20;**7. Hardening the servers and firewalls**

Server and firewall hardening enhances system security by reducing vulnerabilities and preventing unauthorized access.

AMRIT IT team performs regular server hardening, including disabling unnecessary ports and services, applying patches promptly, and using intrusion detection systems (IDS). Firewalls are configured based on the principle of least privilege, with logging enabled to detect suspicious activity.

#### Conclusion

By following these recommendations, organizations hosting AMRIT can achieve a robust level of data privacy and security. Implementing VPNs, data replication, HTTPS, RBAC, VAPT, secure APIs, and server hardening ensures protection against potential breaches while maintaining the integrity and confidentiality of sensitive data. These practices not only safeguard user information but also reinforce trust in AMRIT as a secure and reliable healthcare platform.

&#x20;
