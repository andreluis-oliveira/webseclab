# Milestone 3: Linux Advanced Topics

## 1. Shell Scripting

Shell scripting is a powerful tool for automating tasks in Linux. This section will cover advanced concepts in bash scripting.

### Variables and Data Types

```bash
# Declaring variables
name="John"
age=30
readonly CONSTANT="This cannot be changed"

# Arrays
fruits=("apple" "banana" "cherry")
echo ${fruits[1]}  # Outputs: banana

# Associative arrays (requires bash 4.0+)
declare -A user
user[name]="Alice"
user[id]=1001
echo ${user[name]}  # Outputs: Alice
```

### Functions

```bash
function greet() {
    local name=$1
    echo "Hello, $name!"
}

greet "World"  # Outputs: Hello, World!
```

### Control Structures

```bash
# If-else statement
if [ $age -ge 18 ]; then
    echo "Adult"
elif [ $age -ge 13 ]; then
    echo "Teenager"
else
    echo "Child"
fi

# For loop
for fruit in "${fruits[@]}"; do
    echo "I like $fruit"
done

# While loop
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    ((count++))
done
```

### Error Handling

```bash
# Exit on error
set -e

# Custom error function
error_exit() {
    echo "Error: $1" >&2
    exit 1
}

# Usage
command_that_might_fail || error_exit "Command failed"
```

### Advanced Command Substitution and Process Substitution

```bash
# Command substitution
current_date=$(date +%Y-%m-%d)

# Process substitution
diff <(ls dir1) <(ls dir2)
```

## 2. SELinux (Security-Enhanced Linux)

SELinux is a security architecture integrated into the Linux kernel. It provides mandatory access controls (MAC) that restrict the capabilities of processes beyond the standard discretionary access controls (DAC).

### SELinux Modes

- Enforcing: SELinux policy is enforced
- Permissive: SELinux prints warnings but does not enforce policy
- Disabled: SELinux is turned off

Check the current mode:

```bash
getenforce
```

Set the mode (temporarily):

```bash
sudo setenforce 1  # Set to enforcing
sudo setenforce 0  # Set to permissive
```

### SELinux Contexts

SELinux uses contexts to determine access. A context consists of user, role, type, and level.

View file contexts:

```bash
ls -Z /path/to/file
```

Change file context:

```bash
sudo chcon -t httpd_sys_content_t /path/to/file
```

### SELinux Booleans

Booleans are switches that change the behavior of SELinux policy.

List all booleans:

```bash
getsebool -a
```

Set a boolean:

```bash
sudo setsebool httpd_can_network_connect on
```

### Troubleshooting SELinux

View SELinux log:

```bash
sudo ausearch -m AVC -ts recent
```

Generate a troubleshooting report:

```bash
sudo sealert -a /var/log/audit/audit.log
```

## 3. Intrusion Detection Systems (IDS)

An IDS monitors network traffic for suspicious activity and issues alerts when such activity is discovered.

### Types of IDS

1. Network-based IDS (NIDS)
2. Host-based IDS (HIDS)

### Setting up OSSEC (Host-based IDS)

OSSEC is a popular open-source HIDS.

Installation:

```bash
sudo apt-get install ossec-hids
```

Basic configuration (edit `/var/ossec/etc/ossec.conf`):

