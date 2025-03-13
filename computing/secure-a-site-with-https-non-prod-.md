# Secure a site with HTTPS (non-prod)

#selfhosting #web #security 

-----

> [!warning] NON-PRODUCTION USE ONLY

The simplest method to ensure that the necessary headers etc. are added to the nginx configuration and that a valid certificate exists is to use CertBot.

Install as follows:

```bash
# Ubuntu
sudo apt-get install certbot

# ArchLinux
sudo pacman -Syyu certbot 
```

Then generate the certificate for the specific domain/location.

```bash
sudo certbot -d site.domain.com
```

If DNS resolves to the machine invoking this command (and you haven't reached your limit for new certificates) then you should receive a success message.  Check the nginx config to see the new certificate has been applied.


