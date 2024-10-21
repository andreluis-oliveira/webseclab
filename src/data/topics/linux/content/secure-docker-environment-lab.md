# Lab Exercise: Setting up a Secure Docker Environment

## Objective
In this lab, you will set up a secure Docker environment on a Linux system. You'll install Docker, create a custom Docker image with security considerations, implement Docker Content Trust, and use Trivy for image vulnerability scanning.

## Prerequisites
- A Linux system (Ubuntu 20.04 LTS recommended)
- Sudo privileges on the system
- Basic familiarity with the command line

## Estimated Time
2-3 hours

## Steps

### 1. Install Docker on a Linux System

a. Update your system packages:
```bash
sudo apt update
sudo apt upgrade -y
```

b. Install Docker dependencies:
```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

c. Add Docker's official GPG key:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

d. Add the Docker repository:
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

e. Install Docker:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

f. Verify the installation:
```bash
sudo docker run hello-world
```

### 2. Create a Custom Docker Image with Security Considerations

a. Create a directory for your Docker project:
```bash
mkdir ~/secure-docker-app && cd ~/secure-docker-app
```

b. Create a simple Python application (app.py):
```bash
echo 'print("Hello, Secure Docker!")' > app.py
```

c. Create a Dockerfile with security considerations:
```bash
cat << EOF > Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim-buster

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
# RUN pip install --no-cache-dir -r requirements.txt

# Run app.py when the container launches
CMD ["python", "app.py"]

# Run the container as a non-root user
RUN useradd -m myuser
USER myuser

# Set Python to run in unbuffered mode
ENV PYTHONUNBUFFERED=1

# Metadata as defined in OCI image spec annotations
# https://github.com/opencontainers/image-spec/blob/master/annotations.md
LABEL org.opencontainers.image.title="Secure Docker App"
LABEL org.opencontainers.image.description="A simple, secure Docker application"
LABEL org.opencontainers.image.version="1.0.0"
LABEL org.opencontainers.image.vendor="Your Company Name"
EOF
```

d. Build the Docker image:
```bash
sudo docker build -t secure-app:v1 .
```

### 3. Implement Docker Content Trust

a. Enable Docker Content Trust:
```bash
export DOCKER_CONTENT_TRUST=1
```

b. Create a Docker Hub account if you don't have one, and log in:
```bash
sudo docker login
```

c. Tag your image for Docker Hub:
```bash
sudo docker tag secure-app:v1 yourusername/secure-app:v1
```

d. Push the image to Docker Hub (this will automatically sign the image):
```bash
sudo docker push yourusername/secure-app:v1
```

### 4. Use Trivy for Image Vulnerability Scanning

a. Install Trivy:
```bash
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

b. Scan your Docker image:
```bash
trivy image secure-app:v1
```

c. Review the scan results and address any vulnerabilities found.

### 5. Verify Your Secure Docker Environment

a. Run your secure container:
```bash
sudo docker run --rm secure-app:v1
```

b. Verify that the container is running as a non-root user:
```bash
sudo docker run --rm secure-app:v1 id
```

c. Pull your signed image from Docker Hub:
```bash
sudo docker pull yourusername/secure-app:v1
```

## Conclusion
In this lab, you've set up a secure Docker environment by installing Docker, creating a custom image with security considerations, implementing Docker Content Trust, and using Trivy for vulnerability scanning. These practices help ensure the integrity and security of your Docker images and containers.

## Further Exercises
1. Implement additional security measures in your Dockerfile (e.g., multi-stage builds, minimal base images).
2. Set up a local Docker registry with TLS encryption.
3. Create and apply custom seccomp profiles to your containers.

## Quiz: Test Your Understanding

After completing the lab, test your knowledge with this quiz:

1. What command is used to verify the Docker installation?
   a) docker --version
   b) docker run hello-world
   c) docker status
   d) docker verify

2. Why is it important to run a Docker container as a non-root user?
   a) It improves container performance
   b) It reduces the potential impact of a container breach
   c) It allows the container to access more system resources
   d) It enables container networking

3. What does the DOCKER_CONTENT_TRUST environment variable do when set to 1?
   a) It enables automatic container updates
   b) It enforces signature verification on Docker images
   c) It increases Docker daemon security
   d) It optimizes Docker image builds

4. Which of the following is NOT a security consideration in our custom Dockerfile?
   a) Using a slim base image
   b) Running the container as a non-root user
   c) Setting Python to run in unbuffered mode
   d) Installing all available system updates

5. What is the primary purpose of using Trivy in this lab?
   a) To build Docker images
   b) To scan Docker images for vulnerabilities
   c) To optimize Docker image size
   d) To manage Docker networks

6. In the context of Docker Content Trust, what does image signing accomplish?
   a) It compresses the image for faster downloads
   b) It encrypts the image contents
   c) It verifies the integrity and publisher of the image
   d) It improves image layer caching

7. Why do we add LABEL instructions in our Dockerfile?
   a) To improve container performance
   b) To add metadata to the image
   c) To set environment variables
   d) To define build arguments

8. What is the significance of using a specific version tag (e.g., python:3.9-slim-buster) instead of 'latest' in the FROM instruction?
   a) It ensures reproducible builds
   b) It automatically updates to the newest version
   c) It reduces image size
   d) It improves security by using the most recent patches

9. What does the --no-cache-dir flag do when used with pip install?
   a) It installs packages without checking for updates
   b) It prevents caching of downloaded packages, reducing image size
   c) It caches packages for faster future installations
   d) It installs packages in verbose mode

10. Which of the following is a benefit of using multi-stage builds in Docker?
    a) It automatically scales containers
    b) It provides real-time vulnerability scanning
    c) It can significantly reduce the final image size
    d) It enables cross-platform builds

Answers: 1-b, 2-b, 3-b, 4-d, 5-b, 6-c, 7-b, 8-a, 9-b, 10-c

## Official Documentation and Further Reading

For students who want to dive deeper into the concepts and tools covered in this lab, here are links to official documentation and additional resources:

1. Docker
   - [Docker Documentation](https://docs.docker.com/)
   - [Docker Security](https://docs.docker.com/engine/security/)
   - [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

2. Docker Content Trust
   - [Content Trust in Docker](https://docs.docker.com/engine/security/trust/)

3. Trivy
   - [Trivy Documentation](https://aquasecurity.github.io/trivy/)
   - [Trivy GitHub Repository](https://github.com/aquasecurity/trivy)

4. Container Security
   - [NIST Application Container Security Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-190.pdf)
   - [CIS Docker Benchmark](https://www.cisecurity.org/benchmark/docker)

5. Python Docker Images
   - [Official Python Docker Images](https://hub.docker.com/_/python)

6. Linux Security Modules
   - [AppArmor Documentation](https://gitlab.com/apparmor/apparmor/-/wikis/Documentation)
   - [SELinux Project](https://github.com/SELinuxProject)

7. OWASP Docker Security
   - [OWASP Docker Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)

These resources will provide students with a deeper understanding of Docker security concepts and best practices, as well as detailed information about the tools used in this lab.
