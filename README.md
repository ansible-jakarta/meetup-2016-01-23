Introduction to Ansible Demo
============================

To get started run `vagrant up` to bring up three machines:

 - trusty (192.168.20.11)
 - pricise (192.168.20.12)
 - centos7 (192.168.20.13)

Default user:password on each box is: vagrant:vagrant

To SSH into any of the machines use:

```
vagrant ssh <box-name>
```

Most of the example are done on trusty box, so `vagrant ssh trusty`.

Demo: Installing Ansible
------------------------

Source: http://docs.ansible.com/ansible/intro_installation.html

Login into trusty box and:

```
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

Demo: Ad-hoc Commands
---------------------

Concepts covered:

 - inventory file
 - ping module
 - setup module
 - shell module
 - ansible CLI options:
   - -k, --ask-pass
   - -m, --module-name
   - -a, --args

Execute ad-hoc commands against all machines in the inventory from trusty box.

Use 'vagrant' password when prompted.

```
ansible all -k -m ping
ansible all -k -m setup
ansible all -k -m shell -a uptime
```

Demo: Simple Playbook Internals (ansible.yml)
---------------------------------------------

Concepts covered:

 - playbook internals:
   - hosts
   - tasks
   - become
 - modules:
   - apt
 - ansible-playbook CLI options:
   - -i, --inventory-file
   - -c, --connection
   - -C, --check
   - -v, --verbose

Install ansible using ansible on trusty box using local connection.

Since we already isntalled ansible manually this will simply confirm that ansible is installed.

```
ansible-playbook -i 'localhost,' -c local ansible.yml -v
ansible-playbook -i 'localhost,' -c local ansible.yml -C -v
```

Demo: Ansible Module Overview
-----------------------

Concepts covered:

 - ansible-doc
 - ansible-doc CLI options:
   - -l, --list
   - -s, --snippet module

Explore module documentation and avaialble modules.

```
ansible-doc -l
ansible-doc apt
ansible-doc -s apt
```

Demo: SSH Key Deployment (authorized_keys.yml)
----------------------------------------------

Concepts covered:

 - modules:
   - authorized_key
 - plugins:
   - file lookup

Generate SSH key-pair on trusty, then add it to autoorized keys on all hosts in inventory.

First time you'll be prompted for password ('vagrant'). Second time no password is required.

```
ssh-keygen
ansible-playbook -k authorized_keys.yml
ansible-playbook authorized_keys.yml
```

Demo: Resty (resty_git.yml)
---------------------------

Concepts covered:
 - variable prompt
 - modules:
   - package
   - git

Install resty application from git.

You'll be prompted for resty release during playbook run.

```
ansible-playbook resty_git.yml -v
```

Run the same playbook in non-interactive mode by passing the variable via CLI.

```
ansible-playbook resty_git.yml -e "resty_version=2.1" -v
```

Demo: Templating (template.yml)
-------------------------------

Concepts covered:

 - ansible facts
 - playbook variables
 - modules:
   - template

Create a sample config file on each box using playbook variables and ansible facts.

New file /tmp/my_config.txt should be created on each box.

```
ansible-playbook template.yml -v
```

Demo: Apache Install (apache_try1.yml)
--------------------------------------

Concepts covered:

 - inventory groups
 - modules:
   - apt
   - yum
   - service

Install apache on all boxes. Use group variables to provide correct package name to apt and yum modules.

Group variables are defined in group_vars/[ubuntu,centos].yml

```
ansible-playbook apache_try1.yml -v
```

Demo: Apache Install (apache_try2.tml)
--------------------------------------

Concepts covered:

 - group variables
 - modules:
   - package
   - service

Use generic package module instead of OS specific (apt/yum).

```
ansible-playbook apache_try2.yml -v
```

Demo: Apache Install (apache_try3.yml)
--------------------------------------

Concepts covered:

 - using facts
 - group variables
 - modules:
   - group_by
   - package
   - service

Use groups dynamically created with group_by module, instead of pre-defined groups.

Use 'flat_inventory', which doesn't have any groups defined, instead of default 'inventory' file.

Use variables defined in group_vars/os_[Ubuntu,CentOS].yml.

```
ansible-playbook apache_try3.yml -i flat_inventory -v
```
