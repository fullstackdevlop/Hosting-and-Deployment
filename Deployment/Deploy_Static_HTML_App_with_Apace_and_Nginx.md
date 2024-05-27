# Deploy Static Web App Using Nginx

We will provide a comprehensive guide on serving static content using NGINX, offering a step-by-step approach for a practical and hands-on experience.

## Prerequisites

- A HTML Web Application
- A Linux Machine
- A Registered Domain (Point your domian to server)

### Step 1: Update & Upgrade Linux OS Package

Before installing NGINX, it is crucial to update the Linux OS package.

```sh
sudo apt update
```

- Now, We need to upgrade the Linux OS package

```sh
sudo apt upgrade
```

### Step 2: Install NGINX

- Execute the below command to install NGINX on your machine.

```sh
sudo apt install nginx
```

- Once we have installed the NGINX, we will need to **start** the NGINX service.

```sh
sudo service nginx start
```

### Step 3: Configuring NGINX for the Sample Web App

After installation of NGINX in Ubuntu machine, if we go to `cd /etc/nginx` directory we will see the below files/directories.

- **nginx.conf file:** This is the NGINX main configuration file.
- **sites-available directory:** In this directory, we can put all the sites or static content. We can make the sites/static pages available in this directory and later we can enable/disable them as per our wish.
- **sites-enabled directory:** In this directory, we can create a symlink to the `sites-available` directory for the concerned static webpage.

### Step 4: Uploading HTML Application Files with\*\* \*\*`scp`

- Create a directory under `/var/www`

```sh
sudo mkdir /var/www/sample-app
```

- Add permission to created folder.

```sh
sudo chmod -R 777 sample-app
```

- Copy all local HTML files using the `*` wildcard to `/var/www/sample-app`:

```sh
# Syntax
scp files_to_copy username@server_ip:path_on_server

# Example For Linux or Mac
scp -r ./html-app/* username@server_ip:/var/www/sample-app

# Example For Windows
scp -r .\html-app\* username@server_ip:\var\www\sample-app
```

> Note: If you want move only html-app folder use `./html-app`, not use `./html-app/*`

- If you are using `.pem` file then follow below syntax.(Only for pem file)

```sh
#Example
scp -i pem_file_path\pem_file_name -r .\html-app username@server_ip:\var\www\sample-app

# Syntax
scp -i C:\Users\ajay\Downloads\ajay.pem -r .\html-app ajay@216.32.44.12:\var\www\sample-app
```

### Step 4: Create a Configuration File under /etc/nginx/sites-available

Run the below command to create a configuration file under `/etc/nginx/sites-available` for a new static web page.

```sh
sudo nano /etc/nginx/sites-available/your_domain.com.conf
```

- The above command will open up a new file in the editor. Now, enter the below content:

```sh
server {
    listen 80;
    listen [::]:80;
    server_name sampleapp www.sampleapp.com ;
    root /var/www/sample-app;
    index index.html index.htm;
    location / {
    try_files $uri $uri/ =404;
    }
}
```

- Save and exit to virtual host file.

```sh
ctrl + x
y
enter
```

### Step 5: Create symlink of the sites created under sites-available to sites-enabled

We have added a sample site (sample-app) under `sites-available` directory. In this step we will create a symlink of this site to the `sites-enabled` directory.

```sh
sudo ln -s /etc/nginx/sites-available/sample-app /etc/nginx/sites-enabled
```

> You can check enabled virtual host with `cd /etc/nginx/sites-enabled/` with `ls` command.

- Verify the Configurations

```sh
sudo nginx -t
```

- Reload or restart NGINX

```sh
# For Reload
sudo nginx -s reload


# For Restart
sudo systemctl restart nginx
```
