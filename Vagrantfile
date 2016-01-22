# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  # config.vm.network "private_network", type: "dhcp"

  # define machines
  machines = []
  machines << { name: "trusty", box: "ubuntu/trusty64", ip: "192.168.20.11"}
  machines << { name: "precise", box: "ubuntu/precise64", ip: "192.168.20.12" }
  machines << { name: "centos7", box: "centos/7", ip: "192.168.20.13"}

  # iterate through machines
  machines.each do |machine|
    # define config for given machine
    config.vm.define machine[:name] do |conf|
      conf.vm.box = machine[:box]
      conf.vm.network "private_network", ip: machine[:ip], virtualbox__intnet: true
      
      # provision all machines together, once last machine is up
#      if machine == machines.last
#        conf.vm.provision :ansible do |ansible|
#          ansible.playbook = "test.yml"
#          ansible.limit = "all"
#          ansible.verbose = "v"
#        end
#      end
    end
  end
end
