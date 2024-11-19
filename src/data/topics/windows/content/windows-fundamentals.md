# Windows Fundamentals for Cybersecurity

## Glossary of Terms
- **Active Directory (AD)**: Microsoft's directory service for Windows domain networks
- **NTFS**: New Technology File System, Windows' primary file system
- **SID**: Security Identifier, unique identifier for user and group accounts
- **UAC**: User Account Control, security feature preventing unauthorized system changes
- **Windows Registry**: Hierarchical database storing system and application configurations

## 1. Windows Architecture and System Components

### 1.1 Operating System Architecture
- **Kernel Mode vs User Mode**
  - Kernel Mode: Highest privilege level, direct hardware access
  - User Mode: Restricted access, sandboxed environment

#### Lab Exercise: Exploring Windows Architecture
```powershell
# Check system architecture
systeminfo | findstr /C:"System Type"

# List running processes and their privilege levels
tasklist /svc
```

### 1.2 Windows Process Management
- **Process Privileges**
  - Understand SYSTEM, Administrator, and User level processes
  - Identify potential privilege escalation vectors

#### Practical Scenario: Process Security Analysis
```powershell
# List processes with their integrity levels
Get-Process | Where-Object {$_.StartInfo.UserName -ne $null}
```

## 2. Authentication and Access Control

### 2.1 Windows Authentication Mechanisms
- **Local Authentication**
- **Domain Authentication**
- **Windows Hello**
- **Multi-Factor Authentication**

#### Lab Exercise: Password Policy Configuration
```powershell
# View password policy
net accounts

# Set complex password requirements
net accounts /minpwlen:12
net accounts /maxpwage:90
```

### 2.2 Access Control Lists (ACLs)
- Understand NTFS permissions
- File and folder access management
- Principle of least privilege

#### Hands-on Exercise: ACL Management
```powershell
# View file/folder permissions
Get-Acl C:\Users\Public | Format-List

# Modify permissions
icacls "C:\path\to\file" /grant "Username:F"
```

## 3. Windows Security Features

### 3.1 Windows Defender and Built-in Security
- Antivirus capabilities
- Real-time protection
- Threat detection mechanisms

#### Lab Exercise: Defender Configuration
```powershell
# Check Windows Defender status
Get-MpComputerStatus

# Update virus definitions
Update-MpSignature
```

### 3.2 Windows Firewall
- Inbound and outbound traffic control
- Rule configuration
- Network protection strategies

#### Practical Scenario: Firewall Configuration
```powershell
# List current firewall rules
Get-NetFirewallRule

# Create a new firewall rule
New-NetFirewallRule -Name "BlockRDP" -DisplayName "Block RDP" -Action Block -Protocol TCP -LocalPort 3389
```

## 4. Windows Logging and Monitoring

### 4.1 Event Logging
- Security Event Log
- Application and System Logs
- Log analysis techniques

#### Log Analysis Exercise
```powershell
# View security event log
Get-EventLog -LogName Security -Newest 50

# Filter specific security events
Get-WinEvent -FilterHashtable @{LogName='Security'; ID=4624} -MaxEvents 10
```

## 5. Vulnerability Management

### 5.1 Patch Management
- Windows Update mechanisms
- Vulnerability scanning
- Patch deployment strategies

#### Lab Exercise: Update Management
```powershell
# Check for available updates
Install-Module PSWindowsUpdate
Get-WindowsUpdate
```

## Assessment Quiz

1. What is the primary difference between Kernel Mode and User Mode?
   a) Processing speed
   b) Hardware access privileges
   c) Memory allocation

2. Which Windows feature provides user-level protection against unauthorized system changes?
   a) Windows Defender
   b) User Account Control (UAC)
   c) Firewall

3. What does the SID represent in Windows?
   a) System Installation Directory
   b) Security Identifier
   c) Server Identification

## References and Further Reading
- Microsoft Windows Security Documentation
- SANS Windows Security Poster
- NIST Special Publication 800-123: Guide to Reducing Security Vulnerabilities in Federal Systems

## Recommended Tools
- Sysinternals Suite
- NMAP
- Wireshark
- Metasploit Framework

## Learning Objectives
By completing this module, students will:
- Understand Windows system architecture
- Implement security best practices
- Configure access controls
- Perform basic security monitoring
- Recognize and mitigate common Windows vulnerabilities
