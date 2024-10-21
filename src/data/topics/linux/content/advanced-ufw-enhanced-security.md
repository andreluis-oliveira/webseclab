# Advanced UFW Security Enhancement Module

## Lab 5: Integrating UFW with Fail2ban

### Theoretical Overview
Fail2ban is an intrusion prevention software that protects systems from brute-force attacks. When integrated with UFW, it provides dynamic protection by automatically blocking IP addresses that show malicious behavior.

### Installation and Configuration

1. Install Fail2ban:
```bash
sudo apt update
sudo apt install fail2ban
```

2. Create a custom jail configuration:
```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

3. Configure the SSH jail:
```ini
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
findtime = 300
bantime = 3600
```

4. Create a custom action for UFW:
```bash
sudo nano /etc/fail2ban/action.d/ufw.conf
```

Add the following content:
```ini
[Definition]
actionstart = 
actionstop = 
actioncheck = 
actionban = ufw insert 1 deny from <ip> to any
actionunban = ufw delete deny from <ip> to any

[Init]
```

5. Update the jail to use UFW:
```bash
sudo nano /etc/fail2ban/jail.local
```

Add to the [sshd] section:
```ini
banaction = ufw
```

6. Restart services:
```bash
sudo systemctl restart fail2ban
sudo ufw reload
```

### Testing the Integration

1. Monitor Fail2ban status:
```bash
sudo fail2ban-client status
```

2. Simulate failed login attempts (from another machine):
```bash
ssh nonexistent_user@target_ip
```

3. Check if the IP was banned:
```bash
sudo fail2ban-client status sshd
sudo ufw status numbered
```

## Troubleshooting Common UFW Issues

### Issue 1: Rules Not Taking Effect

Symptoms:
- Traffic still passes despite deny rules
- New rules don't seem to work

Solution Steps:
1. Check rule order:
```bash
sudo ufw status numbered
```

2. Reset UFW if necessary:
```bash
sudo ufw reset
```

3. Verify rule syntax:
```bash
sudo ufw show added
```

### Issue 2: Service Access Problems

Symptoms:
- Services cannot be accessed despite allowing ports
- Inconsistent access to services

Solution Steps:
1. Check listening ports:
```bash
sudo netstat -tulpn
```

2. Verify service status:
```bash
sudo systemctl status service_name
```

3. Test with telnet:
```bash
telnet localhost port_number
```

### Issue 3: UFW Logging Issues

Symptoms:
- No log entries
- Incomplete logs

Solution Steps:
1. Enable logging:
```bash
sudo ufw logging on
sudo ufw logging medium
```

2. Verify rsyslog configuration:
```bash
sudo nano /etc/rsyslog.d/20-ufw.conf
```

3. Restart logging:
```bash
sudo systemctl restart rsyslog
```

## Project: Complete Network Security Solution

### Objective
Set up a segmented network with DMZ using UFW for a small business environment.

### Network Architecture
```
Internet
   ↓
[Router]
   ↓
   ├─── DMZ (192.168.1.0/24)
   │    ├─── Web Server (192.168.1.10)
   │    └─── Mail Server (192.168.1.20)
   │
   ├─── Internal Network (192.168.2.0/24)
   │    ├─── File Server (192.168.2.10)
   │    └─── WorkStations (192.168.2.100-200)
   │
   └─── Management Network (192.168.3.0/24)
        └─── Admin Workstation (192.168.3.10)
```

### Implementation Steps

1. Set up network interfaces:
```bash
sudo nano /etc/network/interfaces
```

Add:
```
auto eth0
iface eth0 inet static
    address 192.168.1.1
    netmask 255.255.255.0

auto eth1
iface eth1 inet static
    address 192.168.2.1
    netmask 255.255.255.0

auto eth2
iface eth2 inet static
    address 192.168.3.1
    netmask 255.255.255.0
```

2. Configure base UFW rules:
```bash
sudo ufw reset
sudo ufw default deny incoming
sudo ufw default deny outgoing
```

3. Set up DMZ rules:
```bash
# Allow web traffic to DMZ
sudo ufw allow in on eth0 to 192.168.1.10 port 80,443
sudo ufw allow in on eth0 to 192.168.1.20 port 25,587,993
```

4. Configure internal network access:
```bash
# Allow internal network access to DMZ services
sudo ufw allow in on eth1 to 192.168.1.0/24
# Allow internal network access to file server
sudo ufw allow in on eth1 to 192.168.2.10 port 445
```

5. Set up management network rules:
```bash
# Allow admin access to all networks
sudo ufw allow in on eth2 to any
# Allow SSH only from management network
sudo ufw allow in on eth2 to any port 22
```

6. Configure logging and monitoring:
```bash
sudo ufw logging on
sudo ufw logging high
```

### Verification Tests

1. Test DMZ access:
```bash
# From external network
curl http://192.168.1.10
telnet 192.168.1.20 25
```

2. Test internal network isolation:
```bash
# From DMZ server
ping 192.168.2.10  # Should fail
# From internal network
ping 192.168.1.10  # Should succeed
```

3. Test management access:
```bash
# From management network
ssh admin@192.168.1.10
ssh admin@192.168.2.10
```

## Quiz: Enhanced Security Features

1. What is the primary purpose of integrating Fail2ban with UFW?
   a) To improve firewall performance
   b) To automatically block malicious IP addresses
   c) To monitor network traffic
   d) To encrypt network communications

2. Which file contains the custom jail configurations for Fail2ban?
   a) /etc/fail2ban/jail.conf
   b) /etc/fail2ban/jail.local
   c) /etc/fail2ban/ufw.conf
   d) /etc/fail2ban/config

3. In a DMZ setup, which network segment typically hosts public-facing services?
   a) Internal network
   b) Management network
   c) DMZ
   d) Backup network

4. What is the recommended UFW logging level for security auditing?
   a) low
   b) medium
   c) high
   d) full

5. Which command would you use to view the numeric order of UFW rules?
   a) sudo ufw show
   b) sudo ufw list
   c) sudo ufw status
   d) sudo ufw status numbered

Answers: 1-b, 2-b, 3-c, 4-c, 5-d

## Additional References

1. [Fail2ban Official Documentation](https://www.fail2ban.org/wiki/index.php/Main_Page)
2. [Network Segmentation Best Practices](https://www.nist.gov/publications/guide-industrial-control-systems-ics-security)
3. [DMZ Implementation Guide](https://csrc.nist.gov/publications/detail/sp/800-41/rev-1/final)

