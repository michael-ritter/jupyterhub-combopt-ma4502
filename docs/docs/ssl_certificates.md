# Obtain SSL Certificates

Now that nginx is running on our JupyterHub server, we'll be able to obtain an SSL certificate and run JupyterHub over https. I followed the instructions on [https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx](https://certbot.eff.org/lets-encrypt/ubuntubionic-nginx) to install **certbot**, a program used to generate SSL certificates from [Let's Encrypt](https://letsencrypt.org).

[TOC]

## Install certbot

We'll use **certbot** to obtain a standalone SSL certificate. Log onto the server with ssh in a terminal window.

Certbot will need to communicate over the network, so before we run certbot, we need to open up Port 80 using the ufw firewall utility (if you have not done so already).

```text
$ sudo ufw allow 80
```

First, we will install certbot from the PPA maintained by Let's Encrypt:

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository universe
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt update
$ sudo apt install certbot python-certbot-nginx 
```

# Run certbot

Next, we run certbot to obtain a certificate and automatically change the nginx configuration to use that certificate:

```bash
$ sudo certbot --nginx
```

The script will ask you for your email address so that you may receive renewal and security notices:

```text
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Enter email address (used for urgent renewal and security notices) (Enter 'c' to
cancel): 
```

After that, you need to agree to the terms of service:

```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: a
```

You may then optionally choose to share your email address with the [Electronic Frontier Foundation (EFF)](https://www.eff.org), the organization that develops certbot. If you wish to not receive any email from the EFF, opting out here is a safe choice, no functionality will be lost.

As we have not configured our nginx installation yet, certbot will ask for the name of our server. Enter the appropriate information:

```text
No names were found in your configuration files. Please enter in your domain
name(s) (comma and/or space separated)  (Enter 'c' to cancel): m09vm14.ma.tum.de
```

After getting and deploying the certificate, you may choose to switch the nginx configuration to https only. This is a safe choice, we would have to do it later anyways:

```text
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Redirecting all traffic on port 80 to ssl in /etc/nginx/sites-enabled/default

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://m09vm14.ma.tum.de

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=m09vm14.ma.tum.de
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Finally, certbot gives us some details on the location of our certificate and the expiration date.

```text
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/m09vm14.ma.tum.de/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/m09vm14.ma.tum.de/privkey.pem
   Your cert will expire on 2019-07-16. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```
## Test the nginx Installation with HTTPS

Let us give the https installation a quick test run: Enter the url [http://m09vm14.ma.tum.de](http://m09vm14.ma.tum.de) into your browser's address bar. (Note the http, no S.) You should immediately be redirected to the https version of the site (check the address bar!) and see the same generic greeting as before.

![nginx welcome page](images/welcome_to_nginx.png)

## Summary

In this section, we installed Certbot on the server and ran Cerbot to obtain an SSL certificate. One gotcha to remember is that Port 80 must be open for Certbot to obtain the certificate. After we obtained our SSL certificate, we let Certbot handle the necessary nginx configuration. Finally, we made sure that we can now connect via https and that http connect are re-routed to https immediately.

## Next Steps

The next step is to create a cookie secret, proxy auth token, and dhparem.pem file.
