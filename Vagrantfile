# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile for three VMs: webserver, dbserver, apiserver
# Uses public_network so it works without admin rights

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-22.04"
  config.vm.boot_timeout = 1200;

  # Webserver VM
  config.vm.define "webserver" do |webserver|
    webserver.vm.hostname = "webserver"

    # Port forwarding: host 8888 -> guest 80
    webserver.vm.network "forwarded_port", guest: 80, host: 8888, host_ip: "127.0.0.1"

    # Private network with fixed IP
    webserver.vm.network "private_network", ip: "192.168.56.11"

    # Shared folder
    webserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant",
      mount_options: ["dmode=775,fmode=777"]

    # Provision script
    webserver.vm.provision "shell", path: "build-webserver-vm.sh"
  end

  # Database server VM
  config.vm.define "dbserver" do |dbserver|
    dbserver.vm.hostname = "dbserver"

    # Private network with fixed IP
    dbserver.vm.network "private_network", ip: "192.168.56.12"

    dbserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant",
      mount_options: ["dmode=775,fmode=777"]

    dbserver.vm.provision "shell", path: "build-dbserver-vm.sh"
  end

  # API server VM
  config.vm.define "apiserver" do |apiserver|
    apiserver.vm.hostname = "apiserver"

    # Port forwarding: host 8889 -> guest 8888
    apiserver.vm.network "forwarded_port", guest: 8888, host: 8889, host_ip: "127.0.0.1"

    # Private network with fixed IP
    apiserver.vm.network "private_network", ip: "192.168.56.13"

    apiserver.vm.synced_folder ".", "/vagrant", owner: "vagrant", group: "vagrant",
      mount_options: ["dmode=775,fmode=777"]

    apiserver.vm.provision "shell", path: "build-apiserver-vm.sh"
  end

end
