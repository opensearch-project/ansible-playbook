# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/focal64"

#  config.vm.provider "virtualbox" do |v|
#    v.memory = 3072
#    v.cpus = 3
#  end

  N = 3 
  (1..N).each do |i|
    config.vm.define "os#{i}" do |machine|
      machine.vm.hostname = "os#{i}"
      machine.vm.network "private_network", type: "dhcp"
      machine.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 2
      end #machine.privoder
    end #config
  end #(1..N)

  config.vm.define "dashboards1" do |machine|
    machine.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
    end
    machine.vm.hostname = "dashboards1"
    machine.vm.network "private_network", type: "dhcp"
    machine.vm.provision :ansible do |ansible|
      ansible.limit = "all"
      ansible.playbook = "opensearch-cluster.yml"
      ansible.extra_vars = "vagrant/oscluster.yml"
      #ansible.tags = ["dashboards"]
      ansible.raw_arguments = [
        "--diff"
      ]
      ansible.groups = {
        "all" => ["os1", "os2", "os3", "dashboards1"],
        "os-cluster" => ["os1", "os2", "os3"],
        "os-dashboards" => ["dashboards1"]
      }# groups
    end # provision
  end # config
end #file
