# Farming Environment

## Requirements

* VirtualBox
* Vagrant

## Setup Folders

Clone the folder `ansible/secrets.example` below `ansible`, rename the new directory to `secrets`

## Setup Files

Clone all the files inside `ansible/host_vars`, change its extension to `.yml`, their name must be the same. 

For example for `ansible/host_vars/mainserver.yml_`, its cloned file is `ansible/host_vars/mainserver.yml`


## Update Project

```
git pull
vagrant reload --provision
vagrant up
```

## Turn On 
```
vagrant up
```

## Turn off
```
vagrant halt
```



