# Install JupyterHub

After the server is set up, it is time to install JupyterHub on the server.

[TOC]

## Update System

It is probably best to update the packages installed on the server in case there are updates to the operating system and installed packages since the server was created. 

Open a terminal, log into the server, then update the system:

```bash
$ sudo apt-get update
$ sudo apt-get upgrade
```

## Install Python

Before we install JupyterHub on the server, we need to install a current version of  Python and create a virtual environment. We'll install Python 3.7 following [this](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/) description for Ubuntu 18.04 and we will use the built-in capabilities for setting up a virtual environment. Then, using the **pip** package manager, we will install the necessary Python packages and JupyterHub. For an alternative installation using **Miniconda**, see [the original version of this documentation](https://professorkazarinoff.github.io/jupyterhub-engr101/install_jupyterhub/).

1. We start by installing the prerequisites:

    ```text
    $ sudo apt install software-properties-common
    ```

 1. Next, add the deadsnakes PPA to your sources list:

    ```text
    $ sudo add-apt-repository ppa:deadsnakes/ppa
    ``` 

    When prompted press `Enter` to continue.

2. Once the repository is enabled, install Python 3.7 with:

    ```text
    $ sudo apt install python3.7
    ```

    At this point, Python 3.7 is installed on your Ubuntu system and ready to be used. You can verify it by typing:

    ```text
    $ python3.7 --version
    ```
    
## Create a virtual environment and install and packages

For this JupyterHub install, we are going to create a conda environment (a virtual environment) and install packages into that environment. We'll call the conda environment ```jupyterhub``` and use ```python=3.7``` as our Python version. Then activate the ```jupyterhub``` environment and install **NumPy**, **Matplotlib**, **Pandas** and **Jupyter**. Also don't forget to install **xlrd**, this package is needed for **Pandas** to read ```.xlsx``` files. 

Finally, install **JupyterLab** and **JupyterHub** from the ```conda-forge``` channel.

```text
$ conda create -n jupyterhub python=3.7
$ conda activate jupyterhub
$(jupyterhub) conda install numpy matplotlib pandas xlrd jupyter notebook
$(jupyterhub) conda install -c conda-forge jupyterlab
$(jupyterhub) conda install -c conda-forge jupyterhub
```

<br>

## Run a very unsecured instance of Jupyter Hub just to see if it works

OK- let's give JupyterHub a whirl. We'll start JupterHub for the first time. Note the ```--no-ssl``` flag at the end of the command. This flag needs to be included or you won't be able to browse to the server. Also note we have to have our ```(jupyterhub)``` virtual environment active when we run the command. 

```text
$(jupyterhub) jupyterhub --no-ssl
```

We see some output in the PuTTY window. The last line is something like ```JupyterHub is now running at http://:8000/```. The first time I set up JupyterHub, I wasn't able to see the site using a web browser. No web page loaded, and the connection timed out.

![site can't be reached](images/site_cant_be_reached.png)

Why? It turns out Digital Ocean installs a firewall called **ufw** by default and turns the **ufw** firewall on. When the server was created, ufw was configured to only allow incoming connections on ports 22, 80 and 433. The output below is shown when we first log into the Digital Ocean server:

```text
"ufw" has been enabled. All ports except 22 (SSH), 80 (http) and 443 (https)
have been blocked by default.
```

But **JupyterHub runs on port 8000** - it tells us so when JupyterHub starts. So we need to configure **ufw** to allow connections on port 8000 (at least for now, just to see if JupyterHub works). 

To allow communication on port 8000 and start JupyterHub, type:

```text
$(jupyterhub) sudo ufw allow 8000
# make sure the (jupyterhub) conda env is activated

$(jupyterhub) jupyterhub --no-ssl
```

Now we can browse to the server IP address of our Digital Ocean Droplet appended with ```:8000```. The web address should look something like: ```http://165.228.68.178:8000```. You can find the IP address of the server by going into the Digital Ocean dashboard. 

The JupyterHub login screen looks like:

![jupyter hub no ssl login](images/jupyterhub_no_ssl_login.png)

Awesome! Quick log into JupyterHub using the username and password for the non-root sudo user (in my case ```peter```) that we set up earlier and are running as in our current PuTTY session. 

You should see the typical notebook file browser with all the files you can see when you run ```ls ~/```. Try creating and running a new Jupyter notebook. The notebook works just like a **Jupyter notebook running locally**.

![start my server](images/start_my_server.png)
![jupyter file browser](images/jupyter_file_browser.png)

<br>

## Quick! Log out and shut down JupyterHub

!!! warning
    <strong>Warning!</strong> You should not run JupyterHub without SSL encryption on a public network.

**Quick! Log out and shut down JupyterHub**. (does quick really matter in internet security?) The site is running without any ssl security over regular HTTP not HTTPS. Key in [Ctrl] + [c] to stop JupyterHub.

After you have confirmed that JupyterHub works, close off Port 8000 on the server by keying in the following command.

```text
$(jupyterhub) sudo ufw deny 8000
$(jupyterhub) sudo ufw status

Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
8000                       DENY        Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
8000 (v6)                  DENY        Anywhere (v6)
```

## Summary

In this section, we installed **Miniconda** onto the server. We made sure **Miniconda** was installed in the ```/opt``` directory and then changed the directory permissions so that our non-root sudo user can use the **Miniconda** installation. 

Next we created a Python 3.7 virtual environment and installed NumPy, Matplotlib, Pandas, xlrd, and Jupyter into it. Then we installed JupyterLab and JupyterHub into the virtual environment from the ```conda-forge``` channel. Finally we ran a very un-secure instance of JupyterHub with no SSL encryption. 

**Running JupyterHub without SSL encryption is NOT ADVISED**.

## Next Steps

The next step is to acquire a domain name and link the domain name to our Digital Ocean server.

<br>
