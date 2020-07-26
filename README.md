# RB4
Raspberry 4 with ubuntu 64 setup &amp; config


## INSTALLATION Ubuntu 64 bit
### default user/password: ubuntu/ubuntu
### set german keyboard
sudo dpkg-reconfigure keyboard-configuration

### setting static IP

edit /etc/netplan/50-cloud-init.yaml
```bash
    # This file is generated from information provided by
    # the datasource.  Changes to it will not persist across an instance.
    # To disable cloud-init's network configuration capabilities, write a file
    # /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
    # network: {config: disabled}
    network:
        ethernets:
            enp0s3:
                dhcp4: false
                addresses: [192.168.1.202/24]
                gateway4: 192.168.1.1
                nameservers:
                addresses: [8.8.8.8,8.8.4.4,192.168.1.1]
        version: 2
```


### add a new user 

```bash

sudo adduser franzn
sudo usermod -aG franzn
sudo usermod -aG sudo  franzn
```


### install tailscale
```bash

curl https://pkgs.tailscale.com/stable/ubuntu/focal.gpg | sudo apt-key add -
curl https://pkgs.tailscale.com/stable/ubuntu/focal.list | sudo tee /etc/apt/sources.list.d/tailscale.list

sudo apt update
sudo apt install tailscale
sudo tailscale up
```


## Installing docker on Ubuntu 

```bash
sudo apt install docker.io

add user to docker-group in /etc/group

```

## Fan 
Download the script for Ubuntu

git clone https://github.com/meuter/argon-one-case-ubuntu-20.04.git

run argon1.sh

## Install Portainer

    docker volume create portainer_data
    docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer>




### ssh login without password

use ssh-copy-id, e.g.
ssh-copy-id rhost1

# Argon-Case utilities
git clone https://github.com/meuter/argon-one-case-ubuntu-20.04.git
cd argon-one-case-ubuntu-20.04/
./argon1.sh
argonone-tempmon
argon
apropos argon


## Install k3s and Portainer beta for Kubernetes

    Default:
    
    curl -sfL https://get.k3s.io | sh -
    
    For Portainer use:
    
    curl -sfL https://get.k3s.io | sh -s - server --no-deploy servicelb
 
    Install Portainer-Beta:
    
    curl -LO https://raw.githubusercontent.com/portainer/portainer-k8s/master/portainer-nodeport.yaml
    kubectl apply -f portainer-nodeport.yaml


## INSTALLATION Raspberry PI 3 B

### Download and write to SSD Raspberry Pi OS (32- bit) Lite - https://www.raspberrypi.org/downloads/raspberry-pi-os/

### German Keyboard  other Configs (enable ssh)
sudo raspi-config

### Set static IP
edit /etc/dhcpcd.config:
```bash
    interface eth0
    static ip_address=192.168.0.4/24
    static routers=192.168.0.1
    static domain_name_servers=192.168.0.1
```


    See: https://howtoraspberrypi.com/how-to-raspberry-pi-headless-setup/


### network device name setting
sudo touch /etc/udev/rules.d/73-usb-net-by-mac.rules

