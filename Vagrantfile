# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/jammy64"

  
  config.vm.provider "virtualbox" do |vb|
    vb.cpus = 4   # Adjust CPUs  
    # Customize the amount of memory on the VM:
    vb.memory = "4096"
  end

  # Define ports 
  rpc_port = ENV['RPC_PORT'] || "9933"
  prometheus_port = ENV['PROMETHEUS_PORT'] || "9651"
  node_name = ENV['NODE_NAME'] || "test-node"
  nginx_domain = ENV['NGINX_DOMAIN'] || "192.168.56.101"
  nginx_ports_guest = ENV['NGINX_GUEST_PORT'] || "80"
  nginx_ports_host = ENV['NGINX_HOST_PORT'] || "8080"
  
  # Network forwarding
  config.vm.network "forwarded_port", guest: rpc_port, host: rpc_port
  config.vm.network "forwarded_port", guest: prometheus_port, host: prometheus_port
  config.vm.network "forwarded_port", guest: nginx_ports_guest , host: nginx_ports_host  

  # Network pvt for local host
  config.vm.network "private_network", ip: nginx_domain

    # Provision with Ansible and pass variables
  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "avail_revprox_setup.yml"
      ansible.extra_vars = {
        avail_node_name: node_name,
        avail_rpc_port: rpc_port,
        avail_prometheus_port: prometheus_port,
        nginx_domain: nginx_domain
      }
  end

end
