# Set Up and Tools

In this section, we'll review the tools we need to install locally get JupyterHub running on a remote sever.

[TOC]

Before we launch into the server setup, let's quick review where certain files are going to go on the JupyterHub remote server.

## File Locations and Directory Structure

According to the [JuptyerHub docs](https://jupyterhub.readthedocs.io/en/stable/installation-basics.html):

The folks at JupyterHub recommend that we put all of the files used by JupyterHub on the server into standard UNIX filesystem locations:

* ```/srv/jupyterhub``` for all security and runtime files
* ```/etc/jupyterhub``` for all configuration files
* ```/var/log```  for log files

## Development tools

### OpenSSL

Before we create the remote server, a set of private/public SSH keys
are needed. SSH keys can be created with OpenSSL which is commonly installed on any unix-like system.

### Python Editors

I use a couple of different Python code editors. My favorites are Emacs and [PyCharm](https://www.jetbrains.com/pycharm/), but  [Visual Studio Code](https://code.visualstudio.com/) is also a good choice. You can download and install Visual Studio Code [here](https://code.visualstudio.com/download). PyCharm has a community edition which is free, and a professional version which requires a license.

### Digital Ocean

This JupyterHub deployment runs on a Virtual Machine set up by the local "Rechnerbetriebsgruppe". Request a server with maximum CPUs and RAM running the latest Ubuntu LTS (18.04 was current at the time of writing this). Details on the registration process are available at [the VM wiki page](https://wiki.rbg.tum.de/Informatik/Benutzerwiki/LehrstuhlVM).

## Summary

JupyterHub has a set of standard file locations where we will put our configuration and runtime files.


## Next Steps

The next step is to create a public-private SSH key pair with openssl. We'll use this public-private SSH key to log into the server with ssh.
