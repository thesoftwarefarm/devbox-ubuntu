# devbox-ubuntu-14

A Vagrant box with Ansible provisioner, based on Ubuntu 14.

Includes:

- Apache 2.4.7, PHP 5.5.9, MySQL 5.5.49
- phpMyAdmin
- automatic backup of local databases on `vagrant halt` using automysqlbackup; the backups are stored in the `.database-backups` folder
- various tools like `curl`, `wget`, `git`, `mc`, `git`, `bash-completion`, `mcrypt`, `xdebug`, `phpunit`, `composer`
- redis
- wkhtmltox

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
git clone git@gitlab.thesoftwarefarm.co.uk:thesoftwarefarm/devbox-ubuntu-14.git .
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

### phpMyAdmin
phpMyAdmin can be accessed by adding `/phpmyadmin` after any domain you have configured.

Example: `http://my-project.dev/phpmyadmin/`

Log in using the MySQL credentials above.

### xip.io
Each vhost that the provisioner creates will have a xip.io ServerAlias, so you can access your projects from other computers in the same LAN using `http://PROJECT_NAME.IP.xip.io/`.

Example: `http://my-project.192.168.0.12.xip.io/`.

### Using NFS
In the playbook.yml file, `vagrantfile.use_nfs` controls whether NFS should be used when syncing the synced folder.

### Other considerations

On a mac, if you use WiFi you may need to replace the bridged network interface (`vars.network.bridge`) with something else (`en0: Wi-Fi (AirPort)`), or you will be asked to choose one of the available interfaces when you `vagrant up`.

The box will be provisioned on the first `vagrant up`; later, use `vagrant provision` whenever the configuration (playbook, roles) is updated.

Additional software can be added to the provisioner by creating additional Ansible roles and specifying them in the `playbook.yml` file.