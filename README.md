# Aggregate Resource Example
These demonstration playbooks compare using `with_items` to `aggregate` for the [eos_vlan](https://docs.ansible.com/ansible/latest/eos_vlan_module.html) Ansible module.  They were tested on Arista EOS but are written in a way where the networking platform can be easily changed by editing the `ansible_network_os` variable.

For this scenario we will assume a network operator wants to configure 500 VLANs on a Arista EOS device.  We can use the eos_vlan module to easily accomplish this.  The `with_items` can run the eos_vlan for each VLAN.

Why use aggregate?  There is significant speed savings by using aggregate.  Instead of looping over each item, the list of VLANs is sent as one data structure.

## Using loop Method
The loop method will run the task eos_vlan X times where X is the amount of VLANs.  This means with 500 VLANs we are running the eos_vlan task 500 times.

Run the playbook that uses a `with_items` loop like this:
```bash
ansible-playbook oldway.yml -k
```
To view the playbook [click here](oldway.yml).

## Using aggregate Method
The aggregate method will run the task once (vs the loop method running X times) and send the list of VLANs in bulk as one data structure.

Run the playbook that uses the `aggregate` like this:
```bash
ansible-playbook newway.yml -k
```
To view the playbook [click here](newway.yml).

## Using aggregate Method with purge
The purge method will remove all VLANs not sent with the aggregate.  For example if there was 300 VLANs configured (VLANs 1-300) on the switch prior to the playbook running and the aggregate had 1-200 sent as part of the task, The VLANs 201-300 would be removed.

Run the playbook that uses the `aggregate` with `purge` like this:
```bash
ansible-playbook purge.yml -k
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

If you want to play with the `network_cli` feature or other features coming in Red Hat Ansible 2.5 try installing the dev branch->
```
bash
pip install git+https://github.com/ansible/ansible.git@devel
```

Detailed directions are available at:
http://docs.ansible.com/ansible/latest/intro_installation.html#latest-releases-via-pip  

---
![Red Hat Ansible Automation](images/rh-ansible-automation.png)

Red Hat® Ansible® Automation consists of  three products:

- [Red Hat® Ansible® Tower](https://www.ansible.com/tower): Built for operationalizing and scaling automation, managing complex deployments and speeding up productivity. Extend the power of Ansible Tower with Workflows and Surveys to streamline jobs and simple tools to share solutions with your team.

- [Red Hat® Ansible® Engine](https://www.ansible.com/ansible-engine): a fully supported product built on the foundational capabilities of the Ansible project. Also provides support for select modules including Infoblox.

- [Red Hat® Ansible® Network Automation](https://www.ansible.com/networking): provides support for select networking modules from Arista (EOS), Cisco (IOS, IOS XR, NX-OS), Juniper (JunOS), Open vSwitch, and VyOS. Includes Ansible Tower, Ansible Engine, and curated content specifically for network use cases.
