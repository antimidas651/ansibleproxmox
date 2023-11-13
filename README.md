# Ansible for Proxmox
Ansible configuration for rapid deployment of Linux VMs to Proxmox

This code was borrowed from https://vectops.com/2020/01/provision-proxmox-vms-with-ansible-quick-and-easy/

I have an error about too many parameters being passed, but I am not exactly sure how that is being caused.  Output of command execution is below.  It is creating the VM.  It jsut does not appear to be turning it on.

antimidas@u23-desktop:~/vscode/ansibleproxmox$ ansible-playbook -i hosts.ini playbooks/proxmox_deploy.yml 
Node Password: 
VM name: 231113
Network associated to ipconfig0 [vlan10]: 
VM IP [192.168.50.2]: 
VM socket/s [1]: 
VM core/s [1]: 
VM RAM Memory (MB) [1024]: 
Increase virtio0 disk (20 GB) in [0]: 
Migrate Virtual Machine to [none]: 

PLAY [prep proxmox hosts for automation] **************************************************************************************************************************************************************************************

TASK [proxmox_deploy : Cloning virtual machine from "mantic-server-cloudimg-amd64" with name "231113"] ************************************************************************************************************************
changed: [proxmox]

TASK [proxmox_deploy : Increasing disk if it is necessary] ********************************************************************************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_INCREASE_DISK }}" != "0"
skipping: [proxmox]

TASK [proxmox_deploy : Waiting to apply cloud init changes in disk] ***********************************************************************************************************************************************************
fatal: [proxmox]: FAILED! => {"changed": false, "msg": "The wait_for action failed to execute in the expected time frame (5) and was terminated"}
...ignoring

TASK [proxmox_deploy : starting new Virtual Machine to change IPv4 configuration, it is necessary] ****************************************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_INCREASE_DISK }}" != "0"
skipping: [proxmox]

TASK [proxmox_deploy : Waiting to start virtual server machine completely] ****************************************************************************************************************************************************
skipping: [proxmox]

TASK [proxmox_deploy : Resize disk] *******************************************************************************************************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_INCREASE_DISK }}" != "0"
skipping: [proxmox]

TASK [proxmox_deploy : stopping new Virtual Machine to change IPv4 configuration, it is necessary] ****************************************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_network }}" != "vlan10" or "{{ VM_INCREASE_DISK }}" != "0"
skipping: [proxmox]

TASK [proxmox_deploy : Loading set up for Virtual Machine. Assigning correct bridge in network interface] *********************************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_network }}" != "vlan10"
skipping: [proxmox] => (item={'key': 'params', 'value': {'netmask': 24, 'vmbr': 0, 'gateway': '192.168.50.1', 'dnsservers': '192.168.50.253', 'searchdomain': 'antimidas.net'}}) 
skipping: [proxmox]

TASK [proxmox_deploy : debug] *************************************************************************************************************************************************************************************************
ok: [proxmox] => (item={'key': 'params', 'value': {'netmask': 24, 'vmbr': 0, 'gateway': '192.168.50.1', 'dnsservers': '192.168.50.253', 'searchdomain': 'antimidas.net'}}) => {
    "msg": "item.key params item.value {'netmask': 24, 'vmbr': 0, 'gateway': '192.168.50.1', 'dnsservers': '192.168.50.253', 'searchdomain': 'antimidas.net'} item.value.netmask 24 item.value.vmbr 0"
}

TASK [proxmox_deploy : Loading set up for Virtual Machine. Assigning IP, sockets, cores and memory for Virtual Machine] *******************************************************************************************************
[WARNING]: conditional statements should not include jinja2 templating delimiters such as {{ }} or {% %}. Found: "{{ VM_IP }}" != "automatic"
failed: [proxmox] (item={'key': 'params', 'value': {'netmask': 24, 'vmbr': 0, 'gateway': '192.168.50.1', 'dnsservers': '192.168.50.253', 'searchdomain': 'antimidas.net'}}) => {"ansible_loop_var": "item", "changed": true, "cmd": "A=$(qm list |grep \"231113\" | awk '{print $1}'); qm set $A - ipconfig0 'ip=192.168.50.2/24,gw=192.168.50.1' - nameserver '192.168.50.253' - searchdomain 'antimidas.net' - memory '1024' - sockets '1' - cores '1'", "delta": "0:00:00.565300", "end": "2023-11-13 07:46:47.297136", "item": {"key": "params", "value": {"dnsservers": "192.168.50.253", "gateway": "192.168.50.1", "netmask": 24, "searchdomain": "antimidas.net", "vmbr": 0}}, "msg": "non-zero return code", "rc": 255, "start": "2023-11-13 07:46:46.731836", "stderr": "400 too many arguments\nqm set <vmid> [OPTIONS]", "stderr_lines": ["400 too many arguments", "qm set <vmid> [OPTIONS]"], "stdout": "", "stdout_lines": []}

PLAY RECAP ********************************************************************************************************************************************************************************************************************
proxmox                    : ok=3    changed=1    unreachable=0    failed=1    skipped=6    rescued=0    ignored=1   

antimidas@u23-desktop:~/vscode/ansibleproxmox$ 
