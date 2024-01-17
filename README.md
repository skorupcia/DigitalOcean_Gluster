# DigitalOcean / Vagrant Gluster configuration for Ubuntu droplets

## Specifications

macOS: Sonoma 14.2.1

Ubuntu: Ubuntu-20-04-x64

Vagrant box: luminositylabsllc/bento-ubuntu-20.04-arm64

Updated gluster geerlingguy role removing the problem:

- ansible.builtin.include has been removed. Use include_tasks or import_tasks instead. This feature was removed from ansible-core in a release after 2023-05-16. Please update your playbooks.

## INSTRUCTIONS

1. Add your machine SSH to DigitalOcean account

2. Create API token and add to your DigitalOcean project

3. Update vars files to your personal preferences

   a) Update u_token in droplet/connection_vars.yml (api_token)
   
   b) Update u_ssh in droplet/connection_vars.yml (ssh fingerprint)

   c) Update hosts_dest in droplet/connection_vars.yml (hosts.ini physical destination)

4. With some adjustements to Vagrantfile and playbooks you will be able to run gluster with your local environment.


## RUN INSTRUCTIONS

1. Run required roles:

      ansible-galaxy install geerlingguy.firewall (since gluster role tasks are inside the playbook)
   
2. Run digitalocean.yml to create droplets and update hosts file:

      ansible-playbook digitalocean.yml

3. Run playbooks with provision.yml file:

      ansible-playbook -i hosts.ini provision.yml


## CHECK INSTRUCTIONS

1. ansible gluster -i hosts.ini -a "gluster peer status" -b

2. ansible gluster -i hosts.ini -a "gluster volume info" -b
  
3. Connect to both servers via SSH and create a file or directory in /mnt/gluster
     - you are supposed to see created in /mnt/gluster


## Droplet Delete

   If you would like to delete droplets, simply switch state of "Create gluster 1/2" from PRESENT to ABSENT and run your playbook.

## TroubleShooting

1. MacOS - INSTALL CERTIFICATES if your Geerlingguy roles end up with certificate error:
   
    Unknown error when attempting to call Galaxy at 'https://galaxy.ansible.com/api/': <urlopen error [SSL:CERTIFICATE_VERIFY_FAILED]

## Links 

  https://github.com/geerlingguy/ansible-for-devops

  https://github.com/geerlingguy/ansible-role-glusterfs

  https://github.com/geerlingguy/ansible-role-firewall

  2nd edition of Ansible for DevOps Jeff Geerling
