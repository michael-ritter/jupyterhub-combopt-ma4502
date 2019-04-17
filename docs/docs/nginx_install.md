# Install Nginx

Now that the server is set up, the next step is to install Nginx and modify our ufw firewall settings.

[TOC]

Nginx is an open-source web server that can handle many concurrent web connections at the same time. 

Use terminal and ssh to connect to the server with the non-root sudo user ```ritter```. Once logged in, we can update the system and install Nginx.

```text
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install nginx
```

## Add the Nginx app to ufw

Earlier, we have configured the firewall **ufw**. Check out which apps the ufw firewall can work with:

```text
$ sudo ufw app list
```

We see a list of available ufw configurations to work with Nginx:

```text
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

We want to allow in both http and https requests. Once a http request comes in, we'll use Nginx to convert the http connection to a https connection. 

Select ```'Nginx Full'```. Note the **C**apitalization in the command:

```text
$ sudo ufw allow 'Nginx Full'
```

We can check out which ports ufw is allowing through with:

```text
$ sudo ufw status
```

Note the output shows ufw allows Nginx Full. We opened port 8000 earlier, so we could see how JupyterHub works without a domain name or SSL.  Once we get Nginx running and hooked up to JupyterHub, we need to remember to close off port 8000 in ufw. We also need to make sure the Port 80 is open to tcp traffic. If Nginx doesn't seem to be working correctly, it may be because ufw isn't allowing traffic in. In that case try

```text
$ sudo ufw allow 80/tcp
```

Nginx will start running as soon at it is installed. We can see the status with:

```text
$ sudo systemctl status nginx
```

In the output, we should see something like below. This mean Nginx is running. Key in [ctrl-c] to exit the status dashboard.

```text
Active: active (running) since Thu 2018-05-17 04:51:16 UTC; 15min ago
Main PID: 17126 (nginx)
  CGroup: /system.slice/nginx.service
    ├── 17126 nginx: master process /usr/sbin/nginx -g daemon on; master_pr
    └── 17127 nginx: worker process
```

Now we can browse over to the domain m09vm14.ma.tum.de and see the Nginx start page.

![nginx welcome page](images/welcome_to_nginx.png)

## Summary

If this section we installed Nginx on the server and confirmed that it is activate and running. We also changed the rules of our ufw firewall to allow Nginx Full traffic access. If it doesn't seem like Nginx is working right, it might be because ufw is not allowing traffic in over Port 80. Opening Port 80 may solve the problem.

## Next Steps

Now that Nginx is installed, the next step is to configure Nginx to obtain an SSL certificate from Let's Encrypt using certbot and run as a reverse proxy for our JupyterHub server.
