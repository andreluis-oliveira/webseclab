# CIA Triad

## 1. Detailed Explanations (Theoretical + Practical)

### Theoretical Knowledge

- **CIA Triad**: The foundation of cybersecurity, consisting of **Confidentiality**, **Integrity**, and **Availability**.
  - **Confidentiality**: Ensuring sensitive information is accessible only to authorized individuals (e.g., encryption, access controls).
  - **Integrity**: Ensuring data is accurate and untampered (e.g., checksums, digital signatures).
  - **Availability**: Ensuring systems and data are accessible when needed (e.g., redundancy, backups).

In summary, the CIA Triad is an essential aspect of cybersecurity, providing a clear framework to evaluate and implement security measures. By ensuring confidentiality, integrity, and availability, you create a robust and secure environment for your information and systems.

### Practical Applications

- Examples of the CIA Triad in real-world systems (e.g., banking, healthcare).
- Consequences of breaches:
  - **Confidentiality**: Data leaks.
  - **Integrity**: Data tampering.
  - **Availability**: Service disruptions.

---

## 2. Hands-On Lab Exercises

### Lab 1: Confidentiality

**Task**: Use GPG (GNU Privacy Guard) to encrypt and decrypt a file.  
**Steps**:

1. Install GPG on a Linux VM.
2. Generate a GPG key pair.
3. Encrypt a file using the public key.
4. Decrypt the file using the private key.  
   **Learning Outcome**: Understand how encryption ensures confidentiality.

### Lab 2: Integrity

**Task**: Use SHA-256 hashing to verify file integrity.  
**Steps**:

1. Create a text file and generate its SHA-256 hash.
2. Modify the file and generate the hash again.
3. Compare the two hashes to detect tampering.  
   **Learning Outcome**: Understand how hashing ensures data integrity.

### Lab 3: Availability

**Task**: Set up a basic load balancer using Nginx.  
**Steps**:

1. Install Nginx on a Linux VM.
2. Configure Nginx as a load balancer for two web servers.
3. Simulate a server failure and observe how the load balancer maintains availability.  
   **Learning Outcome**: Understand how redundancy and load balancing ensure availability.

---

## 3. Real-World Scenarios and Case Studies

### Scenario 1: Confidentiality Breach

- **Case Study**: The Equifax Data Breach (2017).
- **Discussion**: Weak encryption and poor access controls led to the exposure of sensitive data.

### Scenario 2: Integrity Breach

- **Case Study**: The Stuxnet Attack (2010).
- **Discussion**: Malware tampered with industrial control systems, compromising data integrity.

### Scenario 3: Availability Breach

- **Case Study**: The Mirai Botnet DDoS Attack (2016).
- **Discussion**: A massive DDoS attack disrupted services by overwhelming servers.

---

## 4. Code Snippets, Configuration Examples, and Command-Line Instructions

### Confidentiality

- **GPG Commands**:

  ```bash
  # Generate a GPG key pair
  gpg --full-generate-key

  # Encrypt a file
  gpg --encrypt --recipient recipient@example.com file.txt

  # Decrypt a file
  gpg --decrypt file.txt.gpg
  ```

---

### 5. Learn more from the following resources:

[The CIA Triad - Professor Messer](https://www.youtube.com/watch?v=SBcDGb9l6yo)
