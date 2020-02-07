## devbox-ubuntu - osx only

A Vagrant box with Ansible provisioner

## Provision options

##### Box
- Ubuntu 16.04 LTS: ubuntu/xenial64
- Ubuntu 18.04 LTS: ubuntu/bionic64

##### Web server
- nginx
- apache2

##### PHP
- php5.6-fpm
- php7.0-fpm
- php7.1-fpm
- php7.2-fpm
- php7.3-fpm
- php7.4-fpm

##### Elasticsearch
- 1.7
- 6.4

## Includes
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
sudo easy_install pip && sudo pip install ansible && pip install --upgrade setuptools --user python
```
### Upgrade Ansible
```
sudo pip install --ignore-installed --upgrade ansible
```

### Clone this repository
While in your Projects folder,
```
git@gitlab.thesoftwarefarm.co.uk:thesoftwarefarm/devbox-ubuntu.git devbox
```

### Create a playbook from the given sample
```
cp playbook.yml.sample playbook.yml
```

Change the `IP` in your `playbook.yml` file; pick an IP that is unique in your LAN.

## Usage

##### Web server

Choose to install `nginx` or `apache2` by setting `system.web_server`

##### PHP version

Set the php version which you want to be installed by setting `system.php_version` 

##### Elasticsearch version

Set the elasticsearch version which you want to be installed by setting `system.elasticsearch_version`

##### Virtual hosts

Edit the `playbook.yml` file to specify vhosts. A vhost is described by a line like the following:

The `supervisor` option is optional. Add it if the project has queues implemented.

Multiple supervisor commands are supported.
```
- name: project_1
    server_name: "project_1.localdev api.project_1.localdev"
    document_root: "/var/www/html/project_1/public"
- name: project_2
    server_name: "project_2.localdev api.project_2.localdev"
    document_root: "/var/www/html/project_2/public"
    supervisor:
      project_root: "/var/www/html/project_2"
      commands:
        - name: queue
          command: php artisan queue:work --sleep=1 --tries=3 --daemon --queue=default
          numprocs: 3
          stdout_logfile: "/var/www/html/project_2/storage/logs/supervisor.log"
```

## Credentials

#### MySQL
User: `ubuntu`  
Password: `vagrant`
#### SSH 

Via private key located in `.vagrant/machines/default/virtualbox/private_key` 

User: `ubuntu`
```
cd PATH_TO_BOX
ssh ubuntu@VAGRANT_IP -i .vagrant/machines/default/virtualbox/private_key
```

### Other considerations

On a mac, if you use WiFi you may need to replace the bridged network interface (`vars.network.bridge`) with something else (`en0: Wi-Fi (AirPort)`), or you will be asked to choose one of the available interfaces when you `vagrant up`.

The box will be provisioned on the first `vagrant up`; later, use `vagrant provision` whenever the configuration (playbook, roles) is updated.

Additional software can be added to the provisioner by creating additional Ansible roles and specifying them in the `playbook.yml` file.

## Credit
All thanks should be sent to @ep