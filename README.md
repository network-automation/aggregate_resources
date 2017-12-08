# Aggregate Examples
These demonstration playbooks compare using `with_items` to `aggregate` for the [eos_vlan](https://docs.ansible.com/ansible/latest/eos_vlan_module.html) Ansible module.  They were tested on Arista EOS but are written in a way where the networking platform can be easily changed by editing the `ansible_network_os` variable.

For this scenario we will assume a network operator wants to configure 500 VLANs on a Arista EOS device.  We can use the eos_vlan module to easily accomplish this.  The `with_items` can run the eos_vlan for each VLAN.

Why use aggregate?  There is significant speed savings by using aggregate.  Instead of looping over each item, the list of VLANs is sent as one data structure.

## Using loop Method
The loop method will run the task eos_vlan X times where X is the amount of VLANs.  This means with 500 VLANs we are running the eos_vlan task 500 times.

Run the playbook that uses a `with_items` loop like this:
```bash
ansible-playbook oldway.yml
```
To view the playbook [click here](oldway.yml).

## Using aggregate Method
The aggregate method will run the task once (vs the loop method running X times) and send the list of VLANs in bulk as one data structure.

Run the playbook that uses a `aggregate` loop like this:
```bash
ansible-playbook newway.yml
```
To view the playbook [click here](newway.yml).

# Notes
These playbooks were developed on the Ansible dev branch:
```
ansible 2.5.0
  config file = /root/aggregate_resources/ansible.cfg
  configured module search path = [u'/usr/lib/python2.7/site-packages/napalm_ansible']
  ansible python module location = /usr/lib/python2.7/site-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.5 (default, Aug  4 2017, 00:39:18) [GCC 4.8.5 20150623 (Red Hat 4.8.5-16)]
```
They are also using the new connection method: `network_cli`.  There is no requirement to use the new connection method and simply used for illustration purposes only.  

 ---
![Red Hat Ansible Automation](rh-ansible-automation.png)

Red Hat® Ansible® Automation includes three products:

- [Red Hat® Ansible® Engine](https://www.ansible.com/ansible-engine): a fully supported product built on the foundational capabilities of the Ansible project.

- [Red Hat® Ansible® Networking Add-On](https://www.ansible.com/ansible-engine): provides support for select networking modules from Arista (EOS), Cisco (IOS, IOS XR, NX-OS), Juniper (Junos OS), Open vSwitch, and VyOS.

- [Red Hat® Ansible® Tower](https://www.ansible.com/tower): makes it easy to scale automation, manage complex deployments and speed productivity. Extend the power of Ansible with workflows to streamline jobs and simple tools to share solutions with your team.

Want more info?
[Read this blog post for more info about Engine, the networking add-on and Tower](https://www.ansible.com/blog/red-hat-ansible-automation-engine-vs-tower)
