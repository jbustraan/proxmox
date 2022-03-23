# Ansible playbooks and roles for proxmox deploy and remove operations

A set of Ansible playbooks and roles used to create virtual machines on a Proxmox VE cluster. These scripts use PowerDNS API and phpIPAM API to provision new IP space and register DNS names.  Script requires two inputs 1) VM name and 2) Proxmox cluster to use for provisioning.  Virtual machines are built from AlmaLinux-8-GenericCloud-latest image so machine is simple disk layout with a single root partition.  Cloud init images are used to allow passing configuration from files user-data.yml, network-config.yml, vendor-data.yml and meta-data.yml.  These files are stored as an ISO snippet in Proxmox storage.  Scripts apply ansible and my user RSA keys, provide sudo access, install utils (python3, wget, unzip), and do "dnf update" on initial creation.

## proxmox-deploy role - to deploy new VMs to cluster, do the following
 
$ ansible-playbook -i ./hosts playbooks/proxmox_deploy.yml
* Deploy VM's requires vars set interactively from the CLI.  Follow the prompts to provide VM_name and PVE cluster nodes (pve1 or pve2)
* It is a good idea to add the new host to /etc/ansible/hosts and local directory hosts file, if you intend to leave the VM running.

## proxmox-remove role - to remove a VM from cluster nodes, do the following
$ ansible-playbook -i hosts playbooks/proxmox_remove.yml --extra-vars '{"VM_name":"cloudVM-01","Node_name":"pve1"}'

* Remove VM's requires "--extra-vars" to set global variables to override internal variables.
* Also the VM_name and Node_name variables must have matching "hosts" file entries (./hosts.txt)
* Fixed issue with requirement to manually remove the DNS name from PowerDNS server and IP address from phpIPAM (see additional features below).


## Additional features needed
* Create playbook for removing IPAM IP entry - skeleton exists called "test9-ipam-delete-ip.yml". You can test with: 
  for octet in `echo "130 131 132 133 134 135 136 137 138 139"`; do ansible-playbook -i hosts test9-ipam-delete-ip.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done

* Create playbook for removing PowerDNS A record via API - skeleton exists called "test7-pdns-remove-name.yml". You can test with:
  for octet in `echo "140 144 148 149 151 152 153 154 156 159 160 161"`; do ansible-playbook -i hosts test7-pdns-remove-name.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done

