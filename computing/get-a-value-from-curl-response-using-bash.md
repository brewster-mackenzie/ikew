# Get a curl response header value using bash

#bash #curl #awk #grep

-----

Use `curl -Is <url>` to get the headers only in 'silent' mode

Combine with `grep -i <header>` to match the desired header

Finally, use `awk '{print $2}'` to extract the second field, containing the value

```bash
curl -Is https://example.com | grep -i location | awk '{print $2}'
```

