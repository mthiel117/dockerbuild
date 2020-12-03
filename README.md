# Build Image

## Edit Dockerfile

```shell
vi Dockerfile
```

## Build Docker Image and give it a tagname

```shell
docker build --tag cispteam/base .
```

## Run container and attach local volume ($PWD)

```shell
docker run -it --rm -v $(PWD):/projects cispteam/base
```