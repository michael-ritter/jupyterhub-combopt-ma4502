# Set Up the Virtual Machine

To start the JupyterHub deployment process, we need to set up an Ubuntu 18.04 server and configure key login.

[TOC]

## Add an SSH Key

The public SSH key we created needs to be added to the list of authorized keys for login to our server. To do this, open a terminal and enter the following commands (we will assume the server is called ```m09vm14.ma.tum.de``` and accessible from the network you are in):

```
ssh m09vm14.ma.tum.de
mkdir ~/.ssh
cd ~/.ssh
vim authorized_keys
```

This will open the vim editor with the file ```authorized_keys```. Press the button ```i``` to enter insert mode, paste the contents of the clipboard (the public key), then press ```esc``` and ```:wq``` to save the file and quit vim.
Note the IP address of the new droplet. We need to IP address to log into our server with PuTTY. Copy the IP address of the droplet to the clipboard.

## Log into the server over SSH

Open a terminal and enter the following command to see if you can connect to the server:

```text
ssh -i ~/.ssh/jupyterhub_rsa ritter@m09vm14.ma.tum.de
```

You should now see the server prompt as user ```ritter```. First, let's make sure everything is up to date:

```text
$ sudo apt-get update
$ sudo apt-get upgrade
```

## Setting up a new user

If you want to set up additional users at this point, you may do so with the ```adduser``` command. I called my new user ```peter```.
  
```text
$ adduser peter
```

Set a new password and confirm:

```text
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

The user details can be skipped by pressing [Enter]. Then [Y] to complete the new user setup.

```text
Changing the user information for username
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n]
```

Now let's give our new user sudo privileges:

```text
$ usermod -aG sudo peter
```

The new user account is created and the new user has sudo privileges. We can switch accounts and become the new user with:

```text
$ sudo su - peter
```

The new user ```peter``` should have ```sudo``` privileges. This means when acting as ```peter``` we should be able to look in the ```/root``` directory.

```text
$ sudo ls -la /root
```

If you can see the contents of ```/root```, then the new user ```peter``` is set up with sudo access.

To exit out of the new sudo user's profile, and get back to using the ```ritter``` profile, type ```exit``` at the prompt. Finally, log out by entering the following command:

```text
$ exit
```

## Configuring the firewall

Next, we need to open the **ufw** firewall to OpenSSH traffic. We we'll communicate with the server over SSH and need the **ufw** firewall to allow this type of communication through.

```text
$ sudo ufw allow OpenSSH
$ sudo ufw allow ssh
$ sudo ufw enable
$ sudo ufw status
```

We can see that OpenSSH is now allowed.

```text
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
```

Now we can exit out of the ```ritter``` profile. This terminates the SSH session.

```text
$ exit
```

## Next Steps

The next step is to install Python and JupyterHub on the server. In particular, we will create a virtual environment, and install **JupyterHub** into the virtual environment.

<br>
