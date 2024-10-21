# 3. Advanced Network Security Tools and Techniques

## 3.1 Network Intrusion Detection/Prevention Systems (NIDS/NIPS)

### Theoretical Knowledge

Network Intrusion Detection/Prevention Systems (NIDS/NIPS) are crucial components of modern network security infrastructure. They monitor network traffic for suspicious activity and policy violations, alerting administrators or taking automatic action when threats are detected.

- **NIDS (Network Intrusion Detection System)**: Passively monitors network traffic, detecting and logging suspicious activities.
- **NIPS (Network Intrusion Prevention System)**: Actively monitors and can automatically block or prevent detected intrusions.

Key concepts:
- Signature-based detection
- Anomaly-based detection
- Protocol analysis
- Traffic normalization

### Practical Applications in Cybersecurity

- Real-time threat detection and prevention
- Network traffic analysis and forensics
- Compliance monitoring (e.g., PCI DSS, HIPAA)
- Zero-day attack mitigation

### 3.1.1 Snort: Installation, Configuration, and Rule Writing

Snort is an open-source, lightweight NIDS/NIPS that uses a rule-driven language to perform packet logging and traffic analysis.

#### Installation

Lab Exercise: Installing Snort on Ubuntu 20.04 LTS

1. Update your system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Install Snort dependencies:
   ```bash
   sudo apt install -y build-essential libpcap-dev libpcre3-dev libnet1-dev zlib1g-dev luajit hwloc libdnet-dev libdumbnet-dev bison flex liblzma-dev openssl libssl-dev pkg-config libhwloc-dev cmake cpputest libsqlite3-dev uuid-dev libcmocka-dev libnetfilter-queue-dev libmnl-dev autotools-dev libluajit-5.1-dev libunwind-dev
   ```

3. Download and install Snort:
   ```bash
   wget https://www.snort.org/downloads/snort/snort-2.9.17.tar.gz
   tar xvzf snort-2.9.17.tar.gz
   cd snort-2.9.17
   ./configure --enable-sourcefire && make && sudo make install
   ```

4. Update shared libraries:
   ```bash
   sudo ldconfig
   ```

5. Verify installation:
   ```bash
   snort --version
   ```

#### Configuration

1. Create Snort directories:
   ```bash
   sudo mkdir /etc/snort
   sudo mkdir /etc/snort/rules
   sudo mkdir /var/log/snort
   sudo chmod -R 5775 /etc/snort
   sudo chmod -R 5775 /var/log/snort
   ```

2. Copy configuration files:
   ```bash
   sudo cp etc/*.conf* /etc/snort
   sudo cp etc/*.map /etc/snort
   ```

3. Edit snort.conf:
   ```bash
   sudo nano /etc/snort/snort.conf
   ```
   Update HOME_NET and EXTERNAL_NET variables to match your network.

#### Rule Writing

Snort rules follow this general structure:
```
action protocol source_ip source_port direction destination_ip destination_port (rule options)
```

Example rule to detect ICMP ping:
```
alert icmp any any -> $HOME_NET any (msg:"ICMP Ping Detected"; sid:1000001; rev:1;)
```

Lab Exercise: Creating and Testing a Custom Snort Rule

1. Create a custom rules file:
   ```bash
   sudo nano /etc/snort/rules/local.rules
   ```

2. Add the following rule:
   ```
   alert tcp any any -> $HOME_NET 22 (msg:"Potential SSH Brute Force Attack"; flow:to_server,established; threshold: type threshold, track by_src, count 5, seconds 60; sid:1000002; rev:1;)
   ```

3. Update snort.conf to include your local rules:
   ```bash
   sudo nano /etc/snort/snort.conf
   ```
   Add: `include $RULE_PATH/local.rules`

4. Test the configuration:
   ```bash
   sudo snort -T -c /etc/snort/snort.conf
   ```

5. Run Snort in NIDS mode:
   ```bash
   sudo snort -A console -q -u snort -g snort -c /etc/snort/snort.conf -i eth0
   ```

### 3.1.2 Suricata: Advanced Features and Performance Optimization

Suricata is a high-performance, open-source NIDS/NIPS that supports multi-threading and hardware acceleration.

#### Installation

Lab Exercise: Installing Suricata on Ubuntu 20.04 LTS

1. Install Suricata from the official repository:
   ```bash
   sudo add-apt-repository ppa:oisf/suricata-stable
   sudo apt update
   sudo apt install suricata
   ```

