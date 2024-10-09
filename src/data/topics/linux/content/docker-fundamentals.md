# 1.2 Docker Fundamentals

## Theoretical Knowledge

### Docker Architecture

Docker uses a client-server architecture. The main components are:

1. Docker Daemon: The background service running on the host that manages building, running, and distributing Docker containers.

2. Docker Client: The command line tool that allows users to interact with the Docker daemon.

3. Docker Images: Read-only templates used to create containers. Images are composed of layers of other images, allowing for minimal data duplication.

4. Docker Containers: Runnable instances of Docker images.

5. Docker Registry: A repository for Docker images. Docker Hub is a public registry that anyone can use.

### Basic Docker Commands

1. `docker pull`: Download an image from a registry.
2. `docker run`: Create and start a container from an image.
3. `docker ps`: List running containers.
4. `docker images`: List available images on the local system.
5. `docker stop`: Stop a running container.
6. `docker rm`: Remove a container.
7. `docker rmi`: Remove an image.

### Creating and Managing Docker Images

Docker images are created using a Dockerfile, which is a text file containing instructions for building the image. Key instructions include:

- `FROM`: Specifies the base image.
- `RUN`: Executes commands in the container.
- `COPY` and `ADD`: Copy files from host to container.
- `EXPOSE`: Informs Docker that the container listens on specific network ports.
- `CMD`: Provides defaults for an executing container.

### Docker Networking

Docker networking allows containers to communicate with each other and the outside world. Key concepts include:

1. Bridge Network: The default network driver. Containers on the same bridge network can communicate.
2. Host Network: Removes network isolation between the container and the Docker host.
3. Overlay Network: Enables communication between containers across multiple Docker daemon hosts.
4. Macvlan Network: Allows assigning a MAC address to a container, making it appear as a physical device on the network.

## Practical Application in Cybersecurity

1. Isolated Environments: Use Docker to create isolated environments for testing security tools or analyzing malware.
2. Consistent Security Configurations: Ensure all instances of an application have the same security settings.
3. Rapid Deployment of Security Tools: Quickly deploy and scale security tools like intrusion detection systems.
4. Forensics and Incident Response: Use containers to recreate environments for investigating security incidents.

## Hands-on Lab Exercise: Building and Running a Custom Docker Image

### Objective
Create a custom Docker image for a simple web application with basic security considerations.

### Prerequisites
- Docker installed on a Linux system (refer to the previous lab)
- Basic understanding of HTML and Python

### Steps

1. Create a new directory for your project:
   ```
   mkdir secure-webapp && cd secure-webapp
   ```

2. Create a simple Python web application (app.py):
   ```python
   from flask import Flask
   app = Flask(__name__)

   @app.route('/')
   def hello():
       return "Hello, Secure World!"

   if __name__ == '__main__':
       app.run(host='0.0.0.0', port=8080)
   ```

3. Create a requirements.txt file:
   ```
   Flask==2.0.1
   ```

4. Create a Dockerfile:
   ```dockerfile
   FROM python:3.9-slim-buster

   # Set working directory
   WORKDIR /app

   # Copy requirements and install dependencies
   COPY requirements.txt .
   RUN pip install --no-cache-dir -r requirements.txt

   # Copy application code
   COPY app.py .

   # Run as non-root user
   RUN useradd -m myuser
   USER myuser

   # Expose port
   EXPOSE 8080

   # Run the application
   CMD ["python", "app.py"]
   ```

5. Build the Docker image:
   ```
   docker build -t secure-webapp:v1 .
   ```

6. Run the container:
   ```
   docker run -d -p 8080:8080 --name mywebapp secure-webapp:v1
   ```

7. Verify the application is running:
   ```
   curl http://localhost:8080
   ```

8. Check the running containers:
   ```
   docker ps
   ```

9. Stop and remove the container:
   ```
   docker stop mywebapp
   docker rm mywebapp
   ```

### Discussion
- How does running the application as a non-root user improve security?
- What other security measures could be implemented in this Dockerfile?
- How might you scan this image for vulnerabilities before deployment?

## Real-world Scenario: Secure Microservices in a Financial Institution

FinTech Inc. is modernizing its banking application using a microservices architecture implemented with Docker containers. They need to ensure the highest level of security due to the sensitive nature of financial data.

Implementation:
1. Each microservice (user authentication, transaction processing, account management) is containerized separately.
2. All images are built from minimal, security-hardened base images.
3. Containers run as non-root users with minimal permissions.
4. All container-to-container communication is encrypted using mutual TLS.
5. Secrets (API keys, database credentials) are managed using Docker secrets, not hardcoded in images.
6. All images are scanned for vulnerabilities before deployment using tools like Clair or Trivy.
7. Container runtime security monitoring is implemented using tools like Falco.

Challenges:
1. Ensuring all developers follow secure Docker practices.
2. Managing the increased complexity of networking between microservices.
3. Monitoring and logging across a distributed containerized environment.
4. Keeping all containers updated with the latest security patches.

## Quiz

1. Which component of Docker architecture is responsible for building, running, and distributing Docker containers?
   a) Docker Client
   b) Docker Daemon
   c) Docker Registry
   d) Docker Image

2. What is the purpose of the `EXPOSE` instruction in a Dockerfile?
   a) It opens a port on the host machine
   b) It allows the container to accept incoming traffic on the specified port
   c) It informs Docker that the container listens on the specified network ports
   d) It creates a firewall rule

3. Which networking mode in Docker removes network isolation between the container and the Docker host?
   a) Bridge Network
   b) Host Network
   c) Overlay Network
   d) Macvlan Network

4. In the context of Docker security, why is it important to run containers as non-root users?
   a) It improves container performance
   b) It allows containers to access host resources more easily
   c) It reduces the potential impact if a container is compromised
   d) It's required for containers to communicate with each other

5. What is the purpose of the `CMD` instruction in a Dockerfile?
   a) It specifies the base image to use
   b) It runs a command during the image build process
   c) It provides defaults for an executing container
   d) It copies files from the host to the container

Answers: 1-b, 2-c, 3-b, 4-c, 5-c

## References and Further Reading

1. Docker Documentation: https://docs.docker.com/
2. "Docker Security" by Adrian Mouat (O'Reilly Media)
3. CIS Docker Benchmark: https://www.cisecurity.org/benchmark/docker
4. Docker Security Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
5. Dockerfile best practices: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

## Glossary Terms

- Dockerfile: A text file that contains instructions for building a Docker image.
- Docker Compose: A tool for defining and running multi-container Docker applications.
- Layer: In Docker, a layer is a change on an image, or an intermediate image. Each instruction in a Dockerfile creates a layer in the image.
- Docker Hub: A cloud-based registry service which allows you to link to code repositories, build your images and test them, stores manually pushed images, and links to Docker Cloud so you can deploy images to your hosts.
- Volume: A persistent data store that can be attached to a container.
- Docker Swarm: A native clustering and orchestration solution for Docker.

