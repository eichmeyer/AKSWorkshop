# WSL1 and Docker for Windows

## Preparation

Install [Docker for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

In Docker -> general settings, you’ll want to expose the daemon without TLS. However, do not allow it in the Windows Firewall. We only stay on localhost.
Docker Kubenetes is not needed in this workshop.

## Install WSL

If not already done:

Open PowerShell as Administrator and run:
```PowerShell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
Restart your computer when prompted.

The Linux distribution is installed via the Windows Store.
Microsoft Store: [Ubuntu](https://www.microsoft.com/en-us/p/ubuntu/9nblggh4msv6)
The first time you start Ubutu, a user name and password will be created. The password is required for sudo.

## WSL (Ubuntu)

This guide describes how to configure WSL1 to use Docker for Windows:

```BASH
# Switch to root
sudo su

# Install Azure CLI (AZ):
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# Update the apt package list. Install pip anf git client.
apt-get update -y

# Install Docker's package dependencies.
apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common

# Download and add Docker's official public PGP key.
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify the fingerprint.
apt-key fingerprint 0EBFCD88

# Add the `stable` channel's Docker upstream repository.
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

# Update the apt package list (for the new apt repo). Install the latest version of Docker CE, git and PIP.
apt-get update -y
apt-get install -y \
   docker-ce \
   git \
   python python-pip

# Allow your user to access the Docker CLI without needing root access.
usermod -aG docker $USER

# Docker for Windows expects you to supply your volume paths in a format that matches this: /c/Users/nick/dev/myapp
ln -s /mnt/c /c

# exit sudo
exit

# Install Docker Compose into your user's home directory.
pip install --user docker-compose

# Configure WSL to Connect to Docker for Windows
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc

# You should get a bunch of output about your Docker daemon.
# If you get a permission denied error, close + open your terminal and try again.
docker info

# You should get back your Docker Compose version.
docker-compose --version
```