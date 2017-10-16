# devbox-ubuntu-16 - osx only

A Vagrant box with Ansible provisioner, based on Ubuntu 16.04 Xenial

Includes:
- nginx
- php7-fpm
- mysql-official-latest
- composer
- wkhtmltox
- redis
- beanstalk
- supervisor
- various system tools: curl, wget, git, mc etc.

## Installation

##### Vagrant - [Download](https://www.vagrantup.com/downloads.html)
##### VirtualBox - [Download](https://www.virtualbox.org/wiki/Downloads)

### Install Ansible
```
sudo easy_install pip && sudo pip install ansible && passlib && pip install --upgrade setuptools --user python
```
### Upgrade Ansible
```
sudo pip install --ignore-installed --upgrade ansible
```

### Clone this repository
While in your Projects folder,
```
git@gitlab.thesoftwarefarm.co.uk:thesoftwarefarm/devbox-ubuntu-16.git ubuntu16
```

### Create a playbook from the given sample
```
cp playbook.yml.sample playbook.yml
```

Change the `IP` in your `playbook.yml` file; pick an IP that is unique in your LAN.

## Usage

### Virtual hosts

Edit the `playbook.yml` file to specify vhosts. A vhost is described by a line like the following:
```
- {name: "my-project", server_name: "my-project.dev admin.my-project.dev" document_root: "/var/www/my-project/public"}
```

## Credentials

#### MySQL
user: `ubuntu`  
password: `vagrant`
#### SSH 
user: `ubuntu`  
password: `vagrant`


### Other considerations

On a mac, if you use WiFi you may need to replace the bridged network interface (`vars.network.bridge`) with something else (`en0: Wi-Fi (AirPort)`), or you will be asked to choose one of the available interfaces when you `vagrant up`.

The box will be provisioned on the first `vagrant up`; later, use `vagrant provision` whenever the configuration (playbook, roles) is updated.

Additional software can be added to the provisioner by creating additional Ansible roles and specifying them in the `playbook.yml` file.

## Credit
All thanks should be sent to @ep