# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #common config
  config.vm.box = "geerlingguy/centos7"
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", 512]
    vb.customize ["modifyvm", :id, "--cpus", 1]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  # create back end nginx web servers
  A=3
  (1..A).each do |i|
    config.vm.define "web#{i}" do |node|
      node.vm.hostname = "web#{i}.nginx"
      node.vm.network :private_network, ip: "192.168.15.12#{i}"
      node.vm.network :forwarded_port, guest: 80, host: "809#{i}"
      node.vm.provision "ansible" do |ansible|
        ansible.limit = "web#{i}.nginx"
        ansible.playbook = "web-nginx.yml"
        ansible.inventory_path = "./env/vagrant/inventory"
        ansible.verbose = true
      end
    end
  end

  # create load balancer/front end ha proxy
  config.vm.define "lb", primary: true do |lb_config|
    lb_config.vm.hostname = "lb.haproxy"
    lb_config.vm.network :private_network, ip: "192.168.12.150"
    lb_config.vm.network :forwarded_port, guest: 80, host: "9095"
    lb_config.vm.provision "ansible" do |ansible|
      ansible.limit = "lb.haproxy"
      ansible.playbook = "lb-haproxy.yml"
      ansible.inventory_path = "./env/vagrant/inventory"
      ansible.verbose = true
    end
  end

end
