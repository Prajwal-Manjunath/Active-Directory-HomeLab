<h1>Active Directory Homelab</h1>

<h2>Description</h2>
This project focuses on building and testing Group Policies in a Windows Server Active Directory (AD) lab environment. Using the **Group Policy Management Console (GPMC)**, I created and enforced policies to improve security and user management. The project demonstrates how centralized policy management can be applied to control user access, enforce security standards, and manage system configurations across a domain.
<br />

<h2>Objectives</h2>

- <b>Install and Configure GPMC:</b> Add Group Policy Management Console via Server Manager.  
- <b>Understand GPMC Structure:</b> Learn differences between User Configuration vs. Computer Configuration and Policies vs. Preferences.  
- <b>Enforce Security Policies:</b> Apply Password Policy and Account Lockout Policy.  
- <b>Manage User Access:</b> Restrict Control Panel access and block USB storage devices.  
- <b>Improve User Experience:</b> Configure Drive Mapping for seamless network drive access.  

<h2>What I Learned</h2>

- How to install and set up **Group Policy Management Console (GPMC)**.  
- Difference between **User Configuration** (applies to users) and **Computer Configuration** (applies to systems).  
- Difference between **Policies** (enforced, not changeable by users) and **Preferences** (default settings that users can modify).  
- Practical implementation of GPOs to enforce enterprise security standards.  

---

<h2>Group Policy Objects (GPOs) Implemented</h2>

<h3>1. Password Policy</h3>  

This GPO enforces strong password requirements for all users in the domain to enhance security.  

<b>Steps to Configure Password Policy:</b>  

- <b>Step 1:</b> In **GPMC**, right-click on the domain → **Create GPO in this domain, and Link it here** → Give the policy a meaningful name (e.g., "Domain Password Policy").  

- <b>Step 2:</b> Right-click on the new GPO and select **Edit**.  
  - Here comes the tricky part: deciding whether to apply it under **User Configuration** or **Computer Configuration**.  
  - Since a password policy should apply to **all users regardless of which system they log into**, it is best applied under **Computer Configuration**.  
  - Also, password complexity requirements should be **enforced (Policy)** so that users cannot modify them.  

- <b>Step 3:</b> Navigate to:  
  `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy`  

- <b>Step 4:</b> Configure attributes such as:  
  - Minimum password length  
  - Password complexity (uppercase, lowercase, numbers, special characters)  
  - Maximum password age  
  - Enforce password history
 ![Password Policy Screenshot](/images/password_policy.png)
 ![Password Policy Screenshot](/images/password_policy_complexity_requirments.png) 

- <b>Step 5:</b> Click **Apply** and close the editor. The policy will now be enforced across the domain.  

---

<h3>2. Drive Mapping</h3>  

<b>Introduction</b>  
Drive mapping is an important feature in Active Directory environments that allows administrators to provide users with seamless access to shared resources. Instead of requiring users to remember and manually connect to shared network paths, drive mapping assigns a shared folder on a file server to a specific drive letter (e.g., H: or Z:) on each user’s workstation. This makes accessing shared data as simple as opening a local drive.  

<b>Purpose of Drive Mapping</b>  
The main purpose of drive mapping is to simplify access to organizational files while ensuring central management and security. For example, a department like Finance or HR may have a shared folder stored on a file server. Through drive mapping, all employees in that department automatically see the shared folder as a drive on their computer. This approach ensures consistency, improves collaboration, and enforces the principle of least privilege by granting access only to the correct users or groups.  

<b>Implementation Steps</b>  

- <b>Step 1:</b> In **GPMC**, right-click on the domain → **Create GPO in this domain, and Link it here** → Give the policy a meaningful name (e.g., "Drive Mapping Policy").  

- <b>Step 2:</b> Right-click on the new GPO and select **Edit**. Since drive mappings are specific to users, expand:  
  `User Configuration → Preferences → Windows Settings → Drive Maps`  

- <b>Step 3:</b> Right-click on **Drive Maps** → **New → Mapped Drive**.  

- <b>Step 4:</b> In the dialog box:  
  - Enter the **location of the shared folder** (e.g., `\\FileServer\Finance`) in the “Location” field.  
  - Choose the **Drive Letter** to map (e.g., H:).  
  - Select **Update** action so that changes automatically apply to users.  

- <b>Step 5:</b> Apply and close the policy editor. When the user logs in, the mapped drive will automatically appear on their workstation.
  ![Drive Mapping Screenshot](images/drive_mapping.png)


<b>Reasoning:</b>  
This policy is placed under **User Configuration** and as a **Preference** (not Policy) so users have the flexibility to later modify the mapped drive if needed.

---
<h3>3. Restrict Access to Control Panel</h3>  
- Prevents users from accessing Control Panel and system settings.  
- <b>Configuration Path:</b>  
  `User Configuration → Policies → Administrative Templates → Control Panel → Prohibit access to Control Panel and PC settings`  

---

<h3>4. Prevent USB Storage</h3>  
- Blocks usage of USB storage devices.  
- <b>Configuration Path:</b>  
  `Computer Configuration → Policies → Administrative Templates → System → Removable Storage Access → All Removable Storage classes: Deny all access`  

---

<h3>5. Account Lockout Policy</h3>  
- Locks accounts after a defined number of failed login attempts.  
- <b>Configuration Path:</b>  
  `Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy`  
- <b>Purpose:</b> Critical security control to prevent brute force attacks.  

---

<h2>Result</h2>
This project strengthened my understanding of how **Active Directory Group Policy Management** is used in real IT environments. By implementing password policies, account lockouts, drive mapping, and access restrictions, I learned how enterprises enforce security and usability at scale using centralized GPOs.  
<br />

### Screenshots
  
  
![Restrict Control Panel Screenshot](images/ad/restrict-control-panel.png)  
![USB Restriction Screenshot](images/ad/usb-restriction.png)  
![Account Lockout Policy Screenshot](images/ad/account-lockout.png)  
