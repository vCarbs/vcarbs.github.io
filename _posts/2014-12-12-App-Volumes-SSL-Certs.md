---
title: Replacing SSL Certificates for VMware App Volumes
date: 2014-12-12
tags: [App Volumes, VDI]     # TAG names should always be lowercase
---
![Desktop View](/assets/posts/app_volumes/1.png){: .normal}

VMware App Volumes recently released this week, and I have been fortunate enough to spend the past few days setting it up and deploying various AppStacks. Most of the AppStacks that have been deployed so far are 3D games, as we are simultaneously doing some testing with the Nvidia GRID K2 GPU’s. (More on that to come!) I want to focus this post on replacing the default SSL certificates that ship with App Volumes, since I could not find anything in the documentation about this process. If you have any other suggestions, or I missed something, I invite you to please reach out to me! Other then that, let’s get started.

For this example, I am using a wildcard cert that we use internally here at the company.

– Install the latest version of OpenSSL. The download can be found [here.](http://www.slproweb.com/products/Win32OpenSSL.html)

– Once we have OpenSSL installed, we need to navigate to the directory where our SSL cert is located. (I dropped mine in the OpenSSL folder.)

– The following command will extract the private key from our .pfx file:

`openssl pkcs12 -in [yourcert.pfx] -nocerts -out [yourcert.key]`

– We will be asked for the existing import password, and are required to enter in a new PEM pass phrase.

– This command will extract the certificate from the .pfx file:

`openssl pkcs12 -in [yourcert.pfx] -clcerts -nokeys -out [yourcert.crt]`

– Again, we will be asked for the existing import password.

– Finally, we need to convert our private key to PEM format:

`openssl rsa -in [keyfile-encrypted.key] -outform PEM -out [keyfile-encrypted-pem.key]`

– We are now asked for the pass phrase that we created in the first step.

– The output should look similar to this image:

![Desktop View](/assets/posts/app_volumes/2.png){: .normal}

Shout out to **Mark Brilman** for his post on converting a PFX! The post can be found [here.](https://www.markbrilman.nl/2011/08/howto-convert-a-pfx-to-a-seperate-key-crt-file/) Thanks, Mark!

Once we have converted our PFX cert to a certificate and keyfile, it is time to replace the default App Volumes SSL certificate.

– On our App Volumes server we need to navigate to the following path: C:\Program Files (x86)\CloudVolumes\Manager\nginx_proxy\conf

– We need to rename svserver.crt and svserver.key to svserver.crt.old and svserver.key.old

– Next, we need to copy our recently created [yourcert.crt] and [yourcert-pem.key] files to the server.

– Rename the certs to svserver.key and svserver.crt

– Now we need to perform the same steps in this directory: C:\Program Files (x86)\CloudVolumes\Manager\nginx\conf

– Once we have replaced the certificates in both locations, we need to run the following batch scripts, in order, located in C:\Program Files (x86)\CloudVolumes\Manager:


`nginx_proxy_stop.bat`

`nginx_stop.bat`

`nginx_proxy_start.bat`

`nginx_start.bat`


Navigating back to our App Volumes site using https, we should now see the certificate that we have uploaded:

![Desktop View](/assets/posts/app_volumes/3.png){: .normal}

As we can see, the process is fairly simple and doesn’t require too much time, as long as you have your cert and the private key =). Personally, I find that being prompted to accept the self signed certs annoying, and try to avoid it whenever I can. And, now that the certificates are replaced, I can get back to adding ridiculously addicting games like Star Racing into my AppStack, so that the very important and highly professional “stress testing” can resume.