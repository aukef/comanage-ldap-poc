# This guide is optimized for Vagrant 1.7 and above.
# Although versions 1.6.x should behave very similarly, it is recommended
# to upgrade instead of disabling the requirement below.
Vagrant.require_version ">= 1.7.0"

require 'json'

# Read JSON file with host details
servers = JSON.parse(File.read(File.join(File.dirname(__FILE__), 'servers.json')))

Vagrant.configure(2) do |config|
	servers.each do |server|
  	config.vm.define server['name'] do |srv|
    	srv.vm.box = server['box']
  		srv.vm.hostname = server['name']
  		srv.vm.network "private_network", ip: server['ip']
			srv.vm.provider "virtualbox" do |v|
				v.memory=1024
				v.cpus=1
			end # srv.vm.provider
  		srv.ssh.insert_key = false
			srv.vm.provision "ansible" do |ansible|
			  ansible.playbook = server['playbook']
			end # config.vm.provision
		end # config.vm.define
	end # servers.each
end # Vagrant.configure

