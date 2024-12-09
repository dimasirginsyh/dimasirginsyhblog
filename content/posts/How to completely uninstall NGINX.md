---
title: How to completely uninstall NGINX
author: Dimas Irgiansyah
date: 2024-12-08
draft: false
tags:
  - mitoyus
  - blog
  - nginx
  - english
  - chatgpt
---
To completely uninstall NGINX from your system, follow the appropriate instructions based on your operating system:

### For Ubuntu/Debian-based systems:

1. **Stop the NGINX service**:
    
    ```bash
    sudo systemctl stop nginx
    ```
    
2. **Disable the NGINX service (so it doesn't start on boot)**:
    
    ```bash
    sudo systemctl disable nginx
    ```
    
3. **Remove NGINX and its dependencies**: Run the following command to remove the NGINX package:
    
    ```bash
    sudo apt-get purge nginx nginx-common nginx-full nginx-core
    ```
    
4. **Remove any unused dependencies**: After removing NGINX, you can clean up any unused packages:
    
    ```bash
    sudo apt-get autoremove
    ```
    
5. **Delete configuration and log files (optional)**: If you want to remove all configuration and log files (warning: this deletes custom configurations), use:
    
    ```bash
    sudo rm -rf /etc/nginx /var/log/nginx /var/cache/nginx
    ```
    
6. **Check if NGINX is completely removed**: You can check if NGINX is still installed by typing:
    
    ```bash
    which nginx
    ```
    
    If nothing is returned, NGINX is successfully uninstalled.
    

### For CentOS/RHEL-based systems:

1. **Stop the NGINX service**:
    
    ```bash
    sudo systemctl stop nginx
    ```
    
2. **Disable NGINX from starting at boot**:
    
    ```bash
    sudo systemctl disable nginx
    ```
    
3. **Remove the NGINX package**: Run the following command to remove NGINX:
    
    ```bash
    sudo yum remove nginx
    ```
    
4. **Clean up unused dependencies**: You can remove unused dependencies with:
    
    ```bash
    sudo yum autoremove
    ```
    
5. **Remove configuration and log files (optional)**: If you want to remove the configuration files and logs, use:
    
    ```bash
    sudo rm -rf /etc/nginx /var/log/nginx /var/cache/nginx
    ```
    
6. **Verify removal**: To confirm NGINX is uninstalled, check:
    
    ```bash
    which nginx
    ```
    
    If nothing is returned, it means NGINX is successfully uninstalled.
    

### For Arch Linux:

1. **Stop the NGINX service**:
    
    ```bash
    sudo systemctl stop nginx
    ```
    
2. **Disable the NGINX service**:
    
    ```bash
    sudo systemctl disable nginx
    ```
    
3. **Uninstall NGINX**:
    
    ```bash
    sudo pacman -Rns nginx
    ```
    
4. **Remove configuration and log files (optional)**:
    
    ```bash
    sudo rm -rf /etc/nginx /var/log/nginx /var/cache/nginx
    ```
    
5. **Verify the removal**: Check that NGINX is removed by:
    
    ```bash
    which nginx
    ```
    

By following these steps, you will have completely uninstalled NGINX from your system.