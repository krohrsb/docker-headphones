# Docker for headphones 

This is the Dockerfile setup for [headphones](https://github.com/rembo10/headphones).

## Building (optional)
To build the image locally:

```bash
git clone https://github.com/blackbarn/docker-headphones.git;
cd docker-headphones;
docker build -t headphones .
```

## Running

```bash
docker run -d -v /your_data_location:/data -v /your_music_dir:/volumes/music -v /any_other:/volumes/other -p "8181:8181" --name headphones headphones
```

Or you can replace `headphones` with `blackbarn/headphones` in your `run` command to use the pre-built image from docker hub.

```bash
docker run -d -v /your_data_location:/data -v /your_music_dir:/volumes/music -v /any_other:/volumes/other -p "8181:8181" --name headphones blackbarn/headphones
```

Change the port mapping to suit your needs. Also, aside from the `:/data` volume, mount any other directories you may need to reference via headphones config. Such as music download directory or torrent directories. 
Just remember, within headphones settings you refer to them by their locally mounted path, such as `/volumes/music`.

## Compose

An example of a `docker-compose.yml` file:

```yml
web:
  build: .
  container_name: headphones
  ports:
    - "8181:8181"
  volumes:
    - /opt/data/headphones/data:/data
    - /media:/media
  external_links:
    - nzbget:nzbget-docker
  restart: always
```

Obviously your volume mapping will vary. The `external_links` is essentially defining a `--link` to another container, in this case `nzbget`. This way you can reference it by name within the container.
