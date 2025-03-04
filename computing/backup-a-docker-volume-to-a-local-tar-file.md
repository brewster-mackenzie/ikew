# Backup a Docker volume to a local tar file

#docker #backup #tar #bash

-----

Use a temporary container to backup data from a Docker volume to a local tar file

```bash
docker run --rm \
-v <volume>:/data -v $(pwd):/backup \
ubuntu tar cvf /backup/backup.tar /data
```

- the `--rm` switch will clean up the container when the command completes
- replace `<volume>` with the Docker volume name
- replace `ubuntu` with another distribution if you want

## Example

```bash
docker run --rm \
-v paperless_media:/data -v $(pwd):/backup \
ubuntu tar cvf /backup/backup.tar /data
```

Restore the backup by following: [[restore-a-docker-volume-from-a-local-tar-file]]
