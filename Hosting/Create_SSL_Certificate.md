# Create a SSL Certificate for Apache in Ubuntu 20.04

An SSL (Secure Sockets Layer) certificate is a digital certificate that provides authentication and secure communication over the internet, ensuring that the data exchanged between the server and the client remains private and integral.

## Let’s Encrypt

Let’s Encrypt is an open, automated certificate authority (CA) provided by Internet Security Research Group (ISRG) so that users can enable secure connection (https) on their websites for free, without hassle.

### Step 1: Update System

Execute the following command to update your ubuntu server with the most updated packages.

```sh
sudo apt update && sudo apt upgrade -y
```

### Step 2: Install Certbot

Certbot is a free, open source software tool for automatically using Let’s Encrypt certificates on manually-administrated websites to enable HTTPS.

```sh
apt install certbot -y
```

### Step 3: Install Certbot plugin for Apache or Nginx

If you are using Apache webserver then use the following command to install the respective certbot plugin for Apache.

```sh
apt install python3-certbot-apache -y
```

If you are using Nginx, then use this command:

```sh
apt install python3-certbot-nginx -y
```

>

### Step 4: Generate Let’s Encrypt SSL using Certbot

It’s time to generate your free ssl!

- Command for Apache users:

```sh
certbot --apache
```

- Command for Nginx users:

```sh
certbot --nginx
```

### Step 5: Generate Let’s Encrypt SSL using Certbot

Last step is to **restart** our web server so that the new configuration with SSL enabled are applied.

- If you are using Apache then use this command:

```sh
systemctl restart apache2
```

- or the following command for Nginx web server:

```sh
systemctl restart nginx
```

### Step 6: Check Status & Renewal SSL

- Check Status of Certbot

```sh
systemctl status certbot.timer
```

- Dry Run SSL Renewal

```sh
certbot renew --dry-run
```
