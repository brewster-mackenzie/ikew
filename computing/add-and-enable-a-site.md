# Add and enable a site

#nginx #hosting #self-hosting #ssl

-----

Copy the site configuration to the available sites folder

```bash
sudo cp site.conf /etc/nginx/sites-available/site.conf
```

Then symlink the site configuration in the enabled sites folder

```bash
sudo ln -s /etc/nginx/sites-available/site.conf /etc/nginx/sites-enabled/site.conf
```


