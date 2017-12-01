# Docker, Apt-Cacher_Server Demo 

## Background

Readers are assumed to have basic Ansible, Docker Engine idea. We use this repo for our demo playbooks.
At the end of the demo, we will have a Vagrant instance, with Docker inside and a running docker instance: apt-cacher-ng.

## How to use this Repo: 

   0.  Using Virtualbox or Vagrant, it is assume we have ansible installed, clone this repository inside this machine and run the following commands:

	```
        $ cd ~/
	$ git clone https://github.com/cocoy/demo-docker-apt-cacher.git  tower-base
        $ cd ~/tower-base
        $ ansible-galaxy  install -r roles/requirements.yml -c 
        ```

   1.  Setting up the Docker Host,  the playbook setup_docker_host.yml will target the machines defned at inventories/dev/host inventory file.
   
       `ansible-playbook -i inventories/dev/host setup_docker_host.yml`

       The result is we will have a Docker host setup on the current host we are running.. We can perform docker commands below:

       a. List Docker instances: `sudo docker ps` 
       b. List Docker volumes: `sudo docker volume ls` 
   

   2. Playbook to create a docker container: apt-cache-ng. 
   
      2.1.  You have to run first the setup_docker_volume.yml 
	
 	    `ansible-playbook -i inventories/dev/host setup_docker_volume.yml`

            The volume stored on the docker_host, thus apt_cacher_volume can be shared with docker instances. 

      2.2.  Now you can run the run_apt_cacher_container.yml 
       What it will do next is to create an apt-cacher-ng inside a Docker Host. It means or target host here would be the hostname/IP of docker host above.

       `ansible-playbook -i inventories/dev/host run_apt-cacher_container.yml`

       This apt-cacher will run on port specified to this, we can check that using the command netstat. 
       `netstat -l | grep 3142` 

      This will run apt-cacher-ng on port 3142 of the docker host. 
    
	  
   3. Another playbook that will install apt-cacher client to target client hosts defined on new set of inventory file, to be apt-cacher client.

       `ansible-playbook -i inventories/dev/hosts install_apt_cacher_client.yml`

		site_url: "apt.example.com:3142"

      Where: apt.example.com can be IP or dns of your apt-cacher-ng server and port.


## References:

Use for the documentation:
https://galaxy.ansible.com/juju4/ansible-apt-cacher-ng-client/

## Author Information

Auther info goes here.
