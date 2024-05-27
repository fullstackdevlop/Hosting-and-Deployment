# Deploy Next.js Apps on Ubuntu with Nginx

This guide will show you how to deploy a **Next.js** app on Ubuntu with Nginx.

### Prerequisites

- A domain name
- A fresh Ubuntu 22.04 (64 Bit)
- A user with sudo privileges

### Step 1: Update your system

- First, we need to make sure that our system is up to date:

```sh
sudo apt update && sudo apt upgrade
```

- Verify that all required softwares are installed or Install the required packages:

```sh
nginx -v
node -v
npm -v
pm2 --version
git --version
```

### Step 2: Install Nginx, Node, PM2 & Git

- Install Nginx

```sh
sudo apt install nginx
```

- Install Node and NPM

```sh
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

sudo apt install -y nodejs
```

- Install PM2

```sh
sudo npm install -g pm2@latest
```

- Install Git

```sh
sudo apt install git
```

- Verify NGINX is Active and Running

```sh
sudo systemctl status nginx
```

### Step 3: UFW Firewall Configuration

Now, we need to configure the UFW firewall.

```sh
sudo ufw allow 'Nginx Full'
```

### Step 4: Allow OpenSSH

OpenSSH is the default SSH server on Ubuntu. We need to allow it in the firewall.

```sh
ufw allow OpenSSH
```

### Step 5: Enable Firewall

Now, we need to enable the firewall.

```sh
ufw enable
```

### Step 6: Create a Next.js App

Create a Next.js app with the following command at `/var/www` directory.

```sh
npx create-next-app@latest ubuntu-next-app
```

> Success! Created ubuntu-next-app at `/var/www/ubuntu-next-app`.

### Step 7: Configure Nginx

- Change directory to `sites-available` and list the files.

```sh
cd /etc/nginx/sites-available
ls
```

- Create a virtual host file

```sh
sudo nano /etc/nginx/sites-available/your_domain.conf
```

- Then, copy and paste the following config into the file.

```sh
server {
        listen 80;
        server_name ubuntu-next-app.mydomain.com; # !!! - change to your domain name
      gzip on;
        gzip_proxied any;
        gzip_types application/javascript application/x-javascript text/css text/javascript;
        gzip_comp_level 5;
        gzip_buffers 16 8k;
        gzip_min_length 256;

    location /_next/ {
                alias /var/www/ubuntu-next-app/.next/; # !!! - change to your app name
                expires 365d;
                access_log on;
        }

    location / {
                proxy_pass http://127.0.0.1:3000; # !!! - change to your app port
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection 'upgrade';
                proxy_set_header Host $host;
                proxy_cache_bypass $http_upgrade;
        }
}
```

- Now, save **(ctrl+o)** and exit **(ctrl+x)**.

- Time to enable the site.

```sh
sudo ln -s /etc/nginx/sites-available/ubuntu-next-app /etc/nginx/sites-enabled
```

> You can check enabled virtual host with `cd /etc/nginx/sites-enabled/` with `ls` command.

- Verify the Configurations

```sh
sudo nginx -t
```

- Restart Nginx again.

```sh
sudo systemctl restart nginx
```

### Step 8: Configure PM2

- Change directory to `/var/www/ubuntu-next-app` and list the files.

```sh
cd /var/www/ubuntu-next-app
```

- Start the app with PM2.

```sh
pm2 start npm --name ubuntu-next-app -- run start -- -p 3000
```

- To ensure PM2 starts on boot, run:

```sh
pm2 startup
```

- This command will generate a script that you can **copy and paste** into your terminal to enable PM2 to start on boot.

```
Output
[PM2] Init System found: systemd
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy
```

```sh
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u sammy --hp /home/sammy
```
