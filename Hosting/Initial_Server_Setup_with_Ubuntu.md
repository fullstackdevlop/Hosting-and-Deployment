# Initial Server Setup with Ubuntu

- [Access VPS Server with Root or sudo-enabled user](#1)
- [Add New User on VPS Hosting Remote Server](#2)
- [Delete User on VPS Hosting](#3)
- [Create Root User on VPS Hosting](#4)
- [Reset SSH Configuration on VPS Hosting](#5)
- [Change SSH Default Port on VPS Hosting](#6)
- [Disable Root Login on VPS Hosting](#7)
- [Disable Password Authentication on VPS Hosting](#8)
- [Set TimeZone on Remote Server](#9)
- [Use PuTTY to Access Your Server](#10)
- [Connect VPS Servers with WinSCP](#11)

## <span id="1">Access VPS Server with Root or sudo-enabled user</span>

SSH in to your server as the root user:

```sh
ssh root@your_server_ip_address
```

## Add New User on VPS Hosting Remote Server

- Use the `adduser` command to add a new user `ajay` to your system:

```sh
adduser ajay
```

- Navigate to new user directory like `ajay` user.

```sh
cd /home
ls
cd ajay
```

- Check user directory info

```sh
ls -a

# .bash_history  .bash_logout  .bashrc  .cache  .cloud-locale-test.skip  .local  .profile  .ssh
```

- Login with created new user with user name & password

```sh
ssh username@your_server_ip_address

ssh ajay@181.211.78.227
```

> New user can setup SSH key for auto login on their system.

- To change a password for user named `ajay` by root user.

```sh
passwd ajay
```

## Delete User on VPS Hosting

You can delete the user itself, without deleting any of his or her files by typing this as root:

- Show all users

```sh
cut -d: -f1 /etc/passwd

# or
cd /home
ls
```

- Delete user name with `ajay`.

```sh
deluser ajay
```

- To delete user from `home` directory.

```sh
rm -r ajay
```

## Create Root User on VPS Hosting

## Reset SSH Configuration on VPS Hosting

## Change SSH Default Port on VPS Hosting

## Disable Root Login on VPS Hosting

## Disable Password Authentication on VPS Hosting

## Set TimeZone on Remote Server

## Use PuTTY to Access Your Server

To access your server using the PuTTy SSH client, simply follow the instructions below.

- Start by opening PuTTy.
- Enter your hostname or IP address into the provided field and set the SSH port to 22.
- Next, click the Open button to open the command line window.
- In the command line window, type in the SSH **username** then press enter.
- Type SSH password and press enter to login.

**[Learn Here](https://it.engineering.oregonstate.edu/accessing-unix-server-using-putty-ssh)**

### Save PuTTY for Auto login via ppk file.

- First of all open **PuTTYgen** and generate **.ppk** file. For this click Load button and upload **id_rsa** file. If you not configured SSH Key, Please follow SSH Key Setup steps.
- After upload id_rsa file click save private key (.ppk) file under .ssh or any location.
- Now open **PuTTY** and enter your user name with IP Address `(root@109.166.119.10)` with SSH port 22. To save configuration for future please enter a name in Saved Session.
- Go to `Connection > SSH > Auth` and select private key file (.ppk).
- After selecting .ppk file click on session & click on save button.
- In future open PuTTY and select saved session & click open.

**[Learn Here](https://www.liquidweb.com/kb/putty-ssh-keys)**

## Connect VPS Servers with WinSCP

Transferring files from your computer to a VPS server with WinSCP

> Ensure you have downloaded and installed [WinSCP](https://winscp.net/eng/download.php).

### Connect with User name & Password

- Launch WinSCP.
- Input your VPS's public IP into the Host name field.
- Enter your User name and Password.
- The default port is 22. If you are using anoter port then add here.
- Click on **save** button for quick access in the future.
- Click **Login** to establish a connection.

### Connect without Password

- Launch WinSCP.
- Input your VPS's public IP into the Host name field.
- Enter your User name only not password.
- The default port is 22. If you are using anoter port then add here.
- Click on **Advanced...** button and select **SSH** section. After that click **Authentication** and select Private key file. If you don't have **.ppk**, Please setup SSH Key & generate **id_rsa** with `ssh-keygen -t rsa` command. Please follow SSH Key Setup steps.
- Click **Login** to establish a connection.

**[Learn Here](https://opensource.com/article/22/11/transfer-files-folders-windows-linux-winscp)**
