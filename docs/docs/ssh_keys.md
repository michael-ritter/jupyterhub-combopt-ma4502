# Create SSH keys

SSH keys allow us to log into the virtual machine server which will run JupyterHub. SSH keys come in pairs, a private key and a public key. The public key will be stored on the JupyterHub cloud server and the private key will stay on our local machine.

[TOC]

## Create SSH keys through openssl

Open a terminal and enter the following command:

```
cd ~/.ssh
ssh-keygen -b4096 -trsa -f jupyterhub_rsa
```

We are using the following parameters:

 * Type of key to generate: RSA
 * Number of bits in generated key: 4096
 * Filename for the keys to be generated: ```jupyterhub_rsa``` (private
   key) and ```jupyterhub_rsa.pub``` (public key)


We need the public key contents available to paste into the server's SSH ```authorized_keys``` file, so copy the contents of that file into the clipboard. Copy all of the contents of the public SSH key including the ```ssh-rsa``` line.

## Summary

After completing these steps, we have a public and private SSH key pair saved in ```~/.ssh/jupyterhub_rsa```. We also have the contents of the public SSH key saved to the clipboard.

## Next Steps

Next, we'll install the SSH keys we just created on our virtual machine and use the private key to log into the server.

<br>
