# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

# read vars from playbook
playbook = File.dirname(__FILE__) + "/playbook.yml"
vars = YAML.load_file(playbook)[0]['vars']['vagrantfile']

Vagrant.configure(2) do |config|

  config.vm.box = vars['box']
  config.vm.hostname = vars['hostname']
  config.vm.network "public_network", ip: vars['network']['public_ip'], bridge: vars['network']['bridge']
  config.vm.network "private_network", ip: vars['network']['private_ip']

  if vars['port_forward']
    vars['port_forward'].each do |port_guest, port_host|
      config.vm.network "forwarded_port", guest: port_guest, host: port_host, auto_correct: true
    end
  end

  config.vm.synced_folder vars['synced_folder']['source'], vars['synced_folder']['target'], type: "nfs", mount_options: ['rw', 'vers=3', 'tcp', 'fsc','actimeo=2']

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.name = vars['hostname']
    vb.memory = vars['memory']
    vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end

  # ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = playbook
    ansible.limit = "all"
    ansible.verbose = "vv"
    ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
  end
end
