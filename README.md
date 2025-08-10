# my_ansible_world
This is my repo where I practice my ansible skills

This README file will be edited progressively

1. First ansible command to establish connectivity to different host servers 
# ansible all --key-file /root/.ssh/ansible_world -i inventories/01_inventory -m ping 

After adding the defauft ansible.cfg(configuration file) we can run the short version command below, and it will work as the previous one and make the connection to the servers:

# ansible all -m ping

2. The ansible command that list all the ansible hosts(servers)

# ansible all --list-hosts

3. When the ansible make connectivity to the servers, it collects all the processes or the informations about those servers. To see those informations we run the command below:

# ansible all -m gather_facts


4. The previous command will collect the informations about all servers, but if we want to get the information about a specific server(host), we can run the same comand by specifying the limit to the server(Ex: 172.18.0.8)

# ansible all -m gather_facts --limit 172.18.0.8
