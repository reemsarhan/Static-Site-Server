# Static-Site-Server




## Step 1: Create a Remote Linux Server

1. **Log in to Azure Portal**: Go to Azure Portal and sign in.
2. **Create a Virtual Machine**:
   - Navigate to Virtual Machines in the Azure portal and click Create.
   - Select the following options:
     - **Image**: Ubuntu Server 20.04 LTS or another Linux distro.
     - **Size**: A small size like B1s should suffice for a simple static site.
     - **Authentication type**: SSH public key (upload your `.ssh/id_rsa.pub` or generate one using `ssh-keygen`).
     - Configure Networking to allow SSH (port 22) and HTTP (port 80).
   - Complete the setup and deploy the VM.
3. **Connect to the VM**:
   - Copy the VM's public IP address.
   - Connect using SSH:
     ```bash
     ssh <username>@<public-ip>
     cp StaticSiteServerVM_key.pem ~/StaticSiteServerVM_key.pem
     chmod 600 ~/StaticSiteServerVM_key.pem
     ssh -i ~/StaticSiteServerVM_key.pem azureuser@57.154.184.152
     ```

---

## Step 2: Install and Configure Nginx

1. Update the system and install Nginx:
   ```bash
   sudo apt update
   sudo apt install nginx -y
   sudo systemctl start nginx
   sudo systemctl enable nginx
   sudo systemctl status nginx
   ```
2. Test Nginx: Open the VM's public IP in your browser (http://<public-ip>). You should see the default Nginx welcome page.

---
## Step 3: Create a Simple Webpage
1.Create the required directories and files:

```bash
sudo mkdir -p /var/www/static_site
sudo nano /var/www/static_site/index.html
sudo nano /var/www/static_site/style.css
```
Set the correct permissions:
```bash
sudo chown -R www-data:www-data /var/www/static_site
sudo chmod -R 755 /var/www/static_site
```

Edit Nginx configuration:
```bash
sudo nano /etc/nginx/sites-available/default
```
Update the server block as follows:
```
server {
    listen 80;
    server_name <your-domain-or-ip>;

    root /var/www/static_site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Test Nginx configuration:

```bash
sudo nginx -t
```

Restart Nginx:
```bash
sudo systemctl restart nginx
```

Test the site:
```bash
curl http://57.154.184.152
```
---

##  Step 3: Create a Simple Webpage

Create the required directories and files:

```bash
sudo mkdir -p /var/www/static_site
sudo nano /var/www/static_site/index.html
sudo nano /var/www/static_site/style.css
```

Set the correct permissions:
```bash
sudo chown -R www-data:www-data /var/www/static_site
sudo chmod -R 755 /var/www/static_site
```

Edit Nginx configuration:
```bash
sudo nano /etc/nginx/sites-available/default
```

Update the server block as follows:
```
server {
    listen 80;
    server_name <your-domain-or-ip>;

    root /var/www/static_site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Test Nginx configuration:
```bash
sudo nginx -t
```

Restart Nginx:
```bash
sudo systemctl restart nginx
```
Test the site:
```bash
curl http://57.154.184.152
```

---
## Step 4: Delete the Virtual Machine (VM)
To delete a Virtual Machine (VM) in Microsoft Azure, follow these steps:

1. Go to the Azure Portal: Open Azure Portal and log in.
2. Navigate to Virtual Machines: On the left menu, click "Virtual machines".
3. Select the VM: Find and click on the name of the VM you want to delete.
4. Click Delete: In the VM overview page, click "Delete" at the top.
5. Confirm Deletion: Type the name of the VM in the confirmation box and click "Delete".
6. Confirm Deletion: Type the name of the VM in the confirmation box and click "Delete".

---
