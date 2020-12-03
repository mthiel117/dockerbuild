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


## Create Dockerfile.centos in local directory
include contents below

```shell
FROM centos

RUN dnf update -y
RUN dnf install python3 -y
RUN ln -s /usr/bin/python3 /usr/bin/python
RUN dnf install openssh-clients -y
RUN dnf install which -y
RUN dnf makecache
RUN dnf install epel-release -y
RUN dnf makecache
RUN dnf install ansible -y
RUN dnf install wget -y
RUN dnf install git -y

# AVD Reqs....
RUN wget --quiet https://raw.githubusercontent.com/aristanetworks/ansible-avd/devel/development/requirements.txt
RUN pip3 install --user --trusted-host files.pythonhosted.org --trusted-host pypi.org --trusted-host pypi.python.org -r requirements.txt
RUN wget --quiet https://raw.githubusercontent.com/aristanetworks/ansible-avd/devel/development/requirements-dev.txt
RUN pip3 install --user --trusted-host files.pythonhosted.org --trusted-host pypi.org --trusted-host pypi.python.org -r requirements-dev.txt

# Netbox collections and module
RUN ansible-galaxy collection install netbox.netbox
RUN pip3 install --user --trusted-host files.pythonhosted.org --trusted-host pypi.org --trusted-host pypi.python.org pynetbox

# ZSH
RUN dnf install util-linux-user -y
RUN dnf install zsh -y
RUN wget --quiet https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true \
    && echo 'plugins=(ansible common-aliases safe-paste git jsontools history git-extras)' >> $HOME/.zshrc \
    && echo 'eval `ssh-agent -s`' >> $HOME/.zshrc \
    && echo 'export TERM=xterm' >>  $HOME/.zshrc

# Create the /project directory and add it as a mountpoint
WORKDIR /projects
VOLUME ["/projects"]

# Use ZSH as default shell with default oh-my-zsh theme
ENV PATH=$PATH:/root/.local/bin
CMD [ "zsh" ]
```

## Build Docker Image and give it a tagname

```shell
docker build -f Dockerfile.centos --tag mycentosbox/base .
```

## Run container and attach local volume ($PWD)

MacOS

```shell
docker run -it --rm -v $(PWD):/projects mycentosbox/base
```

Windows

```shell
docker run -it --rm -v %cd%:/projects mycentosbox/base
```
