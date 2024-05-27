# Deploy Next.js Application with Apache Web Server

Next.js is a popular React framework for building server-rendered React applications with ease.

## Prerequisites

- A Next.js application
- A Hosting Account (AWS, DigitalOcean, Hostinger)
- A registered domain name (optional but recommended)

> Point your domain name to server or hosting provider. [Learn here...](https://github.com/fullstackdevlop/hosting-and-deployment/tree/point-domain-to-server)

### Step 1: Creating a DigitalOcean Droplet

- Signup to Digital Ocean. Once you signup/create account and logged in, click on the button to [Create A Droplet](https://docs.digitalocean.com/products/droplets/how-to/create/).
- Once the droplet created, you will see the **IP address** something like this: 12.345.45.678

### Step 2: Setting Up the Droplet

Now you can login to your digital ocean droplet via terminal.

- SSH into your droplet using the **IP address** and the **password** that you created earlier.

```sh
Syntax:- ssh -p PORT USERNAME@<DROPLET_IP>

Example:- ssh -p 22 root@12.345.45.678
```

- Update and upgrade the packages on the droplet:

```sh
sudo apt update && sudo apt upgrade -y
```

- Verify that all required softwares are installed or Install the required packages:

```sh
apache2 -v
node -v
npm -v
pm2 --version
git --version
```

- Install Apache

```sh
sudo apt install apache2
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

- Add PM2 Process on Startup

```sh
sudo pm2 startup
```

- Install Git

```sh
sudo apt install git
```

- Verify Apache2 is Active and Running

```sh
sudo service apache2 status
```

- Verify Web Server Ports are Open and Allowed through Firewall (If firewall active).

```sh
sudo ufw status verbose
```

### Step 3: Deploying the Next.js Application

- Create your git repository and push your local code to Github private repo:

```sh
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <github-repo-url>
git push -u origin main
```

- Clone your Next.js application from your github account on the Droplet:

```sh
cd /var/www

git clone <github-repo-url>
```

- Run ls command to verify that the project is present:

```sh
ls
```

- Navigate to the Next.js application directory:

```sh
cd nextjs
```

- Install the application dependencies:

```sh
npm install
```

- Build the Next.js application:

```sh
npm run build
```

- Finally, start the Next.js application:

```sh
npm run start
```

### Step 4: Create .env files in production (optional)

If your project had `.env` file that were added to `.gitignore`, it’s time to create them in your project folders in digital ocean droplet.

- Create `.env` file inside your server folder

```sh
cd /var/www/nextjs
sudo vim .env
```

- Press `i` for insert (type or copy paste) env variables for production use.

```sh
i
```

- Now save and exit.

```sh
ESC + :wq
```

### Step 5: Configuring Apache

Create a new Apache2 configuration file for your Next.js application:

- Create Virtual Host File

```sh
sudo nano /etc/apache2/sites-available/your_domain.conf
```

- Add Following Code in Virtual Host File. Change the IP and Port According to your Node Application Code.

```sh
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    ProxyPreserveHost On
    #Write Your own Port
    ProxyPass / http://127.0.0.1:3000/
    ProxyPassReverse / http://127.0.0.1:3000/
    <Directory "/var/www/project_folder_name">
        AllowOverride All
    </Directory>
</VirtualHost>
```

- Save and exit to virtual host file.

```sh
ctrl + x
y
enter
```

- Enable the Proxy module with Apache

```sh
sudo a2enmod proxy
sudo a2enmod proxy_http
```

- Enable Virtual Host

```sh
cd /etc/apache2/sites-available/
sudo a2ensite your_domain.conf
```

- Check configuration is correct or not

```sh
sudo apache2ctl configtest
```

- Restart Apache2

```sh
sudo service apache2 restart
```

> Your Next.js application is now deployed and accessible at your domain name or droplet IP address. To keep your application running in the background and automatically **restart on crashes or server reboots**, you should use a process manager like **PM2.**

### Step 6: Setting Up PM2 Process Manager

We will use a tool called **PM2** to make sure that our Next.js application is always running. PM2 will even restart the Next.js application if it goes down.

- Navigate to the Next.js application directory (if not already there):

```sh
cd /var/www/nextjs
```

- Start the Next.js application using PM2:

```sh
pm2 start npm --name nestjs -- run start -- -p 3000
```

> This command will start the Next.js application with the name **“nextjs”** using the `npm start` command. PM2 will automatically restart the application if it crashes or if the server reboots.

- To ensure PM2 starts on boot, run:

```sh
pm2 startup
```

This command will generate a script that you can copy and paste into your terminal to enable PM2 to start on boot.

- Save the current PM2 processes:

```sh
pm2 save
```

- Check the current PM2 status

```sh
pm2 status
```

### Step 7: Modify Your Application

Now you can make some changes in your project local development VS Code and Pull it on Remote Server.

- Go to Your Project Directory:

```sh
cd /var/www/nextjs
```

- Pull the changes from Github repo:

```sh
git pull
```

- Create production build:

```sh
npm run build
```

- Reload using PM2

```sh
pm2 reload app_name
```
