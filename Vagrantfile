# -*- mode: ruby -*-
# vi: set ft=ruby :

# vagrantfile
# -----------------------------
#
# Test vagrantbox to locally testing ansible provisioning

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true

  config.vm.box = "generic/ubuntu1804"
  config.vm.hostname = "shop.local"
  config.vm.network :private_network, ip: "192.168.34.10"
  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end

  #config.hostmanager.aliases = [""]

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "packer/ansible/playbooks/vault-agent.yaml"
    ansible.extra_vars = {}
  end
end
