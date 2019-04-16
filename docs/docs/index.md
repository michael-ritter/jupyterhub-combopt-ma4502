# Welcome to the JupyterHub Deployment Docs for Combinatorial
Optimization 2019-S

<br>

This documentation serves as a record of the [JupyterHub](https://jupyter.org/hub) Deployment for MA4502 Combinatorial Optimization Summer 2019 at Technische Universität München. It is based on the [GitHub repository](https://github.com/ProfessorKazarinoff/jupyterhub-engr101) for a similar installation of Peter D. Kazarinoff for Portland Community College.

<br>

The GitHub repo for the deployment can be found here: 

 > 
[https://github.com/michael-ritter/jupyterhub-combopt-ma4502.git](https://github.com/michael-ritter/jupyterhub-combopt-ma4502.git)
<br>

Click the menu items on the left to view the deployment steps.

Or start [Here](what_is_jupyterhub.md) and click the arrows at the bottom of each page.

[![Next Setup Arrow](images/next_what_is_jupyterhub_arrow.png)](what_is_jupyterhub.md)

<br>

## Main Steps

* Generate SSH keys
* Request virtual machine with non-root sudo user
* Install JupyterHub and Python packages
* Aquire SSL cert
* Create Cookie Secret, Proxy Auth Token, and dhparam.pem
* Install and configure Nginx
* Configure JupyterHub
* JupyterHub as system service
* optional: Google Authentication
* optional: Create custom login page
* Pull assignments down from GitHub for each user
* Extra configuration
