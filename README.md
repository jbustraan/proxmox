# proxmox ansible scripts

A set of Ansible playbooks used to create virtual machines on a Proxmox VE cluster. These scripts use PowerDNS API and phpIPAM API to provision new IP space and register DNS names.  Script requires two inputs 1) VM name and 2) Proxmox cluster to use for provisioning.  Virtual machines are built from AlmaLinux-8-GenericCloud-latest image so machine is simple disk layout with a single root partition.  Cloud init images are used to allow passing configuration from files user-data.yml, network-config.yml, vendor-data.yml and meta-data.yml.  These files are stored as an ISO snippet in Proxmox storage.

## proxmox-deploy role - to deploy new VMs to cluster, do the following
 
$ ansible-playbook -i ./hosts playbooks/proxmox_deploy.yml
* Deploy VM's requires vars set interactively from the CLI.  Follow the prompts
** added sub-bullet 1
** added sub-bullet 2


## proxmox-remove role - to remove a VM from cluster, do the following
$ ansible-playbook -i hosts playbooks/proxmox_remove.yml --extra-vars '{"VM_name":"nextcloudVM","Node_name":"pve1"}'

* Remove VM's requires extra-vars to set global variables. 
* Also the VM_name and Node_name variables must have matching "hosts" file entries
* Currently you must manually remove the DNS name from dns.bustraan.lab server (PowerDNS) and IP address from phpIPAM (ipam.bustraan.net) server


## Additional features needed
* Create playbook for removing IPAM IP entry - skeleton exists called "test9-ipam-delete-ip.yml". You can test with: 
  for octet in `echo "130 131 132 133 134 135 136 137 138 139"`; do ansible-playbook -i hosts test9-ipam-delete-ip.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done

* Create playbook for removing PowerDNS A record via API - skeleton exists called "test7-pdns-remove-name.yml". You can test with:
  for octet in `echo "140 144 148 149 151 152 153 154 156 159 160 161"`; do ansible-playbook -i hosts test9-ipam-delete-ip.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done

