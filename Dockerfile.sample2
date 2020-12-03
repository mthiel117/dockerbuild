FROM ubuntu

RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-get install -y --no-install-recommends build-essential gcc wget git
RUN wget --quiet https://raw.githubusercontent.com/mthiel117/reqs/main/requirements.txt

RUN apt-get -y install ansible

# Install Python3 & Modules
RUN apt-get -y install python3.8
RUN apt-get -y install python3-pip
RUN pip3 install --user -r requirements.txt
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install componets for NetBox
# RUN pip3 install pynetbox
RUN ansible-galaxy collection install netbox.netbox

# Create the /project directory and add it as a mountpoint
WORKDIR /projects
VOLUME ["/projects"]