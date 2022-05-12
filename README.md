## proxmox - A set of Ansible roles to create and destroy virtual machine on Proxmox VE.

A set of Ansible playbooks and roles used to create virtual machines on a Proxmox VE cluster. These scripts use PowerDNS API and phpIPAM API to provision new IP addresses and register new DNS name.

The script requires two inputs:
1. VM name
2. Proxmox cluster to use for provisioning.

Virtual machines are built from AlmaLinux-8-GenericCloud-latest image so machine is simple disk layout with a single root partition.  Cloud init images are used to allow passing configuration from files user-data.yml, network-config.yml, vendor-data.yml and meta-data.yml.  These files are stored as an ISO snippet in Proxmox storage.  Scripts apply ansible user RSA keys, provide sudo access, install utils (python3, wget, unzip), and do "dnf update" on initial creation.

### proxmox-deploy role - to deploy new VMs to cluster, do the following

```$ ansible-playbook -i ./hosts playbooks/proxmox_deploy.yml```
 1. Deploy VM requires vars set interactively from the CLI.
 2. Remember to add new host to */etc/ansible/hosts* if you intend to leave the VM running.

### proxmox-remove role - to remove a VM from cluster nodes, do the following
```$ ansible-playbook -i hosts playbooks/proxmox_remove.yml --extra-vars '{"VM_name":"cloudVM-01","Node_name":"pve1"}'```

 1. Remove VM's requires "--extra-vars" to set global variables to override internal variables.
 2. Also the VM_name and Node_name variables must have matching "hosts" file entries *(/etc/ansible/hosts)*.
 3. DNS name and IP address are deprovisioned automatically.

## Additional features requested
* None at present

