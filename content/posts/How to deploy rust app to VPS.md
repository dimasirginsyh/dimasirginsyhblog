---
title: How to deploy rust app to VPS
date: 2024-12-07
draft: false
tags:
  - mitoyus
  - blog
  - rust
  - english
  - chatgpt
---
Deploying a Rust application to a VPS (Virtual Private Server) involves several key steps. Here’s a comprehensive guide on how to do it:

### Prerequisites

1. **Rust Toolchain** installed on your local machine (using `rustup`).
2. **VPS Access**: SSH access to the VPS (for example, using SSH keys).
3. **VPS OS**: Linux-based OS (Ubuntu, Debian, CentOS, etc.) is assumed.
4. **Cargo** (Rust's package manager and build tool) should be installed on the VPS.
5. A **web server** (like Nginx or Apache) for serving HTTP applications (optional, only necessary for web apps).

### Step-by-Step Guide

#### 1. Build Your Rust Application

First, build your Rust app for release on your local machine:

```bash
cargo build --release
```

This will create an optimized, production-ready build of your application in the `target/release` directory.

#### 2. Set Up Your VPS

Make sure you have access to your VPS and can SSH into it. You’ll need to install some dependencies for Rust and your application.

- **SSH into your VPS**:
    
    ```bash
    ssh user@your_vps_ip
    ```
    

#### 3. Install Rust on the VPS

Install Rust on your VPS if it’s not already installed:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Once Rust is installed, make sure it’s available in your `$PATH` by sourcing the Rust environment variables:

```bash
source $HOME/.cargo/env
```

#### 4. Transfer Your Rust Application to the VPS

There are multiple ways to transfer your application to your VPS. Here are two common approaches:

- **Using SCP (Secure Copy Protocol)**
    
    From your local machine, run this command to transfer the compiled binary to your VPS:
    
    ```bash
    scp target/release/your_app user@your_vps_ip:/home/user/your_app
    ```
    
- **Using Git** (if you use version control)
    
    If you have your Rust application in a Git repository, you can clone it on your VPS:
    
    ```bash
    git clone https://github.com/yourusername/your-repository.git
    cd your-repository
    cargo build --release
    ```
    

#### 5. Install Dependencies (Optional)

If your app has dependencies like a database or additional libraries, install them on the VPS. For example, if you’re using PostgreSQL, install it:

```bash
sudo apt-get update
sudo apt-get install postgresql
```

#### 6. Run the Application

Once your application is transferred to the VPS, run it:

```bash
./your_app
```

If it’s a long-running service (like a web server or API), you might want to run it in the background.

- **Run in background using `nohup`**:
    
    ```bash
    nohup ./your_app &> app.log &
    ```
    
- **Using `systemd` for automatic service management** (recommended for production)
    
    Create a `systemd` service file for managing your application.
    
    ```bash
    sudo nano /etc/systemd/system/your_app.service
    ```
    
    Example `your_app.service`:
    
    ```ini
    [Unit]
    Description=Your Rust Application
    After=network.target
    
    [Service]
    Type=simple
    ExecStart=/home/user/your_app
    Restart=always
    User=user
    WorkingDirectory=/home/user
    
    [Install]
    WantedBy=multi-user.target
    ```
    
    Then reload `systemd`, enable and start the service:
    
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable your_app
    sudo systemctl start your_app
    ```
    

#### 7. Configure Firewall and Open Ports (Optional)

If your app uses a specific port (for example, HTTP on port 8080), you’ll need to open that port in your VPS firewall:

```bash
sudo ufw allow 8080/tcp
```

#### 8. Set Up Reverse Proxy (Optional for Web Apps)

If you are deploying a web app and want to serve it through a domain, you might want to use Nginx or Apache as a reverse proxy.

Example Nginx config:

```bash
sudo nano /etc/nginx/sites-available/your_app
```

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://127.0.0.1:8080;  # Assuming your app runs on port 8080
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

Link it to `sites-enabled` and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/your_app /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

#### 9. Monitor and Maintain the Application

- **Logs**: Use `journalctl` to check the logs of your service:
    
    ```bash
    journalctl -u your_app.service
    ```
    
- **Updates**: Update your Rust app by pulling the latest changes and recompiling it:
    
    ```bash
    git pull
    cargo build --release
    ```
    
- **Stop the app**:
    
    ```bash
    sudo systemctl stop your_app
    ```
    
    After updating, restart it:
    
    ```bash
    sudo systemctl start your_app
    ```
    

---

### Conclusion

Deploying a Rust app to a VPS is straightforward, especially once you've compiled it for release. Ensure you handle things like service management with `systemd`, security (firewall, SSH), and reverse proxies for web applications to ensure your app runs smoothly in a production environment.