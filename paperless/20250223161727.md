# Migrate Paperless-ngx to a new Docker container

Connect to the container terminal

```bash
docker exec -it paperless /bin/bash
```

Create a folder for the export

```bash
mkdir data/export
```

Run the built-in document exporter 

```bash
run document_exporter ./data/export
```

And then exit the terminal

```bash
exit
```

Create a local folder for the exported data

```bash
mkdir ./export
```

Copy the exported data from the Paperless-ngx volume to the local export folder

```bash
docker run --rm -v ./export:/export -v paperless-data:/data alpine cp -R /data/export /export 
```

Do the reverse to copy data to the new Paperless-ngx volume 

Use the built-in `document_importer` to import the files to the new instance.
