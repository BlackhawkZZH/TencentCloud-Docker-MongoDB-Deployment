# Docker and MongoDB Installation Guide (Tencent Cloud Virtual Machine)

© 2024 [Ai{DEAL} Studio](https://anpengcheng.cn/). All rights reserved.

This guide provides step-by-step instructions for installing Docker and MongoDB on a Tencent Cloud Virtual Machine(CVM) instance located in mainland China. Once the installation is complete, a local MongoDB server will be set up on the instance and accessible over the internet. Follow the steps below to ensure a seamless installation process.

## Contents

1. [Prerequisites](#prerequisites)
2. [OPTIONAL: Reset the Root Password](#optional-reset-the-root-password)
3. [Installation](#installation)
4. [Usage](#usage)

## Prerequisites

A Tencent CVM instance with any configuration is required. To ensure accessibility within mainland China, it is recommended to select a server located in a Chinese city. For this guide, I chose a server located in Nanjing with the default configuration.

Once the CVM instance is activated, you can use the following command in the terminal to check the system details:

```
cat /etc/os-release
```

You will see something like this:

```
NAME="TencentOS Server"
VERSION="3.1 (Final)"
ID="tencentos"
ID_LIKE="rhel fedora centos"
VERSION_ID="3.1"
PLATFORM_ID="platform:el8"
PRETTY_NAME="TencentOS Server 3.1 (Final)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:tencentos:tencentos:3"
HOME_URL="https://cloud.tencent.com/product/ts"

TENCENT_SUPPORT_PRODUCT="tencentos"
TENCENT_SUPPORT_PRODUCT_VERSION="3"
NAME_ORIG="TencentOS Server"
```

## OPTIONAL: Reset the Root Password

To ensure a smoother installation process when encountering root permission verification, you can set a simpler root password during the installation. Replace <username> with root and <new_password> with your desired password in the following command to reset the root password, bypassing any password strength checks:

```
echo "<username>:<new_password>" | sudo chpasswd
```

**NOTE:** It is recommended to change the password back to a more secure one after completing the installation to ensure the security of your CVM instance.

## Installation

The following steps demonstrate how to install Docker and MongoDB on TencentOS. The process for other systems should be similar.

### **Step 1: Update the System**

Update your TencentOS to ensure all packages are up-to-date:

```bash
sudo yum update -y
```

### **Step 2: Install Required Dependencies**

Install necessary tools to enable Docker installation:

```bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### **Step 3: Add the Docker Repository**

Set up the official Docker repository:

```bash
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### **Step 4: Install Docker**

Install Docker Engine and related tools:

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### **Step 5: Start and Enable Docker**

Start the Docker service and ensure it runs on boot:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

### **Step 6: Verify Docker Installation**

Check if Docker is installed correctly:

```bash
docker --version
```

Run the following command to verify Docker is working:

```bash
sudo docker run dockerhub.xianfish.site/library/hello-world
```

If you see a "Hello from Docker!" message, Docker is successfully installed.

### **Step 7: Install and Run MongoDB in Docker**

Use docker to pull mongo dependencies from mirror repository:

```
docker pull dockerhub.xianfish.site/library/mongo:latest
```

Run MongoDB in port `27017` with root username `admin` and password `securepassword`

```
docker run --name mongo-auth -d \
  -p 27017:27017 \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=securepassword \
  dockerhub.xianfish.site/library/mongo:latest
```

### **Step 8: Verify MongoDB Installation and Status**

Check if MongoDB is installed and running properly:

```
docker ps
```

If everything works well, you will see somthing like this:

```
CONTAINER ID   IMAGE                                          COMMAND                  CREATED        STATUS        PORTS                                           NAMES
6996e1a20d25   dockerhub.xianfish.site/library/mongo:latest   "docker-entrypoint.s…"   xx hours ago   Up xx hours   0.0.0.0:27017->27017/tcp, :::27017->27017/tcp   mongodb
```

## Usage

You can access MongoDB using MongoDB Compass with the following connection string:

```
mongodb://<username>:<password>@<your_cvm_public_ip>:27017/
```
