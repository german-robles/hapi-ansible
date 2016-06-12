# HApi-ansible-role

This repository contains ansible role to install and configure [HApi] and [HAproxy] (optional test environment).

### Version
1.0.0

### Tech

This repo use [Ansible].

### Installation

*** This Ansible Role can be used in debian OS family distros and RedHat OS family distros. It has been tested in ubuntu 14.04 (Trusty) and Centos 7. ***


You need to install ansible:

http://docs.ansible.com/ansible/intro_installation.html

### Configuration Files
###### You can find the following variables to configure HApi in the following file:

hapi_ansible/defaults/main.yml

```YAML
---
hapi_socat: '/var/run/haproxy.stat'
hapi_user: 'admin'
hapi_password: 'admin1234'
hapi_logfile: '/var/log/hapi/hapi.log'
hapi_debug: False
hapi_port: 6000
hapi_bind: '0.0.0.0'
hapi_sslkey: '/opt/hapi/ssl/hapi.key'
hapi_sslcrt: '/opt/hapi/ssl/hapi.crt'
hapi_accesslog: '/opt/hapi/log/hapi_access.log'
hapi_service_enabled: yes
hapi_service_state: started
hapi_daemon_user: hapi
hapi_daemon_group: hapi

#HAproxy vars
haproxy_service_enabled: yes
haproxy_service_state: started
```

#### Then you can configure the following templates of HApi & HAproxy if it's needed:

##### HApi
hapi_ansible/templates/etc/hapi/hapi.confj2

##### HAproxy
hapi_ansible/templates/etc/haproxy/haproxy.cfg.j2


### Ansible Playbook configuration:

###### You can find in the root of this repo a file called hapi_playbook.yml, this playbook contains the basic initialization of HApi ansible role:


```YAML
---

- hosts: all
  sudo: yes
  roles:
    # This role apply epel repo in order to install python packages in your redhat distro
    - { role: epel, when: "ansible_os_family == 'RedHat'" }
    # HApi ansible role
    - hapi_ansible

  vars:
    # If you want to skip haproxy instalation you can set the following var into false.
    install_haproxy: true
    # The following var set some hosts records into your /etc/hosts file in order to initialize the Proof Of Concept (file: hapi_ansible/tasks/main.yml)
    set_hosts: true

```
## Usage:

This playbook can be used using ansible playbook command as follows:

```sh
ansible-playbook -i <path/to/your/inventoryFile> hapi_playbook.yml
```
#### Vagrant:

You can use this playbook using Vagrant appending the following line into the Vagrantfile:

```ruby
Vagrant.configure(2) do |config|
....
  your configs here
...
  config.vm.provision :ansible do |ansible|
    # You can increase the verbosity adding more "v" ex: ansible.verbose = "vvv"
    ansible.verbose = "v"
    ansible.playbook = "hapi_playbook.yml"
  end
end

```
   [HApi]: <https://github.com/german-robles/hapi>
   [HAproxy]: <http://www.haproxy.org/>
   [Ansible]: <https://www.ansible.com/>
