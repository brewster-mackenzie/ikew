# OAuth2Proxy site configuration

#selfhosting #oauth2 #oauth2proxy #sso 

-----

Firstly, create a site for the oauth2proxy server, e.g.:

```nginx
server {
    server_name auth.domain.com;

    root /var/www/html;

    location ~ ^/oauth2/.+$ {
        proxy_pass http://localhost:7003; # PORTS_OAUTH2PROXY
        proxy_set_header Host auth.27hs.co.uk;
        proxy_buffer_size 128k;
        proxy_buffers 4 256k;
        proxy_busy_buffers_size 256k;
    }
}
```

I like to have my SSO identity provider (e.g. Keycloak, PocketID) in the same sub-domain, which you can achieve by adding a `location /` block, as usual.

Then configure the site for which you want to use oauth2proxy authentication.

The proxy header `X-Auth-User` can be changed to use whichever header is required by your application.

```nginx
server {
    server_name auth.domain.com;

    root /var/www/html;

    location / {
        auth_request /oauth2/auth;
        auth_request_set $user $upstream_http_x_auth_request_preferred_username;

        add_header X-Auth-User $user;
        error_page 401 @error401;

        proxy_pass http://localhost:7007;
        proxy_set_header HOST $host;
        proxy_set_header X-Auth-User $user;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location @error401 {
        return 302 https://auth.domain.com/oauth2/start?rd=https://$host$uri;
    }

    location = /oauth2/auth {
        proxy_pass http://localhost:7003; # PORTS_OAUTH2PROXY
    }
}
```
