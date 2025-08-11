# Name of my repo = my_ansible_world
This is my repo where I practice my ansible skills

This R:EADME file will be edited progressively
# Using ansible COMMANDS to manage servers

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

6. To install tools (vim and tmux) in the ansible servers(Ubuntu/Debian)

        ansible all -m apt -a name=vim --become --ask-become-pass 

or for all(Ubuntu/Debian=apt module; CentOS/RedHat=yum and dnf module) we use: package module:

	 ansible all -m package -a name=vim --become --ask-become-pass

	 ansible all -m package -a name=tmux --become --ask-become-pass

When we run these commands above to make changes to the systems, it will prompt to BECOME Password to ask you to provide the password like you do with sudo(sudo apt update && sudo apt install something)

After updating the system indexes with(sudo apt update) we can see the see the system indexes that are up to date and the ones need to be upgraded(sudo apt dist-upgrade) to upgrade. With ansible, we run the command below to upgrade specific index(tool) by providing the argument (state=lates).


	  ansible all -m package -a "name=wget state=latest" --become --ask-become-pass

You can see that when there multiple arguments(-a) we use " "

In ubuntu/debian systems we can also run:

	 ansible all -m apt -a "upgrade=dist" --become --ask-become-pass  

To upgrade all the necessary indexes(tools) to the latest version

# Using ansible PLAYBOOKS to efficiently manage servers
Here, we use our test editor(vim) to create a playbook file(01_playbook.yaml)


 ---
 - name: The first installation ansible playbook
   hosts: all 
   become: true 

   tasks:
   - name: Installing nginx in the servers...
     package:
     	name: nginx
        state: present

In our playbook we use:

hosts: all #To select all the servers defined in our inventory
become: true #to become sudo
package: #Here, we can use apt(Ubuntu/Debian) but we use package module for all modules(apt,yum,dnf)
state: present #to check if the package is already installed before trying to install it.

The ansible playbook is writing in yaml format, the above code allow us to install apache2 into our targeted servers.

Then we can run the command below to execute our playbook(01_playbook.yaml) with the associated servers inventory(01_inventory)

	  ansible-playbook -i inventories/01_inventory playbooks/01_playbook.yaml --become --ask-become-pass


Note: with a state set to absent, it allows to uninstall the existing tool


There is another way to make changes into the systems based on its criteria like(distributon for example: Ubuntu/Debian or Centos/Fedora and so on.


In this case we use the module(when)


      when: ansible_distribution in ["Ubuntu","Debian"]

      when: ansible_distribution == "Ubuntu"

      when: ansible_distribution == "CentOS"
~                                                 
