# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

role = File.basename(File.expand_path(File.dirname(__FILE__)))

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/xenial64"
  config.vm.synced_folder '.', '/vagrant', disabled:true 
  config.vm.hostname = "ansible-#{role}"
  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.memory = 2048
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
  end


  config.vm.provision "shell", inline: <<-SHELL
    test -e /usr/bin/python || (apt -qqy update && apt install -qqy python-minimal )
  SHELL

  config.vm.provision :ansible do |ansible|
       ansible.playbook = "setup_docker_host.yml"
       ansible.raw_arguments = ['--syntax-check']
  end

  config.vm.provision :ansible do |ansible|
       ansible.playbook = "setup_docker_host.yml"
       ansible.become= true
       ansible.verbose = "vv"
  end
  #config.vm.provision :ansible do |ansible|
  #     ansible.playbook = "setup_docker_volumes.yml"
  #     ansible.become= true
  #     ansible.verbose = "vv"
  #end
end

