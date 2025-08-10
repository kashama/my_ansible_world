# my_ansible_world
This is my repo where I practice my ansible skills

This README file will be edited progressively

1. First ansible command to establish connectivity to different host servers 
# ansible all --key-file /root/.ssh/ansible_world -i inventories/01_inventory -m ping 

After adding the defauft ansible.cfg(configuration file) we can run the short version command below, and it will work as the previous one and make the connection to the servers:

# ansible all -m ping
