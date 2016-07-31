# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.define "ubuntu_postgres" do |ubuntu_postgres|
  end
  config.vm.provider :"virtualbox" do |vb|
    vb.name = "ubuntu-postgres"
  end
  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.87.41"

end