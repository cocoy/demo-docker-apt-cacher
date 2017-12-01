# Docker, Apt-Cacher_Server Demo 

## Background

Readers are assumed to have basic Ansible, Docker Engine idea. We use this repo for our demo playbooks.
At the end of the demo, we will have a Vagrant instance, with Docker inside and a running docker instance: apt-cacher-ng.

## How to use this Repo: 

   0.  Using VirtualBox and  Vagrant, it is assume we have ansible installed, clone this repository and run the following commands:

        ```
          $ cd ~/
          $ git clone https://github.com/cocoy/demo-docker-apt-cacher.git  tower-base
          $ cd ~/tower-base
          $ ansible-galaxy  install -r roles/requirements.yml -c 
          $ vagrant up
          $ vagrant provision
	```
        

   1.  Setting up the Docker Host,  the playbook setup_docker_host.yml will target the machines defned at inventories/dev/host inventory file.
       By default the `vagrant provision` will run the setup_docker_host.yml playbook, but we can also ssh to the vagrant machine and do the following: 
	 
       `ansible-playbook -i inventories/dev/host setup_docker_host.yml -c local`

       The result is we will have a Docker host setup on the current host we are running.. We can perform docker commands below:

       a. List Docker instances: `sudo docker ps` 
       b. List Docker volumes: `sudo docker volume ls` 
       c. Create Docker volume: `sudo docker volume create xvol`` 
   

   2. Playbook to create a docker container: apt-cache-ng. 
   
      2.1.  You have to run first the setup_docker_volume.yml 
	
 	    `ansible-playbook -i inventories/dev/host setup_docker_volume.yml -c local`

            The volume stored on the docker_host, thus apt_cacher_volume can be shared with docker instances. 

      2.2.  Now we  can run the run_apt_cacher_container.yml. 
            What it will do is to create an apt-cacher-ng instance inside a Docker Host. 

       `ansible-playbook -i inventories/dev/host run_apt-cacher_container.yml -c local`

       This apt-cacher will run on port specified to this, we can check that using the command netstat. 
       `netstat -l | grep 3142` 

      As a result: these playbooks will create  apt-cacher-ng on port 3142 of the docker host. 
      We can check the docker instance by `sudo docker ps`  command.
    
	  
   3. Another playbook that will install apt-cacher client to target client hosts defined on new set of inventory file, to be apt-cacher client.

       `ansible-playbook -i inventories/dev/hosts install_apt_cacher_client.yml -c local`

		site_url: "apt.example.com:3142"

      Where: apt.example.com can be IP or dns of your apt-cacher-ng server and port.
      Or we can specify a new inventory file that contains the host we want to target to install the apt-cache client.


## References:

Use for the documentation:
https://galaxy.ansible.com/juju4/ansible-apt-cacher-ng-client/

Bug in Ansible Module Create Docker Volume:
https://github.com/ansible/ansible/issues/30239

## Author Information

Auther info goes here.
