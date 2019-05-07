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
port_forward = playbook_vars['vagrantfile']['port_forward']

Vagrant.configure(2) do |config|

  config.vm.box = box
  config.vm.hostname = hostname
  config.vm.network "public_network", ip: public_ip, bridge: network_bridge
  config.vm.network "private_network", ip: private_ip

  if port_forward
    port_forward.each do |port_guest, port_host|
      config.vm.network "forwarded_port", guest: port_guest, host: port_host, auto_correct: true
    end
  end

  config.vm.synced_folder synced_folder_source, synced_folder_target, type: "nfs", mount_options: ['rw', 'vers=3', 'tcp', 'fsc','actimeo=2']

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.name = hostname
    vb.memory = vb_memory
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end

  # ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = playbook
    ansible.limit = "all"
    ansible.verbose = ansible_verbose
    ansible.extra_vars = { ansible_python_interpreter: "python3" }
  end
end