```xml
<ossec_config>
  <global>
    <email_notification>yes</email_notification>
    <email_to>your_email@example.com</email_to>
    <smtp_server>smtp.example.com</smtp_server>
    <email_from>ossec@example.com</email_from>
  </global>

  <rules>
    <include>rules_config.xml</include>
    <include>pam_rules.xml</include>
    <include>sshd_rules.xml</include>
    <include>telnetd_rules.xml</include>
    <include>syslog_rules.xml</include>
    <include>arpwatch_rules.xml</include>
    <include>symantec-av_rules.xml</include>
    <include>symantec-ws_rules.xml</include>
    <include>pix_rules.xml</include>
    <include>named_rules.xml</include>
    <include>smbd_rules.xml</include>
    <include>vsftpd_rules.xml</include>
    <include>pure-ftpd_rules.xml</include>
    <include>proftpd_rules.xml</include>
    <include>ms_ftpd_rules.xml</include>
    <include>ftpd_rules.xml</include>
    <include>hordeimp_rules.xml</include>
    <include>roundcube_rules.xml</include>
    <include>wordpress_rules.xml</include>
    <include>cimserver_rules.xml</include>
    <include>vpopmail_rules.xml</include>
    <include>vmpop3d_rules.xml</include>
    <include>courier_rules.xml</include>
    <include>web_rules.xml</include>
    <include>web_appsec_rules.xml</include>
    <include>apache_rules.xml</include>
    <include>nginx_rules.xml</include>
    <include>php_rules.xml</include>
    <include>mysql_rules.xml</include>
    <include>postgresql_rules.xml</include>
    <include>ids_rules.xml</include>
    <include>squid_rules.xml</include>
    <include>firewall_rules.xml</include>
    <include>apparmor_rules.xml</include>
    <include>cisco-ios_rules.xml</include>
    <include>netscreenfw_rules.xml</include>
    <include>sonicwall_rules.xml</include>
    <include>postfix_rules.xml</include>
    <include>sendmail_rules.xml</include>
    <include>imapd_rules.xml</include>
    <include>mailscanner_rules.xml</include>
    <include>dovecot_rules.xml</include>
    <include>ms-exchange_rules.xml</include>
    <include>racoon_rules.xml</include>
    <include>vpn_concentrator_rules.xml</include>
    <include>spamd_rules.xml</include>
    <include>msauth_rules.xml</include>
    <include>mcafee_av_rules.xml</include>
    <include>trend-osce_rules.xml</include>
    <include>ms-se_rules.xml</include>
    <!-- <include>policy_rules.xml</include> -->
    <include>zeus_rules.xml</include>
    <include>solaris_bsm_rules.xml</include>
    <include>vmware_rules.xml</include>
    <include>ms_dhcp_rules.xml</include>
    <include>asterisk_rules.xml</include>
    <include>ossec_rules.xml</include>
    <include>attack_rules.xml</include>
    <include>openbsd_rules.xml</include>
    <include>clam_av_rules.xml</include>
    <include>dropbear_rules.xml</include>
    <include>sysmon_rules.xml</include>
    <include>opensmtpd_rules.xml</include>
    <include>exim_rules.xml</include>
    <include>openbsd-dhcpd_rules.xml</include>
    <include>dnsmasq_rules.xml</include>
    <include>local_rules.xml</include>
  </rules>  

  <syscheck>
    <!-- Directories to check (perform integrity checking) -->
    <directories check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
    <directories check_all="yes">/bin,/sbin</directories>

    <!-- Files/directories to ignore -->
    <ignore>/etc/mtab</ignore>
    <ignore>/etc/mnttab</ignore>
    <ignore>/etc/hosts.deny</ignore>
  </syscheck>

  <rootcheck>
    <rootkit_files>/var/ossec/etc/shared/rootkit_files.txt</rootkit_files>
    <rootkit_trojans>/var/ossec/etc/shared/rootkit_trojans.txt</rootkit_trojans>
  </rootcheck>

  <global>
    <white_list>127.0.0.1</white_list>
    <white_list>::1</white_list>
  </global>

  <alerts>
    <log_alert_level>1</log_alert_level>
    <email_alert_level>7</email_alert_level>
  </alerts>

  <command>
    <name>host-deny</name>
    <executable>host-deny.sh</executable>
    <expect>srcip</expect>
    <timeout_allowed>yes</timeout_allowed>
  </command>

  <active-response>
    <command>host-deny</command>
    <location>local</location>
    <level>6</level>
    <timeout>600</timeout>
  </active-response>
</ossec_config>
```

Start OSSEC:

```bash
sudo /var/ossec/bin/ossec-control start
```

### Setting up Snort (Network-based IDS)

Snort is a popular open-source NIDS.

Installation:

```bash
sudo apt-get install snort
```

Basic configuration (edit `/etc/snort/snort.conf`):

```
# Set your home network
ipvar HOME_NET 192.168.1.0/24

# Set external network (usually any)
ipvar EXTERNAL_NET !$HOME_NET

# Path to rules
var RULE_PATH /etc/snort/rules
var SO_RULE_PATH /etc/snort/so_rules
var PREPROC_RULE_PATH /etc/snort/preproc_rules

# Set up the database
output database: log, mysql, user=snort password=<password> dbname=snort host=localhost
```

Start Snort:

```bash
sudo snort -A console -i eth0 -u snort -g snort -c /etc/snort/snort.conf
```

## Conclusion

These advanced Linux topics - shell scripting, SELinux, and intrusion detection systems - provide powerful tools for system administration, security hardening, and threat detection. Practice these concepts in your virtual lab environment to gain hands-on experience and deepen your understanding of Linux security.

## Next Steps
- Explore containerization and container security (Docker, Kubernetes)
- Learn about Linux kernel hardening techniques
- Study advanced network security tools and techniques
- Investigate security information and event management (SIEM) systems

Remember, mastering these advanced topics requires consistent practice and staying updated with the latest security trends and best practices in the Linux ecosystem.
