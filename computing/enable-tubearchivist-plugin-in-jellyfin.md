# Enable TubeArchivist plugin in Jellyfin

#selfhosting #jellyfin #tubearchivist 

The plugin was included by default in my version, but you can import it as a custom plugin if required.

```
https://github.com/tubearchivist/tubearchivist-jf-plugin/raw/master/manifest.json
```

Restart Jellyfin and configure the plugin.

Set the TubeArchivist URL using the HTTP route available on the internal network created by Docker.

```
http://tubearchivist:8000
```

Retrieve the API key from TubeArchivist and set it here too.

After creating libraries, edit their settings and ensure TubeArchivist is set as the top metadata provider where required.



