# Restore a Docker volume from a local tar file

Use a temporary container to restore files from a local tar file into a Docker volume

Works with backups created by following:
[[backup-a-docker-volume-to-a-local-tar-file]]

```bash
docker run --rm \
-v <volume>:/data -v $(pwd):/backup \
ubuntu tar xvf /backup/backup.tar -C /data --strip 1
```

- the `--rm` switch will clean up the container when the command completes
- replace `<volume>` with the Docker volume name
- replace `ubuntu` with another distribution if you want

## Example

```bash
docker run --rm \
-v paperless_media:/data -v $(pwd):/backup \
ubuntu tar xvf /backup/backup.tar -C /data --strip 1
```
