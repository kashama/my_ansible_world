# my_ansible_world
This is my repo where I practice my ansible skills

This R:EADME file will be edited progressively

1. First ansible command to establish connectivity to different host servers 
      ansible all --key-file /root/.ssh/ansible_world -i inventories/01_inventory -m ping 

After adding the defauft ansible.cfg(configuration file) we can run the short version command below, and it will work as the previous one and make the connection to the servers:

       ansible all -m ping

2. The ansible command that list all the ansible hosts(servers)

       ansible all --list-hosts

3. When the ansible make connectivity to the servers, it collects all the processes or the informations about those servers. To see those informations we run the command below:

       ansible all -m gather_facts


4. The previous command will collect the informations about all servers, but if we want to get the information about a specific server(host), we can run the same comand by specifying the limit to the server(Ex: 172.18.0.8)

       ansible all -m gather_facts --limit 172.18.0.8

5. When trying to run a command that is suppose to make changes to the servers(like when we run: apt update command without sudo, this command will fail unless we use sudo for the command to execute like: sudo apt update or sudo apt install something), in the ansible, it is also the same while tryng to run a command with module that is suppose to make changes to the server(like: ansible all -m apt, here the module apt is used to modifying the server like:  ansible all -m apt -a update_cache=true) but this command will fail because we dont have the priviledge. To make this command run, we have to add options(or parameters that will allow the command to run as sudo in ansible)

	
       ansible all -m apt -a update_cache=true --become --ask-become-pass

The equivalent as:

	sudo apt update 

The sudo or user password will be asked to run the command successfully

In case we running different OS like, Ubuntu/Debian  the module is: apt, and for CentOS/Redhat the modules are: yum and dnf
So can run with package module that will select the correct modules(apt,yum,dnf) depending on the linus OS in the server:

	ansible all -m package -a "update_cache=true" --become --ask-become-pass
 
