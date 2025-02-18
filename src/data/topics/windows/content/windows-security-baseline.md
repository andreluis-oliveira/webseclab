# Windows Security Baselines

## 1. Theoretical Explanation

**Security baselines** are predefined sets of security settings that provide a starting point for securing a system. They are designed to help organizations apply consistent security configurations across multiple systems, reducing vulnerabilities and ensuring compliance with industry standards or regulatory requirements.

In the context of **Windows**, Microsoft provides **Security Baseline** templates that can be applied to various versions of Windows (e.g., Windows 10, Windows Server). These baselines include recommended settings for user rights, password policies, audit policies, and more. By applying these baselines, administrators can ensure that their systems are configured securely out-of-the-box.

### Why Use Security Baselines?
- **Consistency**: Ensures that all systems in an organization have the same security settings.
- **Compliance**: Helps meet regulatory requirements such as GDPR, HIPAA, or PCI-DSS.
- **Reduced Attack Surface**: Minimizes unnecessary services and features that could be exploited by attackers.
- **Ease of Deployment**: Simplifies the process of securing multiple systems by providing pre-configured templates.

---

## 2. Key Concepts or Components

- **Group Policy Objects (GPOs)**: GPOs are used to configure and enforce security settings across multiple machines in a Windows domain. Security baselines are often implemented via GPOs.
  
- **Local Security Policy**: For standalone systems, you can configure security settings locally using the **Local Security Policy** editor (`secpol.msc`).

- **Security Compliance Toolkit (SCT)**: Microsoft provides the **Security Compliance Toolkit**, which includes pre-configured security baselines for various versions of Windows. These baselines can be imported into Group Policy or applied manually.

- **Common Security Settings**:
  - **Password Policies**: Enforce strong passwords, account lockout thresholds, and password expiration.
  - **User Rights Assignment**: Control who can log on locally, access the system remotely, or perform administrative tasks.
  - **Audit Policies**: Enable auditing of specific events (e.g., logon attempts, file access) to monitor suspicious activity.
  - **Windows Defender and Antivirus**: Ensure that antivirus software is enabled and up-to-date.
  - **Firewall Configuration**: Ensure that the Windows Firewall is enabled and properly configured.

---

## 3. Practical Example: Hands-On Lab Exercise

### Objective:
Apply a **Windows Security Baseline** to a Windows 10 virtual machine using the **Local Security Policy** editor.

### Tools Needed:
- A Windows 10 virtual machine (VM) running in VirtualBox or any other hypervisor.
- Basic knowledge of navigating Windows settings.

### Step-by-Step Instructions:

#### Step 1: Open the Local Security Policy Editor
1. Press `Win + R` to open the **Run** dialog box.
2. Type `secpol.msc` and press **Enter**. This will open the **Local Security Policy** editor.

#### Step 2: Configure Password Policies
1. In the left-hand pane, navigate to **Account Policies > Password Policy**.
2. Double-click on **Minimum password length** and set it to **8 characters**.
3. Double-click on **Password must meet complexity requirements** and set it to **Enabled**.
4. Double-click on **Maximum password age** and set it to **90 days**.

#### Step 3: Configure Account Lockout Policies
1. Navigate to **Account Policies > Account Lockout Policy**.
2. Double-click on **Account lockout threshold** and set it to **5 invalid login attempts**.
3. Double-click on **Reset account lockout counter after** and set it to **30 minutes**.

#### Step 4: Enable Auditing
1. Navigate to **Advanced Audit Policy Configuration > Object Access**.
2. Double-click on **Audit File System** and enable both **Success** and **Failure** auditing.
3. Apply the changes.

#### Step 5: Verify Changes
1. Open a Command Prompt and type `gpupdate /force` to apply the new security settings.
2. Log out and log back in to test the new password policy.
3. Attempt to log in with incorrect credentials multiple times to trigger the account lockout policy.

---

## 4. Discussion Questions

1. **Why is it important to enforce password complexity and length requirements?**
   - How does this reduce the risk of brute-force attacks?

2. **What are the potential downsides of setting a very low account lockout threshold (e.g., 3 failed attempts)?**
   - How could this be exploited by attackers to cause a denial of service?

3. **How does auditing help in detecting and responding to security incidents?**
   - What are some common events that should be audited in a production environment?

4. **How would you apply these security baselines in a large enterprise environment with hundreds of machines?**
   - What tools or technologies would you use to manage security settings across multiple systems?

---

## 5. Quiz or Small Challenge

### Quiz:
1. **What is the purpose of a security baseline?**
   - A) To provide a starting point for securing a system
   - B) To monitor network traffic
   - C) To encrypt data
   - D) To detect malware

2. **Which tool is used to apply security settings locally on a Windows machine?**
   - A) Task Manager
   - B) Local Security Policy Editor
   - C) Event Viewer
   - D) Windows Defender

3. **What is the recommended minimum password length according to most security baselines?**
   - A) 6 characters
   - B) 8 characters
   - C) 10 characters
   - D) 12 characters

### Challenge:
- **Challenge:** Modify the **Local Security Policy** to disable the Guest account and restrict access to the command prompt for non-administrative users. Document the steps you took and explain why these changes improve security.

---

## 6. Additional Resources

- **Microsoft Security Baselines Documentation**: [Microsoft Security Compliance Toolkit](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-compliance-toolkit-10)
- **Group Policy Overview**: [Group Policy Overview](https://docs.microsoft.com/en-us/windows-server/identity/group-policy/group-policy-overview)
- **NIST Cybersecurity Framework**: [NIST CSF](https://www.nist.gov/cyberframework)
- **OWASP Windows Hardening Guide**: [OWASP Windows Hardening](https://owasp.org/www-project-windows-hardening/)