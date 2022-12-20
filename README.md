How to use this repository

Requirements
------------

Make sure you have multipass installed

Set up your VM
------------

Run ansible playbook main-playbook.yml to provision the vm with SSH access. Check that the multipass VM is running by using the 'multipass list' command in the terminal. Take note of the IP address of the VM that was provisioned and started. Open the 'inventory' file and update the IP address here. 

Create containers and run eCommerce app
------------

Run ansible playbook ip3-playbook.yml to create 3 seperate containers (backend, client, DB) and run the eCommerce app. Now you can open the app in the browser using the IP address of them VM with port 3000. 



