# -*- mode: ruby -*-
# vi: set ft=ruby :                                 

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # General Vagrant VM configuration.
  config.vm.box = "luminositylabsllc/bento-ubuntu-20.04-arm64"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__exclude: ".git/"
  config.vm.provider "parallels" do |prl|
    prl.memory = 512
    prl.linked_clone = true

  end
  # Gluster vms
  boxes = [
    { :name => "gluster1", :ip => "192.168.56.2" },
    { :name => "gluster2", :ip => "192.168.56.3" }
  ]

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network :private_network, ip: opts[:ip]

      if opts[:name] == "gluster2"
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbooks/provision.yml"
          ansible.inventory_path = "hosts.ini"
          ansible.limit = "all"
        end
        
      end
    end
  end
end