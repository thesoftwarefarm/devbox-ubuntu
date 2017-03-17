# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# read vars from playbook
playbook        = File.dirname(__FILE__) + "/playbook.yml"
playbook_vars   = YAML.load_file(playbook)[0]["vars"]
public_ip       = playbook_vars['system']['public_ip']
private_ip      = playbook_vars['system']['private_ip']
box             = playbook_vars['vagrantfile']['box']
use_nfs         = playbook_vars['vagrantfile']['use_nfs']
hostname        = playbook_vars['vagrantfile']['hostname']
network_bridge  = playbook_vars['vagrantfile']['network']['bridge']
vb_memory       = playbook_vars['vagrantfile']['provider']['vb']['memory']
ansible_verbose = playbook_vars['vagrantfile']['provision']['ansible']['verbose']
synced_folder_source = playbook_vars['vagrantfile']['synced_folder']['source']
synced_folder_target = playbook_vars['vagrantfile']['synced_folder']['target']

http_port_forward_guest = playbook_vars['vagrantfile']['http_port_forward']['quest']
http_port_forward_host = playbook_vars['vagrantfile']['http_port_forward']['host']

Vagrant.configure(2) do |config|

  config.vm.box = box

  config.vm.hostname = hostname

  config.vm.network "forwarded_port", guest: http_port_forward_guest, host: http_port_forward_host

  config.vm.network "public_network", ip: public_ip, bridge: network_bridge

  config.vm.network "private_network", ip: private_ip

  config.vm.synced_folder synced_folder_source, synced_folder_target, type: "nfs", mount_options: ['rw', 'vers=3', 'tcp', 'fsc','actimeo=2']

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.memory = vb_memory
  end

  # ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = playbook
    ansible.sudo = true
    ansible.limit = "all"
    ansible.verbose = ansible_verbose
  end
end
