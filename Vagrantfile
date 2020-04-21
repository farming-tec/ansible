# -*- mode: ruby -*-
# vi: set ft=ruby :

ANSIBLE_PATH = "ansible"
ANSIBLE_DEBUG = true
ANSIBLE_GENERAL_ = ANSIBLE_PATH + "/generalservers.yml"
ANSIBLE_UP_ = ANSIBLE_PATH + "/vagrantup.yml"
ANSIBLE_INVENTORY = ANSIBLE_PATH + "/development"


Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"

  config.vm.network "forwarded_port", guest: 80, host: 8000
  config.vm.network "forwarded_port", guest: 5000, host: 4000

  config.vm.synced_folder "../fast_bionic64", "/var/www/html"
  config.vm.synced_folder "../flask_bionic64", "/vagrant_data"
  config.vm.synced_folder "../ansible_bionic64", "/ansible"

  config.vm.provision "ansible_local" do |ansible|
    ansible.inventory_path = ANSIBLE_INVENTORY
    ansible.playbook = ANSIBLE_GENERAL_
    ansible.install = true
    ansible.install_mode = "pip"
    ansible.limit = "all"
    ansible.verbose = ANSIBLE_DEBUG
    ansible.compatibility_mode = "2.0" 
  end
  
  config.vm.provision "ansible_local", run: "always" do |ansible|
    ansible.inventory_path = ANSIBLE_INVENTORY
    ansible.playbook = ANSIBLE_UP_
    ansible.limit = "all"
    ansible.verbose = ANSIBLE_DEBUG
    ansible.compatibility_mode = "2.0"
  end 
end
