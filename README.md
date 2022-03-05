# proxmox ansible scripts

## proxmox-remove role
$ ansible-playbook -i hosts playbooks/proxmox_remove.yml --extra-vars '{"VM_name":"testvm130","Node_name":"pve1"}'

* Remove VM's requires extra-vars to set global variables. 
* Also the VM_name and Node_name variables must have matching "hosts" file entries
* Currently you must manually remove the DNS name from dns.bustraan.lab server (PowerDNS) and IP address from phpIPAM (ipam.bustraan.net) server


## proxmox-deploy role
 
$ ansible-playbook -i ./hosts playbooks/proxmox_deploy.yml
* Deploy VM's requires vars set interactively from the CLI.  Follow the prompts
** added sub-bullet 1
** added sub-bullet 2

---

## ipam delete playbook using extra-vars
# code block
for octet in `echo "130 131 132 133 134 135 136 137 138 139"`; do ansible-playbook -i hosts test9-ipam-delete-ip.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done
for octet in `echo "140 144 148 149 151 152 153 154 156 159 160 161"`; do ansible-playbook -i hosts test9-ipam-delete-ip.yml --extra-vars "ansible_default_ipv4=192.168.1.$octet"; done

