

<img width="960" alt="04" src="https://github.com/saikiranpi/Mastering-Docker/assets/109568252/a002e620-a69a-4565-8086-33bd1ac0351f">


# Dockerfile Setup for Terraform and Packer

## Overview

This repository contains a Dockerfile designed to create an environment with **Terraform** and **Packer** using an Ubuntu base image. This setup is essential for DevOps practices, enabling automation for cloud provisioning and application deployment.

### Table of Contents
- [Dockerfile Overview](#dockerfile-overview)
- [Dockerfile Contents](#dockerfile-contents)
- [Building the Docker Image](#building-the-docker-image)
- [Running the Container](#running-the-container)
- [Setting Environment Variables](#setting-environment-variables)
- [Useful Docker Commands](#useful-docker-commands)
- [Dockerfile Instructions Explained](#dockerfile-instructions-explained)

---

## Dockerfile Overview

The Dockerfile performs the following key tasks:

1. **Sets up an Ubuntu base image**: Utilizes the latest version of Ubuntu for a stable environment.
2. **Installs necessary tools**: Installs essential utilities like `jq`, `net-tools`, `curl`, `wget`, `unzip`, `nginx`, and `iputils-ping`.
3. **Downloads and installs specific versions of Terraform and Packer**: This ensures consistency and reliability in your infrastructure as code (IaC) practices.
4. **Runs nginx in the foreground**: Sets up a simple web server to serve as a lightweight HTTP server.

## Dockerfile Contents

```Dockerfile
FROM ubuntu:latest

LABEL name="saikiran"

ENV AWS_ACCESS_KEY_ID=SDFSDFSDFSDFSDFSDFSDFSDF
ENV AWS_SECRET_KEY_ID=SDSDSDSDSDSDSDSDSDSDSDSD
ENV AWS_DEFAULT_REGION=us-east-1

ARG T_VERSION='1.6.6'
ARG P_VERSION='1.8.0'

RUN apt update && apt install -y jq net-tools curl wget unzip \
    && apt install -y nginx iputils-ping

RUN wget https://releases.hashicorp.com/terraform/${T_VERSION}/terraform_${T_VERSION}_linux_amd64.zip \
    && wget https://releases.hashicorp.com/packer/${P_VERSION}/packer_${P_VERSION}_linux_amd64.zip \
    && unzip terraform_${T_VERSION}_linux_amd64.zip \
    && unzip packer_${P_VERSION}_linux_amd64.zip \
    && chmod 777 terraform \
    && chmod 777 packer \
    && ./terraform version \
    && ./packer version

CMD ["nginx", "-g", "daemon off;"]
```

### Key Sections Explained:
- **Base Image**: Uses `ubuntu:latest` as the foundation for the Docker image.
- **Environment Variables**: Sets AWS credentials and default region (modify these for your actual usage).
- **ARG Variables**: Defines build arguments for specifying Terraform and Packer versions.
- **RUN Commands**: Updates package lists, installs necessary tools, and downloads Terraform and Packer.
- **CMD Instruction**: Starts the Nginx server in the foreground when the container runs.

## Building the Docker Image

To create the Docker image from the Dockerfile, use the following command:

```bash
docker build -t your_image_name:v3 .
```

### Additional Build Options
- **Build without Cache**: To ensure a fresh build without using any cached layers:
    ```bash
    docker build --no-cache -t your_image_name:v3 .
    ```

- **Pass Build Arguments**: Specify versions for Terraform and Packer during the build process:
    ```bash
    docker build --build-arg T_VERSION=1.4.0 --build-arg P_VERSION=1.5.0 -t your_image_name:v3 .
    ```

- **Build with Plain Progress Output**: For a more verbose output during the build:
    ```bash
    docker build --progress=plain -t your_image_name:v3 .
    ```

## Running the Container

After building the image, you can run a container from it using:

```bash
docker run -it your_image_name:v3 /bin/bash
```

### Check Versions
To check the installed versions of Terraform and Packer:
```bash
docker run -it your_image_name:v3 /bin/bash -c './terraform version && ./packer version'
```

## Setting Environment Variables

When running the container, you can pass AWS credentials and region as environment variables:

```bash
docker run -p 80:80 \
  -e AWS_ACCESS_KEY_ID=your_access_key \
  -e AWS_SECRET_KEY_ID=your_secret_key \
  -e AWS_DEFAULT_REGION=your_region \
  your_image_name:v3
```

## Useful Docker Commands

### Adding a Tag
To add a tag to an existing image, use:
```bash
docker tag your_image_name:v3 your_image_name:latest
```

### Check Image History
To inspect the history of your Docker image:
```bash
docker history your_image_name:v3
```

## Dockerfile Instructions Explained

### ADD
- **Description**: Adds files from your host system to the Docker image.
- **Use Case**: Useful for adding archives or files that need extraction.

### COPY
- **Description**: Copies files from your host system to the Docker image.
- **Use Case**: Ideal for transferring local files and directories.

### ENTRYPOINT
- **Description**: Configures the container to run as an executable.
- **Use Case**: Used for running a specific application when the container starts.

### WORKDIR
- **Description**: Sets the working directory inside the image.
- **Use Case**: Useful for organizing files and commands that run in the specified directory.

---

## Conclusion

This repository provides a foundational setup for creating a Docker container with Terraform and Packer installed on an Ubuntu base image. By following the instructions outlined in this README, you can easily build, run, and manage your Docker container, setting the stage for effective infrastructure automation.

Feel free to modify environment variables and versions as per your project requirements. If you encounter any issues, please consult the [Docker documentation](https://docs.docker.com/) or reach out for assistance!
