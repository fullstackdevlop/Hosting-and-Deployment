# SSH Key Setup & Access VPS Hosting Without Password

SSH (Secure SHELL) is an open-source and trusted network protocol that is used to log in to remote servers for the execution of commands and programs.

There are two ways of enabling SSH:

- **Password-based authentication** - Authentication with Password
- **Public key-based authentication** - Public key-based authentication is often called passwordless SSH.

## Password-based authentication

- To Get Access via SSH

```sh
ssh remote_username@remote_IP_Address

ssh root@216.32.44.12
```

Then enter your server password & login.

## Public key-based (passwordless) authentication

### Step:1 Run Below Commands on Your Local Machine.

Generate SSH Key

```sh
ssh-keygen -t rsa
```

The default key is of 2048 bits. However, if you want stronger security, you can change the value to 4096 bits.

```sh
ssh-keygen -t rsa -b 4096
```

### Step:2 Copy SSH Public Key to Remote Server

- Use the ssh-copy-id command (Only For Linux)

```sh
# Syntax
ssh-copy-id -p PORT -i ~/.ssh/keyname.pub remote_username@remote_IP_Address

# Example
ssh-copy-id -p 21350 -i ~/.ssh/id_rsa.pub root@216.32.44.12
```

- Copy using SSH (Only for Windows)

```sh
# Syntax
scp SSH_PUBLIC_KEY_PATH USERNAME@HOSTIP:\home\USERNAME\.ssh\authorized_keys

# Example
scp C:\Users\ajay.prasad\.ssh\id_rsa.pub root@206.189.133.70:\root\.ssh\authorized_keys
```

> Make sure you have `.ssh` folder in remote server

- Copy Manually (For Linux & Windows Both)

The third method is slightly more easy as its completely manual. Copy `id_rsa.pub` keys from local and paste it `.ssh\authorized_keys` file in server.

## Disable Password Authentication (Optional)

For increased security, you can disable password authentication on the remote server and only allow SSH key authentication.

- Open the SSH server configuration file on the remote server

```sh
sudo nano /etc/ssh/sshd_config
```

Use the option no with the `PermitRootLogin` directive

```sh
PermitRootLogin no
```

- Save the file and restart the SSH service.

```sh
sudo systemctl restart sshd
# or
sudo systemctl restart ssh
```
