# 🚀 Setup Guide: Install Essential Tools on Ubuntu, Mac, and Windows system

## 🔹 Ubuntu/Linux Setup

### Step-by-Step Manual Installation
```bash
# Update package list
sudo apt-get update

# Install Docker
sudo apt-get install -y docker.io

# Install AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Install Python
sudo apt-get install -y python3 python3-pip
```

### Easy Installation:
If the above commands are not run then execute [Script.md](./Script.md)

---

## 🔹 macOS Setup

Run these commands in the macOS terminal to install Docker, AWS CLI, and Python.

---

```bash
# Install Homebrew (if not already installed)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Install Docker
brew install --cask docker

# Install AWS CLI
brew install awscli

# Install Python
brew install python
```

---

## 🔹 Windows Setup

Run these commands in a PowerShell terminal with Administrator privileges.

---

```powershell
# Install Chocolatey (if not already installed)
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Install Docker Desktop
choco install docker-desktop

# Install AWS CLI
choco install awscli

# Install Python
choco install python
```
