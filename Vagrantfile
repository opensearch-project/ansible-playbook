# -*- mode: ruby -*-
# vi: set ft=ruby :

net_prefix = "10.0.1"
os_hosts = 3
master_hosts = 3
dashboard_hosts = 1

os_device = "/dev/sdc"
os_vg = "opensearch"
os_lv = "opensearch"
os_mountpoint="/usr/share/opensearch"

provision_set=<<-SHELL
  set -eux
  if [ -f /tmp/os_updated ]; then
    echo OS already updated
  else
    apt-get update
    apt-get -y upgrade
    apt-get -y autoremove
    touch /tmp/os_updated
  fi
  apt-get install -y net-tools
  if [ "$(df -h | egrep -c "^.+#{os_vg}-#{os_lv}.+$")" -eq 1 ] ; then 
    df -h | egrep -c "^.+#{os_vg}-#{os_lv}.+$"
  else
    pvcreate -ff -y #{os_device}
    vgcreate #{os_vg} #{os_device}
    lvcreate #{os_vg} -l 100%FREE -n #{os_lv}
    mkfs.ext4 /dev/#{os_vg}/#{os_lv}
    mkdir -p #{os_mountpoint}
    mount /dev/#{os_vg}/#{os_lv} #{os_mountpoint}
  fi
SHELL

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.box_version = "20221107.0.0"
  
  config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox"
  
  (1..os_hosts+dashboard_hosts).each do |i|
    config.vm.define "os-#{i}" do |node|
      node.vm.provider "virtualbox" do |v|
        if i < os_hosts+dashboard_hosts #os host
          node.vm.hostname = "os-#{i}"
          v.name = "os-#{i}"
          v.memory = "1024"
          v.cpus = "1"
          v.gui = false
        else #dashboards host
          node.vm.hostname = "dashboard-#{i-os_hosts}"
          v.name = "dashboard-#{i-os_hosts}"
          v.memory = "1024"
          v.cpus = "1"
          v.gui = false
        end        
      end
      node.vm.network "private_network", ip: "#{net_prefix}.#{10+i-1}", hostname: true,virtualbox__intnet: true,virtualbox__intnet: "os-transport"
      node.vm.disk :disk, size: "5GB", name: "opensearch"

      node.vm.provision "shell", inline: provision_set
      
      if i == os_hosts+dashboard_hosts
        node.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.limit = "all"
          ansible.playbook = "opensearch.yml"

          ansible.host_vars = {
            "os-1" => {
              "ip" => "#{net_prefix}.10",
              "roles" => "data,master,injest"
            }, 
            "os-2" => {
              "ip" => "#{net_prefix}.11",
              "roles" => "data,master,injest"
            }, 
            "os-3" => {
              "ip" => "#{net_prefix}.12",
              "roles" => "data,master,injest"
            }, 
            "os-4" => {
              "ip" => "#{net_prefix}.13",
              "roles" => "data,master,injest"
            } 
          }
                    
          ansible.groups = {
            "os-cluster" => ["os-[1:#{os_hosts}]"],
            "master" => ["os-[1:#{os_hosts}]"],
            "dashboards" => ["os-[#{os_hosts+1}:#{os_hosts+1+dashboard_hosts}]"]
          }
          ansible.extra_vars = {
            os_cluster_name: "development-cluster",
            os_download_url: "https://artifacts.opensearch.org/releases/bundle/opensearch",
            os_version: "2.3.0",
            os_dashboards_version: "2.3.0",
            domain_name: "local",
            xms_value: "1",
            xmx_value: "1",
            cluster_type: "multi-node",
            os_user: "opensearch",
            os_dashboards_user: "opensearch",
            cert_valid_days: "730",
            auth_type: "internal",
            copy_custom_security_configs: "false",
            iac_enable: "true",

            admin_password: "Test@123",
            kibanaserver_password: "Test@6789"
          }
          ansible.verbose = "v"
        end
      end
    end
  end
end
