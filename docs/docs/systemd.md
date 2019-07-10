# Run JupyterHub as a system service

Running JupyerHub as a system service allows JupyterHub to run continuously even if we are not logged into the server. It also keeps JupyterHub running while when we log into the server and make any changes.

[TOC]

To run JupyterHub as a system service (according to [this wiki](https://github.com/jupyterhub/jupyterhub/wiki/Run-jupyterhub-as-a-system-service)
), we need to create a service file in the ```/etc/systemd/system``` directory. ```cd``` into the directory and have a look around. We see a couple files that end in ```.service```.

```text
$ cd /etc/systemd/system
$ ls
cloud-init.target.wants                network-online.target.wants
dbus-org.freedesktop.thermald.service  paths.target.wants
default.target.wants                   sockets.target.wants
final.target.wants                     sshd.service
getty.target.wants                     sysinit.target.wants
graphical.target.wants                 syslog.service
iscsi.service                          timers.target.wants
multi-user.target.wants
```

Create a new ```.service``` file called ```jupyterhub.service```

```text
pwd
/etc/systemd/system
$ sudo nano jupyterhub.service
```

In the ```jupyterhub.service``` file, add the following. Note that as
part of the ```PATH``` environment variable
```/srv/jupyterhub/venv/bin/``` is included. This is the path to our virtual environment.  Optionally, you may also
include environment variables for MIP solvers such as gurobi or
xpress. In the following example, we included the necessary settings
for a gurobi set up at ```/opt/gurobi```. As part of the ```ExecStart=
``` section, we include a flag for our JupyterHub config file located
at  ```/etc/jupyterhub/jupyterhub_config.py```.  The example also
includes a setting for GitLab that will only become relevant later.
Feel free to not include that for now, especially if you do not plan
on using GitLab for user authentication.

```text
[Unit]
Description=JupyterHub
After=syslog.target network.target

[Service]
User=root
Environment="PATH=/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/srv/jupyterhub/venv/bin/:/opt/gurobi/linux64/bin"
Environment="GUROBI_HOME=/opt/gurobi/linux64"
Environment="GRB_LICENSE_FILE=/opt/gurobi/linux64/bin/gurobi.lic"
Environment="LD_LIBRARY_PATH=/opt/gurobi/linux64/lib"
Environment="GITLAB_HOST=https://gitlab.lrz.de"
ExecStart=/srv/jupyterhub/venv/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

Save and exit the nano text editor with [Ctrl]+[x] and [y] then [Enter]. 

Now we need to reload the system daemon. After the system daemon is reloaded, we can run JupyterHub as a system service using the command: ```sudo systemctl <start|stop|status> jupyterhub```

```text
$ sudo systemctl daemon-reload
$ sudo systemctl start jupyterhub
```

We can see if JupyterHub is running with:

```text
$ sudo systemctl status jupyterhub

 Loaded: loaded (/etc/systemd/system/jupyterhub.service; 
 Active: active (running)
```

## Test local Authentication

Now we can point a web browser at our domain name and log into JupyterHub as our non-root user ```ritter``` and the password we set for ```ritter``` on the server. This time we are running with SSL security in place and even if we browse to ```http://m09vm14.ma.tum.de```, Nginx will forward us to ```https://m09vm14.ma.tum.de```.

The JupyterHub login screen looks something like the screen capture below:

![JupyterHub PAM Login](images/jupyterhub_pam_spawner_login.png)

A couple times I thought that JupyterHub was running after using ```systemctl start jupyterhub```, but the JupyterHub wasn't working when I went to the server's web address. It turned out that JupyterHub wasn't running when I keyed in ```systemctl status jupyterhub```. Most times looking for an error and tracking down the the error worked, but one time it seemed to be a problem with the http-configurable-proxy. 

The following command will shut down the proxy if you get stuck like I did (insert the number corresponding to the configurable-http-proxy process after the ```kill``` command):

```text
$ ps aux | grep configurable-http-proxy
$ kill #### 
```

## Summary

In this section, we got JupyterHub running as a system service. We created a ```jupyterhub.service``` file in the ```/etc/systemd/system/``` directory and made sure to include the PATH to our ```(jupyterhub)``` virtual environment and the path to our ```jupyterhub_config.py``` file. Finally, we reloaded the system service daemon and started the JupyterHub service. Then be opened a web browser and keyed in our domain name and logged into JupyterHub. 

## Next Steps

The next step is to add users to our server and see if we can log in as a different user than our non-root sudo user, ```ritter```.
