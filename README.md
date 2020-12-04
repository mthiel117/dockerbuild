# Build Image

## Summary

Create a custom Docker container built on Centos that has Python and Ansible installed with modules and collections pre-installed to support Arista AVD and Netbox.

## Versions

- CentOS Linux release 8.2.2004 (Core)
- Python 3.6.8
- Ansible 2.9.6

## Ansible Collections & Python Modules to Support

- Arista AVD
- Netbox

## Default Shell

- Zsh

## Prerequisites

- Docker installed on local machine
- https://docs.docker.com/get-docker/
- GIT installed on local machine


## STEP 1 - Clone the repo
Create file called Dockerfile.centos and include contents below

```shell
git clone https://github.com/mthiel117/dockerbuild.git
```

## STEP 2 - Build Docker Image

```shell
docker build -f Dockerfile.centos --tag mycentosbox/base .
```

## STEP 3 - Run container and attach to local current directory

MacOS

```shell
docker run -it --rm -v $(PWD):/projects mycentosbox/base
```

Windows

```shell
docker run -it --rm -v %cd%:/projects mycentosbox/base
```
