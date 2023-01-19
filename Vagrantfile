# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
require 'getoptlong'

opts = GetoptLong.new(
  [ '--tags', GetoptLong::OPTIONAL_ARGUMENT ]
)

opts.ordering=(GetoptLong::REQUIRE_ORDER)

ansible_tags=''

opts.each do |opt, arg|
  case opt
    when '--tags'
      ansible_tags=arg
  end
end

# read vars from playbook
playbook = File.dirname(__FILE__) + "/playbook.yml"
vars = YAML.load_file(playbook)[0]['vars']['vagrantfile']

Vagrant.configure(2) do |config|

  config.vm.box = vars['box']
  config.vm.hostname = vars['hostname']
  config.vm.network "public_network", ip: vars['network']['public_ip'], bridge: vars['network']['bridge']
  config.vm.network "private_network", ip: vars['network']['private_ip']

  config.ssh.forward_agent = true

  if vars['port_forward']
    vars['port_forward'].each do |port_guest, port_host|
      config.vm.network "forwarded_port", guest: port_guest, host: port_host, auto_correct: true
    end
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder vars['synced_folder']['source'], vars['synced_folder']['target'], 
    type: "nfs", 
    nfs_udp: false, 
    nfs_version: 3, 
    mount_options: ['rw', 'tcp', 'fsc','actimeo=2', 'wsize=65536','rsize=65536']

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.name = vars['hostname']
    vb.memory = vars['memory']
    vb.cpus = 1
    vb.customize [ "modifyvm", :id, "--uartmode1", "file", File::NULL ]
  end

  # ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = playbook
    ansible.limit = "all"
    ansible.verbose = "vv"
    ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }

    if ansible_tags != ''
        ansible.tags = ansible_tags
    end
  end
end
