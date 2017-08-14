# -*- mode: ruby -*-
# vi: set ft=ruby :

hosts = {
  "Mysql-01" => "192.168.33.10",
  "Mysql-02" => "192.168.33.11",
  "Control-01" => "192.168.33.12"
}

Vagrant.configure("2") do |config|
  hosts.each do |name, ip|
    config.vm.define name do |machine|
      machine.vm.box = "geerlingguy/ubuntu1604"
      machine.vm.hostname = "%s" % name
      machine.vm.network :private_network, ip: ip
      machine.vm.provider "virtualbox" do |v|
          v.name = name
          v.customize ["modifyvm", :id, "--memory", 512]
      end
    end
  end
end