2. Verify installation:
   ```bash
   suricata --version
   ```

#### Configuration and Optimization

1. Edit Suricata configuration:
   ```bash
   sudo nano /etc/suricata/suricata.yaml
   ```

2. Optimize thread usage:
   ```yaml
   max-pending-packets: 1024
   runmode: workers
   detect-thread-ratio: 1.5
   ```

3. Enable hardware acceleration (if supported):
   ```yaml
   af-packet:
     - interface: eth0
       cluster-id: 99
       cluster-type: cluster_flow
       defrag: yes
       use-mmap: yes
       mmap-locked: yes
       tpacket-v3: yes
   ```

4. Update rule sources:
   ```yaml
   default-rule-path: /var/lib/suricata/rules
   rule-files:
     - suricata.rules
   ```

Lab Exercise: Performance Testing Suricata

1. Generate test traffic:
   ```bash
   sudo apt install iperf
   iperf -s  # On the server
   iperf -c <server_ip>  # On the client
   ```

2. Monitor Suricata performance:
   ```bash
   sudo suricata -c /etc/suricata/suricata.yaml -i eth0 --engine-analysis
   ```

3. Analyze the results and adjust configuration as needed.

### Real-world Scenario: Detecting and Preventing a DDoS Attack

A mid-sized e-commerce company experienced a sudden surge in traffic, causing their website to become unresponsive. Upon investigation, it was discovered that they were under a Distributed Denial of Service (DDoS) attack.

Solution:
1. Implement Suricata as a NIPS at the network edge.
2. Configure Suricata with rules to detect common DDoS patterns.
3. Set up automatic blocking of IP addresses generating suspicious traffic.
4. Implement rate limiting on the web server to mitigate application-layer DDoS attacks.

Result: The company successfully mitigated the DDoS attack, reducing downtime and financial losses.

### Quiz

1. What is the main difference between NIDS and NIPS?
   a) NIDS is software-based, NIPS is hardware-based
   b) NIDS only detects threats, NIPS can also prevent them
   c) NIDS works at the application layer, NIPS at the network layer
   d) NIDS is open-source, NIPS is proprietary

2. Which of the following is NOT a detection method used by NIDS/NIPS?
   a) Signature-based detection
   b) Anomaly-based detection
   c) Frequency-based detection
   d) Protocol analysis

3. In a Snort rule, what does the following represent: "alert tcp any any -> $HOME_NET 80"?
   a) Alert on any TCP traffic from the home network to any destination on port 80
   b) Alert on any TCP traffic from any source to the home network on port 80
   c) Alert on any TCP traffic from the home network to any destination on any port
   d) Alert on any UDP traffic from any source to the home network on port 80

4. Which feature of Suricata allows it to process network traffic more efficiently on multi-core systems?
   a) Multi-threading
   b) Rule compression
   c) Hardware offloading
   d) Packet normalization

5. What is the purpose of the "threshold" option in NIDS/NIPS rules?
   a) To set the minimum packet size for inspection
   b) To limit the number of times a rule can match within a specified time period
   c) To define the sensitivity of anomaly detection
   d) To set the maximum number of concurrent connections

Answers: 1-b, 2-c, 3-b, 4-a, 5-b

### References and Further Reading

1. [Snort Documentation](https://www.snort.org/documents)
2. [Suricata User Guide](https://suricata.readthedocs.io/)
3. [NIST Guide to Intrusion Detection and Prevention Systems](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-94r1.pdf)
4. [Emerging Threats Open Ruleset](https://rules.emergingthreats.net/)

### Glossary

- **NIDS**: Network Intrusion Detection System
- **NIPS**: Network Intrusion Prevention System
- **Signature-based detection**: Identifying threats by matching patterns against a database of known attack signatures
- **Anomaly-based detection**: Identifying threats by detecting deviations from normal network behavior
- **False positive**: An alert triggered by legitimate traffic mistakenly identified as malicious
- **False negative**: A failure to detect actual malicious activity
- **Rule**: A set of criteria used to identify specific types of network traffic
- **Packet capture**: The process of intercepting and logging network traffic
- **DDoS**: Distributed Denial of Service, an attack aimed at overwhelming a system's resources
- **Inline mode**: A deployment method where the NIDS/NIPS is placed directly in the path of network traffic, allowing for active prevention

