# Deploy React with Apache Web Server

We'll deploy a React application on our local machine to an Ubuntu server running Apache.

## Prerequisites

- A React application
- A Hosting Account (AWS, DigitalOcean, Hostinger)
- A registered domain name (optional but recommended)

> Point your domain name to server or hosting provider. [Learn here...](https://github.com/fullstackdevlop/hosting-and-deployment/tree/point-domain-to-server)

### Step 1: Creating a React Project

- Create a new application using Create React App in your local environment.

```sh
npx create-react-app react-app
```

- Move into the project folder.

```sh
cd react-app
```

- Run it locally to test how it will appear on the server.

```sh
npm start
```

- Stop the project by entering either `CTRL+C` or `⌘+C` in a terminal. Now make a production **build** follow the below command.

```sh
npm run build
```

> Now you will get a `build` folder in your project. In this build directory, include compiled and minified versions of all the files that need for your project.

### Step 2: Connect to Your Remote Ubuntu Server or VPS

- Log in to your server using ``ssh```. Using the **IP address** and the **password** that you created earlier.

```sh
ssh username@server_ip
```

- Update and upgrade the packages of remote machine.

```sh
sudo apt update && sudo apt upgrade -y
```

- Verify that all required softwares are installed or Install the required packages.

```sh
apache2 -v
node -v
npm -v
```

- Install Apache

```sh
sudo apt install apache2
```

- Install Node and npm

```sh
sudo apt install -y nodejs
```

- Verify Apache2 is Active and Running

```sh
sudo service apache2 status
```

- Navigate to **www** directory & Create a project folder `react-app`.

```sh
cd /var/www

sudo mkdir react-app
```

> Also add permission to `react-app` folder using `sudo chmod -R 777 react-app`.

### Step 3: Uploading Build Files with\*\* \*\*`scp`

- Copy all the build files using the `*` wildcard to `/var/www/react-app`:

```sh
# Syntax
scp files_to_copy username@server_ip:path_on_server

# Example For Linux or Mac
scp -r ./build/* username@server_ip:/var/www/react-app

# Example For Windows
scp -r .\build\* username@server_ip:\var\www\react-app
```

> Note: If you want move only build folder use `./build`, not use `./build/*`

- If you are using `.pem` file then follow below syntax.(Only for pem file)

```sh
#Example
scp -i pem_file_path\pem_file_name -r .\build username@server_ip:\var\www\react-app

# Syntax
scp -i C:\Users\ajay\Downloads\ajay.pem -r .\build ajay@216.32.44.12:\var\www\react-app
```

### Step 4: Configuring Apache

Create a new Apache2 configuration file for your React application:

- Create Virtual Host File

```sh
sudo nano /etc/apache2/sites-available/your_domain.com.conf
```

- Add Following Code in Virtual Host File.

```sh
# For http
<VirtualHost *:80>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/react-app/build/

    <Directory /var/myapplication/pr_admin/build/>
        Options Indexes FollowSymLinks
        AllowOverride all
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# For https
<VirtualHost *:443>
    ServerName www.example.com
    ServerAdmin contact@example.com
    DocumentRoot /var/www/react-app/build/

    <Directory /var/myapplication/pr_admin/build/>
        Options Indexes FollowSymLinks
        AllowOverride all
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Save and exit to virtual host file.

```sh
ctrl + x
y
enter
```

- Enable Virtual Host

```sh
cd /etc/apache2/sites-available/
sudo a2ensite your_domain.conf
```

> You can check enabled virtual host with `cd /etc/apache2/sites-enabled/` with `ls` command.

- Check configuration is correct or not

```sh
sudo apache2ctl configtest
```

- Restart or reload Apache2

```sh
# For Restart
sudo service apache2 restart

# For Reload
sudo systemctl reload apache2
```

### Step 5: Modify Your Application

Make some changes in your local development project.

- After changes in local development make production build.

```sh
npm run build
```

- Delete the build folder on server.

```sh
cd /var/www/react-app

rm -rf build
```

- Upload local build folder on server `/var/www/react-app` following step 3.

> Using Apache as your web server, you can insert this into your `.htaccess` file:

```sh
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteCond %{REQUEST_FILENAME} !-l
  RewriteRule . /index.html [L]
</IfModule>
```
