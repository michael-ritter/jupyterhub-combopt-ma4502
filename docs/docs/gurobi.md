# Installing Gurobi

In this step, we are going to install Gurobi into our JupyterHub
environment. 

## Installing Gurobi on the server
For installing Gurobi on the server, first download Gurobi from the
Gurobi website at https://www.gurobi.com and unpack the archive in a
temporary directory. This will create a folder named ```gurobi811```or
similar, depending on the version you downloaded. We will install
gurobi into ```/opt```, so issue the following commands from your
temporary directory:

```
$ sudo mv gurobi811 /opt
$ cd /opt
$ ln -s gurobi811 gurobi
```

## Installing Gurobi into the Virtual Environment
To install Gurobi into the virtual environment, change into the
directory ```/opt/gurobi/linux64``` and issue the following command

```
/opt/gurobi/linux64$ /srv/jupyterhub/venv/bin/python setup.py install
```
This will make sure that the Gurobi modules are installed into the
correct environment.


## Configuring Environment Variables for Gurobi
Gurobi needs to set a few environment variables to work properly. We
will automate this by setting the appropriate variables in the
systemd configuration. Unfortunately, a little more work is required
so that these settings actually make their way into the Jupyter
Notebook / Lab process and can then be seen by Gurobi.

### Step 1: Include Variable Settings into systemd
Change ```/etc/systemd/system/jupyterhub.service``` to look like this:

```
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
PassEnvironment=GUROBI_HOME GRB_LICENSE_FILE LD_LIBRARY_PATH
ExecStart=/srv/jupyterhub/venv/bin/jupyterhub -f /etc/jupyterhub/jupyterhub_config.py

[Install]
WantedBy=multi-user.target
```

Note that we not only set the variables, we also pass them on using
the ```PassEnvironment``` directive. This is necessary, because
otherwise the variables would only be set for the command in
```ExecStart``` and then be unset again.
    
### Step 2: Pass Variable Settings to Jupyter Notebooks
While the variables are now visible for the jupyterhub process, they
are not automatically passed on to the separate notebook processes, so
the local Python interpreter will still not be able to see them. This
is a security measure, but for our purposes, passing the variables is
necessary, so we will add the following lines to the file ```/etc/jupyterhub/jupyterhub_config.py```:

```
# import all variables from os into jupyter processes
import os
for var in os.environ:
    c.Spawner.env_keep.append(var)
```


