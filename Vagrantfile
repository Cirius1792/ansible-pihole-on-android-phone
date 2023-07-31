# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
 config.vm.box = "debian/buster64"
#  config.vm.hostname = "android-pihole"
#  config.vm.network "public_network", ip: "192.168.1.100"

 # SHELL
   config.vm.define "android-pihole" do |app|
   	app.vm.hostname = "app1"
   	app.vm.box = "debian/buster64"
        app.vm.network "public_network", ip: "192.168.1.100"
        #app.vm.network "private_network", type: "dhcp"
   end

  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "playbook.yml"
  end
end
