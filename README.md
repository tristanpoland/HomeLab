# Docker Homelab Setup Guide for Beginners

## Table of Contents
1. [What is a Homelab?](#what-is-a-homelab)
2. [Prerequisites](#prerequisites)
3. [Installing Docker](#installing-docker)
4. [Understanding the Project Structure](#understanding-the-project-structure)
5. [Setting Up Each Service](#setting-up-each-service)
6. [Accessing Your Services](#accessing-your-services)
7. [Troubleshooting](#troubleshooting)
8. [Next Steps](#next-steps)

## What is a Homelab?

A homelab is a personal collection of servers and services that you run at home for learning, development, and personal use. Think of it as your own private cloud! With Docker, you can easily run multiple services like:

- **Nextcloud**: Your personal cloud storage (like Google Drive)
- **Gitea**: Your own Git repository hosting (like GitHub)
- **Coder**: A cloud development environment
- **Portainer**: A web interface to manage your Docker containers

## Prerequisites

### Hardware Requirements
- **Computer**: Any modern computer with at least 4GB RAM (8GB+ recommended)
- **Storage**: At least 20GB free space
- **Network**: Internet connection for downloading Docker images

### What You'll Learn
- Basic command line usage
- How Docker works
- How to manage web services
- Basic networking concepts

## Installing Docker

### Windows Installation

1. **Download Docker Desktop**
   - Go to [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
   - Click "Download for Windows"

2. **Install Docker Desktop**
   - Run the installer as Administrator
   - Follow the installation wizard
   - Restart your computer when prompted

3. **Verify Installation**
   - Open Command Prompt or PowerShell
   - Type: `docker --version`
   - You should see something like: `Docker version 24.0.6`

### Linux Installation (Ubuntu/Debian)

1. **Update your system**
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Install Docker**
   ```bash
   # Remove old versions
   sudo apt remove docker docker-engine docker.io containerd runc
   
   # Install prerequisites
   sudo apt install ca-certificates curl gnupg lsb-release
   
   # Add Docker's official GPG key
   sudo mkdir -m 0755 -p /etc/apt/keyrings
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
   
   # Set up repository
   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   
   # Install Docker Engine
   sudo apt update
   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

3. **Add your user to Docker group**
   ```bash
   sudo usermod -aG docker $USER
   newgrp docker
   ```

4. **Verify Installation**
   ```bash
   docker --version
   docker compose version
   ```

## Understanding the Project Structure

Your homelab will be organized like this:

```
homelab/
├── nextcloud/          # Personal cloud storage
│   └── docker-compose.yml
├── gitea/              # Git repository hosting
│   └── docker-compose.yml
├── coder/              # Cloud development environment
│   └── docker-compose.yml
├── portainer/          # Docker management interface
│   └── docker-compose.yml
├── .gitignore          # Files to ignore in Git
└── README.md           # Project documentation
```

### What is docker-compose.yml?
Think of `docker-compose.yml` as a recipe that tells Docker:
- What services to run
- How they should be configured
- How they connect to each other
- What ports to use

## Setting Up Each Service

### Step 1: Create Your Project Directory

**Windows (PowerShell):**
```powershell
mkdir C:\homelab
cd C:\homelab
```

**Linux/macOS:**
```bash
mkdir ~/homelab
cd ~/homelab
```

### Step 2: Set Up Portainer (Docker Management)

Portainer gives you a web interface to manage your Docker containers - perfect for beginners!

1. **Navigate to the portainer folder:**
   ```bash
   cd homelab/portainer
   ```

2. **Start Portainer:**
   ```bash
   docker compose up -d
   ```

3. **Access Portainer:**
   - From the same computer: `https://localhost:9443`
   - From other devices: `https://YOUR_IP_ADDRESS:9443`
   - Create an admin account when prompted

### Step 3: Set Up Nextcloud (Personal Cloud)

Nextcloud is like having your own Google Drive at home!

1. **Navigate to the nextcloud folder:**
   ```bash
   cd ../nextcloud
   ```

2. **Start Nextcloud:**
   ```bash
   docker compose up -d
   ```
   This will take a few minutes the first time as it downloads everything.

3. **Access Nextcloud:**
   - From the same computer: `http://localhost:8081`
   - From other devices: `http://YOUR_IP_ADDRESS:8081`
   - Create an admin account when prompted

### Step 4: Set Up Gitea (Git Repository)

Gitea is like having your own GitHub at home!

1. **Navigate to the gitea folder:**
   ```bash
   cd ../gitea
   ```

2. **Start Gitea:**
   ```bash
   docker compose up -d
   ```

3. **Access Gitea:**
   - From the same computer: `http://localhost:3000`
   - From other devices: `http://YOUR_IP_ADDRESS:3000`
   - Complete the initial setup wizard

### Step 5: Set Up Coder (Development Environment)

Coder provides cloud-based development environments accessible from your browser!

1. **Navigate to the coder folder:**
   ```bash
   cd ../coder
   ```

2. **Create environment file:**
   You need to tell Coder what your IP address is:
   ```bash
   # Windows (PowerShell)
   echo "CODER_ACCESS_URL=http://YOUR_IP_ADDRESS:7080" > .env
   
   # Linux/macOS
   echo "CODER_ACCESS_URL=http://YOUR_IP_ADDRESS:7080" > .env
   ```
   **Replace YOUR_IP_ADDRESS with the actual IP you found earlier!**

3. **Start Coder:**
   ```bash
   docker compose up -d
   ```

4. **Access Coder:**
   - From the same computer: `http://localhost:7080`
   - From other devices: `http://YOUR_IP_ADDRESS:7080`
   - Create your first user account

## Understanding Networks and IP Addresses (The Simple Version)

Think of your home network like a neighborhood, and each device (computer, phone, tablet) is like a house with a unique address. This address is called an **IP address**.

### What is an IP Address?
An IP address is like a postal address for your computer. It's a set of numbers (like 192.168.1.100) that tells other devices on your network how to find your computer.

### Two Types of Access:
1. **Local Access (localhost)**: Only works on the same computer where Docker is running
2. **Network Access (IP address)**: Works from any device on your home network

### Why Do You Need This?
If you want to access your homelab services from your phone, tablet, or another computer in your house, you need to use your computer's IP address instead of "localhost".

## Finding Your Machine's IP Address

### Windows

**Method 1: Using Command Prompt (Easiest)**
1. Press `Win + R`, type `cmd`, and press Enter
2. Type: `ipconfig`
3. Look for "IPv4 Address" - it will look something like `192.168.1.100`
4. **Write this number down** - you'll need it later!

**Method 2: Using Settings (User-Friendly)**
1. Click the Start button
2. Go to Settings > Network & Internet
3. Click on your connection (Wi-Fi or Ethernet)
4. Scroll down to see your IP address

### Linux

**Method 1: Simple Command**
```bash
hostname -I
```
This shows your IP address. **Write down the number that looks like 192.168.x.x**

**Method 2: More Detailed**
```bash
ip addr show
```
Look for a number starting with 192.168 or 10.0 (ignore 127.0.0.1)

### What These Numbers Mean
- `192.168.1.x` - Most common home network addresses
- `192.168.0.x` - Also common home network addresses  
- `10.0.0.x` - Some home networks use this
- `127.0.0.1` - This is "localhost" - ignore this one
- `169.254.x.x` - This means no internet connection

**Example**: If your IP address is `192.168.1.100`, remember this number!

## Setting Up Each Service

Since you have the project files, setup is much easier! You don't need to create any files - just run commands.

### Step 1: Download or Clone the Project

If you haven't already, get the project files on your computer. You should have a folder structure like this:
```
homelab/
├── nextcloud/
├── gitea/
├── coder/
├── portainer/
└── README.md
```

### Step 2: Set Up Portainer (Docker Management)

1. **Navigate to the portainer folder:**
   ```bash
   cd homelab/portainer
   ```

2. **Start Portainer:**
   ```bash
   docker compose up -d
   ```

3. **Access Portainer:**
   - From the same computer: `https://localhost:9443`
   - From other devices: `https://YOUR_IP_ADDRESS:9443`
   - Create an admin account when prompted

### Step 3: Set Up Nextcloud (Personal Cloud)

1. **Navigate to the nextcloud folder:**
   ```bash
   cd ../nextcloud
   ```

2. **Start Nextcloud:**
   ```bash
   docker compose up -d
   ```
   This will take a few minutes the first time as it downloads everything.

3. **Access Nextcloud:**
   - From the same computer: `http://localhost:8081`
   - From other devices: `http://YOUR_IP_ADDRESS:8081`
   - Create an admin account when prompted

### Step 4: Set Up Gitea (Git Repository)

1. **Navigate to the gitea folder:**
   ```bash
   cd ../gitea
   ```

2. **Start Gitea:**
   ```bash
   docker compose up -d
   ```

3. **Access Gitea:**
   - From the same computer: `http://localhost:3000`
   - From other devices: `http://YOUR_IP_ADDRESS:3000`
   - Complete the initial setup wizard

### Step 5: Set Up Coder (Development Environment)

1. **Navigate to the coder folder:**
   ```bash
   cd ../coder
   ```

2. **Create environment file:**
   You need to tell Coder what your IP address is:
   ```bash
   # Windows (PowerShell)
   echo "CODER_ACCESS_URL=http://YOUR_IP_ADDRESS:7080" > .env
   
   # Linux/macOS
   echo "CODER_ACCESS_URL=http://YOUR_IP_ADDRESS:7080" > .env
   ```
   **Replace YOUR_IP_ADDRESS with the actual IP you found earlier!**

3. **Start Coder:**
   ```bash
   docker compose up -d
   ```

4. **Access Coder:**
   - From the same computer: `http://localhost:7080`
   - From other devices: `http://YOUR_IP_ADDRESS:7080`
   - Create your first user account

## Quick Setup Summary

Here's what you need to do in order:

1. **Find your IP address** (write it down!)
2. **Open terminal/command prompt**
3. **Navigate to your homelab folder**
4. **For each service folder, run:**
   ```bash
   cd [service-name]
   docker compose up -d
   cd ..
   ```

**That's it!** No need to create files - they're already there.

## Accessing Your Services

Once everything is running, you can access your services:

### From the Same Machine (Local Access)
| Service | URL | Purpose |
|---------|-----|---------|
| Portainer | https://localhost:9443 | Manage Docker containers |
| Nextcloud | http://localhost:8081 | Personal cloud storage |
| Gitea | http://localhost:3000 | Git repositories |
| Coder | http://localhost:7080 | Development environments |

### From Other Devices on Your Network
Replace `YOUR_IP_ADDRESS` with the IP address you found above:

| Service | URL | Purpose |
|---------|-----|---------|
| Portainer | https://YOUR_IP_ADDRESS:9443 | Manage Docker containers |
| Nextcloud | http://YOUR_IP_ADDRESS:8081 | Personal cloud storage |
| Gitea | http://YOUR_IP_ADDRESS:3000 | Git repositories |
| Coder | http://YOUR_IP_ADDRESS:7080 | Development environments |

**Example:** If your IP address is `192.168.1.100`, you would access:
- Nextcloud at `http://192.168.1.100:8081`
- Gitea at `http://192.168.1.100:3000`
- And so on...

### Firewall Considerations

**Windows:**
Your Windows Firewall might block incoming connections. You may need to:
1. Go to Windows Security > Firewall & network protection
2. Click "Allow an app through firewall"
3. Add Docker Desktop or allow the specific ports (3000, 7080, 8081, 9443)

**Linux:**
If you're using a firewall like UFW, you might need to open the ports:
```bash
sudo ufw allow 3000
sudo ufw allow 7080
sudo ufw allow 8081
sudo ufw allow 9443
```

**Router/Network:**
Most home networks allow internal communication by default, but if you're having issues, check your router's settings for any device isolation features.

## Troubleshooting

### Common Issues

**"Port already in use" error:**
```bash
# Check what's using the port
netstat -tlnp | grep :8081
# Or on Windows:
netstat -an | findstr :8081
```

**Container won't start:**
```bash
# Check logs
docker compose logs [service-name]
# For example:
docker compose logs nextcloud
```

**Out of disk space:**
```bash
# Clean up unused Docker data
docker system prune -a
```

### Useful Commands

```bash
# Start all services
docker compose up -d

# Stop all services
docker compose down

# View running containers
docker ps

# View logs
docker compose logs -f [service-name]

# Update a service
docker compose pull [service-name]
docker compose up -d [service-name]
```

## Next Steps

### Security Considerations
1. **Change default passwords** in all services
2. **Enable HTTPS** for external access
3. **Use a reverse proxy** like Nginx Proxy Manager
4. **Set up backups** for your data

### Expanding Your Homelab
- Add more services like Jellyfin (media server) or Home Assistant
- Set up monitoring with Grafana and Prometheus
- Configure automated backups
- Learn about Docker networking and volumes

### Learning Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Awesome Self-Hosted](https://github.com/awesome-selfhosted/awesome-selfhosted)

## Conclusion

Congratulations! You now have a fully functional homelab running on Docker. This setup gives you:

- Personal cloud storage with Nextcloud
- Your own Git hosting with Gitea
- Cloud development environments with Coder
- Easy container management with Portainer

Remember: homelabs are about learning and experimentation. Don't be afraid to break things and try new configurations. Every mistake is a learning opportunity!

**Need help?** The self-hosted community is very welcoming. Check out forums like r/selfhosted on Reddit or the Self-Hosted Discord server.

Happy homelabbing! 🚀