

Hardware Components:
CPU: Central Processing Unit, the brain of the computer.
RAM: Random Access Memory, temporary storage for data being used.
HD: Hard Drive, permanent storage for data.
Graphic Cards: Hardware for rendering images and video.

Software Components:
Operating System: The main software that manages hardware and software resources.
Applications: Software you use, such as Zen Recorder for recording videos or games.

The Kernel:
The kernel is the core part of an operating system. It acts as a bridge between software and hardware, converting requests into instructions the hardware can understand.

Container Runtimes
There are several runtimes for running containers, including:
Container-D
Docker
CRI-O

# Note: In production environments, Container-D is typically used. Developers often use Docker for local testing. For example, KIND (Kubernetes IN Docker) is used to create Kubernetes clusters in Docker for testing.

Key Differences
Scope:

Docker: Comprehensive solution for building, running, and managing containers.
Containerd: Focused container runtime, providing the essential capabilities to manage containers.
CRI-O: Tailored for Kubernetes, implementing the CRI for managing containers within a Kubernetes cluster.
Usage Context:

Docker: Ideal for local development and deployment across various environments.
Containerd: Often used in production scenarios, especially with Kubernetes, but can also be employed in standalone applications.
CRI-O: Specifically designed for Kubernetes clusters, providing a minimalistic approach.
Conclusion
Docker is great for developers needing an all-in-one solution for container management.
Containerd is suited for production environments looking for a lightweight runtime that can integrate with orchestrators.
CRI-O is the go-to choice for Kubernetes users wanting an optimized, Kubernetes-native container runtime.


# Container vs Virtual machines 

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/980faa67-603b-46d5-bb0b-83d40a22de08)

Virtual Machines (VMs) are like having a complete, separate computer within your computer. Each VM runs its own full operating system, like having another Windows or Linux inside your main system. Because they need to load an entire OS, VMs are big and take a while to start. They are very isolated from each other, making them secure, as each VM operates independently without knowing about the others.

Containers, on the other hand, are like small, self-contained environments inside your computer. They share the main computer’s operating system but have their own space for running applications. Containers are much smaller and start very quickly since they don’t need a full OS. They offer enough isolation to keep applications separate but are less isolated than VMs because they share the same OS.

----

# Docker Architecture 

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/c3b01248-cf68-49d0-86eb-3aea429b986d)

Docker Client
What it is: The Docker client is like the command center for Docker. It’s where you type in commands to tell Docker what to do.
How it works: You use the Docker client to build, run, and manage Docker containers. It talks to the Docker daemon, which does the actual work.
Example commands: docker build, docker run, docker pull.
Docker Hub
What it is: Docker Hub is an online service where you can find and store Docker images.
How it works: It’s like an app store but for Docker images. You can download (“pull”) images others have created, or upload (“push”) your own images.
Usage: When you need an image to create a container, you can pull it from Docker Hub.
Docker Registry
What it is: A Docker registry is a place to store Docker images. Docker Hub is the most popular public registry, but you can also have private registries.
How it works: Registries store the images you create and make them available for you to pull and run on your Docker client. Private registries are used for images you don’t want to share publicly.
Example: Companies often use private registries to store their internal application images securely.

# Installing Docker & Network changes 

curl https://get.docker.com/ | bash 

Netwrok : Bridge - Host - none

Bridge : A Docker bridge network is a private internal network created by Docker on the host machine. It's used to allow containers to communicate with each other within the same host.

![image](https://github.com/saikiranpi/Mastering-Docker/assets/109568252/09f22c6c-743a-480e-8e87-7156d090c65d)



# Listing namespaces & Running basic docker commands 

Basic Docker Commands

Check Docker version: docker version

List namespaces: lsns

List PID namespaces: lsns -t pid

Run a container: docker run --name app1 nginx:latest

Run a container in the background: docker -d run --name app1 nginx:latest

Create multiple containers: for i in {1..10}; do docker run -d nginx:latest; done

List running containers: docker ps

Stop all containers: docker stop $(docker ps -aq)

Deploy and remove containers: docker run --rm -d --name app1 nginx:latest

Inspect a container: docker inspect app1

Port forwarding: docker run --rm -d --name app1 -p 8000:80 nginx:latest

View logs: docker logs app1 -f

#1. Docker Components
A. Docker Engine
Definition: The core component of Docker that allows you to create and manage containers. It consists of three main parts:
Server: The long-running process (daemon) that manages Docker containers.
REST API: Provides a way to interact with the Docker daemon programmatically.
Command-Line Interface (CLI): A user interface for interacting with Docker commands.
B. Docker Daemon (dockerd)
Function: The daemon runs on the host machine and is responsible for managing Docker containers, images, networks, and volumes. It listens for API requests and handles container lifecycle management.
C. Docker Client (docker)
Function: The CLI tool that allows users to communicate with the Docker daemon. Commands issued through the CLI are sent to the Docker daemon via the REST API.
D. Docker Registry
Definition: A storage and distribution system for Docker images. The default public registry is Docker Hub, but private registries can also be used.
Function: Docker clients can pull images from a registry to create containers or push images to a registry to share them.
2. Docker Workflow
Build: Users create a Dockerfile that defines how to build an image. This file includes instructions for installing software, setting up configurations, and defining entry points for the application.

dockerfile
Copy code
# Example Dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
Image Creation: The Docker client communicates with the daemon to build an image based on the Dockerfile.

bash
Copy code
docker build -t myapp:latest .
Image Distribution: Once the image is built, it can be pushed to a Docker registry.

bash
Copy code
docker push myapp:latest
Container Creation: Users can pull the image from the registry and create containers from it.

bash
Copy code
docker run -d -p 80:80 myapp:latest
Management: The Docker daemon manages the lifecycle of the containers (start, stop, restart, remove).

3. Docker Networking
Docker provides several networking modes for containers to communicate:

Bridge Network: Default network that allows containers to communicate with each other on the same host.
Host Network: Containers share the host’s network stack.
Overlay Network: Used in Docker Swarm for communication between containers on different hosts.
Macvlan Network: Allows containers to have their own MAC addresses, making them appear as physical devices on the network.
4. Docker Storage
Docker uses different storage options to manage persistent data:

Volumes: Managed by Docker, they exist outside the container lifecycle and are stored in a part of the host filesystem that’s managed by Docker.
Bind Mounts: Allow you to specify a path on the host to be mounted into the container. This provides direct access to files and directories on the host.
tmpfs Mounts: Store data in memory and are used for sensitive information that shouldn’t be persisted.
5. Docker Compose
Definition: A tool for defining and running multi-container Docker applications using a YAML file (docker-compose.yml).
Function: Allows users to define services, networks, and volumes in a single file, making it easier to manage complex applications.
6. Docker Swarm
Definition: Docker’s native clustering and orchestration tool.
Function: Enables the management of a cluster of Docker nodes as a single virtual system, allowing for load balancing, service discovery, and scaling of applications.
7. Security
Namespaces: Isolate containers from each other and from the host system.
Control Groups (cgroups): Manage and limit resource usage (CPU, memory, I/O) for containers.
Docker Security Options: Includes user namespaces, capabilities, and seccomp profiles to enhance container security.
Summary
Docker architecture is modular and designed to provide a comprehensive container management solution. It simplifies application deployment and scaling while ensuring efficient resource usage and isolation. Understanding this architecture is crucial for effectively utilizing Docker in development and production environments.
   
