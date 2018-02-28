Proof of Concept SURFnet COmanage 
=================================

Requirements
------------
This proof of concept uses Virtualbox,  Vagrant and Ansible.  


Overview
--------
The PoC creates 2 virtual machines:  client and server. This is
specified by 'servers.json',  which is read by the Vagrantfile.

The VM's are provisioned using Ansible,  one as an OpenLDAP
server, and one as an OpenLDAP client. The server is configured
to pull the data from the SURFnet COmanage LDAP server, and the
client is configured to authenticate users against this server.


Caveats
-------
 * The client and server VM have hardcoded IP addresses for this 
PoC.  Make sure that 192.168.0.11 and 192.168.0.12 are usable
in your test-setup. 

 * The OpenLDAP server playbook (server.yml) behaves poorly when
executed multiple times.  It is not idempotent,  because the
OpenLDAP configuration is done by executing LDIF's with 
changetype 'add'. Effectively,  re-provisioning the server will
fail and yield Ansible execution errors.

The safest choice is to liberally destroy the VM's when testing,
or at least to not force re-provisioning on the server once it
has completed.


Getting started
--------------
If you do not have Virtualbox,  Vagrant or Ansible installed,
do this first.  On OSX, you can use homebrew:
 * brew cask install virtualbox
 * brew cask install vagrant
 * brew install ansible

If you do not have the hostsupdater and vbguest plugins in Vagrant, 
install them:
  * vagrant plugin install vagrant-hostsupdater
  * vagrant plugin install vagrant-vbguest

Put the following in /etc/sudoers.d/vagrant-hostsupdater
```
# Allow passwordless startup of Vagrant with vagrant-hostsupdater.
Cmnd_Alias VAGRANT_HOSTS_ADD = /bin/sh -c echo "*" >> /etc/hosts
Cmnd_Alias VAGRANT_HOSTS_REMOVE = /usr/bin/sed -i -e /*/ d /etc/hosts
%admin ALL=(root) NOPASSWD: VAGRANT_HOSTS_ADD, VAGRANT_HOSTS_REMOVE
```


Usage
------------------------
  * Edit server.yml and specify your credentials to syncronize the
	  COmanage LDAP server.  

  * Run 'vagrant up' to install and provision the VM's.  Afterwards,
	  login to the client and server systems to observe the LDAP config.

  * You can use 'slapcat' on the server to see the replicated content.

  * Use 'id student54' (or any other existing user) on the client to 
	  check if the user is retrieved from the OpenLDAP server.


Next steps
----------
The OpenLDAP server is setup as a standalone Ansible role and can be
used in your own Ansible environment.

The client-side changes are minimal and can be incorporated in your
own Ansible environment as you see fit. 

