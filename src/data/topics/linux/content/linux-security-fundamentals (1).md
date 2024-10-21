# Linux Security Fundamentals

## Introduction

This guide covers essential Linux security concepts and practices. It's designed for beginners and intermediate users looking to enhance their understanding of Linux system security.

## 1. User Account Management

### Creating Strong Passwords
- Use a mix of uppercase, lowercase, numbers, and special characters
- Aim for at least 12 characters in length
- Avoid using personal information or common words

### Implementing Password Policies
Set password complexity requirements:

```bash
sudo vi /etc/security/pwquality.conf
```

Enforce password expiration:

```bash
sudo chage -M 90 username  # Set maximum password age to 90 days
```

### User and Group Management
Create a new user:

```bash
sudo useradd -m username
sudo passwd username
```

Add a user to a group:

```bash
sudo usermod -aG groupname username
```

## 2. File Permissions

### Understanding Permission Basics
- Read (r), Write (w), Execute (x)
- User (u), Group (g), Others (o)

View file permissions:

```bash
ls -l filename
```

### Changing Permissions
Using chmod with symbolic notation:

```bash
chmod u+x filename  # Add execute permission for the owner
```

Using chmod with octal notation:

```bash
chmod 644 filename  # Set rw-r--r-- permissions
```

### Setting Default Permissions
Use umask to set default permissions for new files and directories:

```bash
umask 022  # Results in 755 for directories and 644 for files
```

## 3. Secure Shell (SSH)

### Configuring SSH
Edit the SSH configuration file:

```bash
sudo vi /etc/ssh/sshd_config
```

Key settings to consider:
- Disable root login: `PermitRootLogin no`
- Use SSH key authentication: `PasswordAuthentication no`
- Change default port: `Port 2222`

### Using SSH Keys
Generate an SSH key pair:

```bash
ssh-keygen -t rsa -b 4096
```

Copy the public key to a remote server:

```bash
ssh-copy-id user@remote_host
```

## 4. Firewall Configuration

### Using UFW (Uncomplicated Firewall)
Enable UFW:

```bash
sudo ufw enable
```

Allow specific ports:

```bash
sudo ufw allow 22/tcp  # Allow SSH
sudo ufw allow 80/tcp  # Allow HTTP
```

### Basic iptables Rules
List current rules:

```bash
sudo iptables -L
```

Add a basic rule to accept established connections:

```bash
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

## 5. System Updates and Patch Management

### Updating the System
For Debian-based systems:

```bash
sudo apt update
sudo apt upgrade
```

For Red Hat-based systems:

```bash
sudo yum update
```

### Enabling Automatic Security Updates
On Ubuntu, install unattended-upgrades:

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

## 6. Logging and Auditing

### Viewing System Logs
Check system logs:

```bash
sudo journalctl
```

View authentication attempts:

```bash
sudo cat /var/log/auth.log
```

### Using Auditd
Install and start auditd:

```bash
sudo apt install auditd
sudo systemctl start auditd
```

Add an audit rule:

```bash
sudo auditctl -w /etc/passwd -p wa -k passwd_changes
```

## 7. Encryption

### Using LUKS for Full Disk Encryption
Create an encrypted partition:

```bash
sudo cryptsetup luksFormat /dev/sdb1
sudo cryptsetup luksOpen /dev/sdb1 secure_data
sudo mkfs.ext4 /dev/mapper/secure_data
```

### Implementing File-level Encryption
Use GPG to encrypt a file:

```bash
gpg -c filename
```

## Conclusion

These Linux security fundamentals form a solid foundation for securing your systems. Remember that security is an ongoing process, requiring constant vigilance and updates to stay ahead of potential threats.

## Next Steps
- Learn about intrusion detection systems (IDS) like Snort or OSSEC
- Explore Security-Enhanced Linux (SELinux) or AppArmor
- Study common attack vectors and how to mitigate them
- Practice in your virtual lab environment to gain hands-on experience

As you progress, always stay informed about the latest security threats and best practices in the Linux ecosystem.
