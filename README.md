# devops-roadmap-projects
The project from: https://roadmap.sh/projects/ssh-remote-server-setup

# Remote Linux Server Setup with SSH

This project involves setting up a basic remote Linux server and configuring it to allow SSH connections. It also includes an optional step to install Fail2ban for security enhancement.
## Steps

### 1. Create a Remote Server
Use your preferred VPWS provider to create a Linux server. For this guide, we are using a generic VPS provider.

- Log into your VPWS provider's control panel.
- Create a new server (e.g., Ubuntu 20.04 LTS).
- Note down the server's IP address.

### 2. Generate SSH Key Pairs
On your local machine, generate an SSH key pair to use for secure access to the server.

```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

- Name the key files (id_rsa or a custom name).
- This will create two files: a private key (id_rsa) and a public key (id_rsa.pub).
  
### 3. Add the SSH Key to the Server
Use the following command to copy the public key to your server:

```sh
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server-ip
```
Replace user with your server's username (e.g., root).
Replace server-ip with your server's IP address.

If ssh-copy-id doesn't work, you can manually copy the public key by logging in to the server using a password and editing the ~/.ssh/authorized_keys file:
```sh
ssh user@server-ip
mkdir -p ~/.ssh
echo "your-public-key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

### 4. Configure SSH Access on Your Local Machine
To simplify connecting to your server, edit the SSH configuration file on your local machine:

```sh
nano ~/.ssh/config
```
Add the following lines to the config file:

```sh
Host myserver
    HostName server-ip
    User user
    IdentityFile ~/.ssh/id_rsa
```
- Replace myserver with an alias you want to use for this server.
- Replace server-ip with your server’s IP address.
- Replace user with your server's username.
  
### 5. Test SSH Connection
Test the SSH connection using the alias you set up:

```sh
ssh myserver
```

# Install Fail2ban for Security

Install Fail2ban:
```sh
sudo apt update
sudo apt install fail2ban
```
Configure Fail2ban to protect SSH by editing the configuration file:
```sh
sudo nano /etc/fail2ban/jail.local
```
Add the following:

```sh
[sshd]
enabled = true
port = ssh
logpath = /var/log/auth.log
maxretry = 5
bantime = 600
findtime = 600
```

Restart the Fail2ban service:
```sh
sudo systemctl restart fail2ban
```







