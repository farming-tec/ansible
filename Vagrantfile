# -*- mode: ruby -*-
# vi: set ft=ruby :

# Ansible constants
ANSIBLE_DEBUG     =   true
ANSIBLE_PATH      =   "ansible"
ANSIBLE_INVENTORY =   ANSIBLE_PATH + "/development"
ANS_PLAYBOOK_CMD  =   "ANSIBLE_FORCE_COLOR=true ANSIBLE_NOCOLOR=false ansible-playbook"  

# Ansible playbook paths, do not modify
ANSIBLE_GENERAL_  =   ANSIBLE_PATH + "/generalservers.yml"
ANSIBLE_DOCKER_   =   ANSIBLE_PATH + "/generaldocker.yml"
ANSIBLE_UP_       =   ANSIBLE_PATH + "/vagrantup.yml"


Vagrant.configure("2") do |config|
  # Use Ubuntu Bionic Amd64
  config.vm.box = "hashicorp/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 1536
  end  

  # Mapping port
  config.vm.network "forwarded_port", guest: 80, host: 8000     # NGNIX service
  config.vm.network "forwarded_port", guest: 5000, host: 4000   # Flask service

  # Synced folders
  config.vm.synced_folder "../fast_bionic64", "/var/www/html"   # NGNIX folder
  config.vm.synced_folder "../flask_bionic64", "/vagrant_data"  # Flask folder
  config.vm.synced_folder "../ansible_bionic64", "/ansible"     # Local ansible folder

  # Run to setup base programs
  config.vm.provision "setup", type: "ansible_local" do |ansible|   
    ansible.compatibility_mode = "2.0" 
    ansible.install_mode = "pip"
    ansible.inventory_path = ANSIBLE_INVENTORY
    ansible.limit = "all"
    ansible.pip_install_cmd = "sudo apt install -y python3-pip && sudo ln -s /usr/bin/pip3 /usr/bin/pip"
    ansible.playbook = ANSIBLE_GENERAL_
    ansible.playbook_command = ANS_PLAYBOOK_CMD
    ansible.verbose = ANSIBLE_DEBUG
  end

  # Ansible provisioner to deploy docker images (Persistent storage)
  config.vm.provision "docker-images", type: "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.inventory_path = ANSIBLE_INVENTORY
    ansible.limit = "all"
    ansible.playbook = ANSIBLE_DOCKER_
    ansible.playbook_command = ANS_PLAYBOOK_CMD
    ansible.verbose = ANSIBLE_DEBUG
  end  
  
  # # Run at boot and reload event of the VM
  config.vm.provision "boot", type: "ansible_local", run: "always" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.inventory_path = ANSIBLE_INVENTORY
    ansible.limit = "all"
    ansible.playbook = ANSIBLE_UP_
    ansible.playbook_command = ANS_PLAYBOOK_CMD
    ansible.verbose = ANSIBLE_DEBUG
  end 
end
