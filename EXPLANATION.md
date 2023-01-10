Structure of my project
=========

My project has 2 playbooks. The first is main-playbook.yml. This playbook is responsible for setting up the VM that will be used for the execution of this project. I used SSH to access the VMs so the keys needed to be generated and all of this happens within mail-playbook.yml

The second playbook is ip3-playbook.yml and this is where the requirements for the IP have been met. Please see the explanation below for the tasks executed in this playbook. 

ip3-playbook.yml
------------

In this playbook we have:
    - hosts: to indicate the playbook will run on all the hosts created in our inventory. In our case this is only one VM but it could be extended to others. The inventory file also includes SSH access details
    - become: to indicate that we should run the playbook as the sudo user so we don't run into permission issues
    - vars: these are the global variables that can be reused within the tasks in the playbook
    - task: install aptitude: we set up the VM with dependencies
    - task: install required system packages: setup the VM with any packages required
    - task: multiple tasks to setup docker and docker dependencies. Since we use docker to run our containers we need to  make sure the VM has docker installed
    - task: Clone repo: Since the IP requirement was to use the github repo, we clone the files onto the VM
    - task: Create a network: Since we will be using 3 different docker containers, we need to network them together so they can exchange info. I used the ansible module to create this network.
    - roles: We then include 3 roles in the playbook - one for each container that we want to setup


Roles
------------

My ansible playbook uses 3 different roles - one for each container that we setup. The basic structure for each role. The project has a folder for each role but the bulk of my customisations are in the tasks folder which has a main.yml file where all the tasks for this role are specified. For example, in the backend role I do the following:

Use the docker module to create a container for the backend and then and use the yolo/backend image to setup this container. I also make sure that the docker container has a seperate volume for the data that is shared with the other containers. And of course that it can access the network that I created for the 3 containers. 

The other roles have similar tasks to create them and set them up. 

Order for my tasks
------------

Since the tasks in the ansible playbook are called in sequence, we have to make sure that the tasks are written in a certain order. For example, when the playbook starts we have a blank VM - we need to first install the OS and software packages needed to run our code. We can then run additional tasks to customise the VM environment and download or install any files or data that we need. Finally we can use ansible modules to setup any additional tools like Docker or clone a repo. Once the VM environment is ready, I finally used the roles to create the 3 different containers I needed to run the eCommerce application. 

A note for my submission
------------

I found this IP unusually complicated because I had to use multipass instead of vagrant since I'm operating on a Mac ARM machine. A lot of time was spent researching how to get the pieces to fit together and work instead of on the usage of Ansible - that was actually the easy part!!