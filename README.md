# Overview

Simple ansible setup for updating (ie. sudo apt-get update) a stack of Raspberry Pi servers.

## Install
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible  # FAILED on RPi
sudo apt install ansible
```

## Generate Keys
Need keys on ansible host:
```
ssh-keygen
```

## Hosts
- 192.168.1.5 - host
- 192.168.1.3 - pihole
- 192.168.1.10
- 192.168.1.4
- 192.168.1.11
- 192.168.1.2

## Pre Setup
```
ssh pi@192.168.1.n # (optional)
ssh-copy-id pi@192.168.1.n
```

## Run
```
cd ansible_pi_stack
ansible-playbook -i hosts.txt apt.yaml
```

## The apt.yaml
Simple script from 2 year old example: https://linuxhint.com/run-apt-get-update-ansible/
```
---
- hosts: stack
  become: yes
  become_method: sudo
  tasks:
    - name: "Update Repository cache"
      apt:
        update_cache: true
        cache_valid_time: 3600
        force_apt_get: true
```

## To test
The following if from https://www.middlewareinventory.com/blog/ansible-apt-examples/
showing how to do:
```
sudo apt-get update
sudo apt-get upgrade
```

This differs from original, need to test this.

```
---
 - name: Ansible apt module examples
   hosts: web
   become: true
   tasks: 
    - name: Ansible Update Cache and Upgrade all Packages
      register: updatesys
      apt:
        name: "*"
        state: latest
        update_cache: yes

    - name: Display the last line of the previous task to check the stats
      debug:
        msg:  "{{updatesys.stdout_lines|last}}"
```

## REFERENCES
- https://linuxhint.com/run-apt-get-update-ansible/
- https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html
- https://www.middlewareinventory.com/blog/ansible-apt-examples/
