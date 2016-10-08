# devbox-ubuntu-16

A Vagrant box with Ansible provisioner, based on Ubuntu 16.

Includes:

- system
- nginx
- php7-fpm
- mysql-official-latest
- composer
- wkhtmltox
- redis
- beanstalk
- supervisor
- curl, wget, git, e, mc

## Installation

Assuming you already have Vagrant and Virtualbox installed.

### Install Ansible

#### OSX
```
sudo easy_install pip && sudo pip install ansible && pip install --upgrade setuptools --user python
```

#### Ubuntu
```
sudo apt-add-repository -y ppa:ansible/ansible && sudo apt-get update && sudo apt-get install -y ansible
sudo mv /etc/ansible/hosts /etc/ansible/hosts.orig
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
- {name: "my-project", document_root: "/var/www/my-project/public"}
```
The entry above will give you a `my-project.dev` domain.

### MySQL

The credentials are `root | vagrant`.

# !!! Important. Until ansible is updated please run the following. On mysql 5.7 root@locahost has a different plugin for auth
```
sudo su

mysql -u root -p

# ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'vagrant';
```

Log in using the MySQL credentials above.

### xip.io
Each vhost that the provisioner creates will have a xip.io ServerAlias, so you can access your projects from other computers in the same LAN using `http://PROJECT_NAME.IP.xip.io/`.

Example: `http://my-project.192.168.0.12.xip.io/`.
### Other considerations

On a mac, if you use WiFi you may need to replace the bridged network interface (`vars.network.bridge`) with something else (`en0: Wi-Fi (AirPort)`), or you will be asked to choose one of the available interfaces when you `vagrant up`.

The box will be provisioned on the first `vagrant up`; later, use `vagrant provision` whenever the configuration (playbook, roles) is updated.

Additional software can be added to the provisioner by creating additional Ansible roles and specifying them in the `playbook.yml` file.