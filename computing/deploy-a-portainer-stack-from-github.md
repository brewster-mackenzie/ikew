# Deploy a Portainer stack from GitHub

#portainer #docker #selfhosting #git

-----

1. Login to Portainer 
1. Select the target environment 
1. Navigate to the Stacks section
1. Select `Deploy a new stack`o
1. Populate the required fields

- Repository URL: copy from GitHub, e.g. `https://github.com/user/repo`
- Reference: for tags use `refs/tags/tag-name`
- Username: your GitHub username
- Password: a secure token from GitHub

Create any additional environment variables.

For multiple stacks that relay on the same environment variables, my recommendation is to create a `.env` file in the root foler of all the stacks, then create a symlink to this file (also named `.env` in each stack folder).  Portainer will detect `.env` by default and include it.

You can use the environment variables from `.env` in your stack, either by using the `env_file` option in Docker Compose, or by referencing the environment variables directly, e.g.:

```yaml
environment:
 - UUID=${SHARED_APP_UUID}
 - PORT=${SHARED_APP_PORT}
```


