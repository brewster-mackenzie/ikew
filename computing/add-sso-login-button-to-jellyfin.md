# Add SSO login button to Jellyfin

#jellyfin #self-hosting #sso 

------

In settings add a login disclaimer using the HTML below

Change the server URL and provider as required.

```html
<form action="https://jellyfin.example.com/sso/OID/start/PROVIDER_NAME">
  <button class="raised block emby-button button-submit">
    Sign in with SSO
  </button>
</form>
```

Add custom CSS to display the button correctly

```css
a.raised.emby-button {
  padding: 0.9em 1em;
  color: inherit !important;
}

.disclaimerContainer {
  display: block;
}
```
