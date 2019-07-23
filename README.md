# Network Facts
Grab Facts from Network Devices

## Overview

Network Facts is an Ansible Role for network engineers and network operators.  
This role gathers facts from network devices based on the value of variable `ansible_network_os` and store the facts under `ansible_facts`.  
The following values are accepted for `ansible_network_os`: 
  - bigip
  - ce
  - cnos
  - dellos10
  - dellos6
  - dellos9
  - edgeos
  - enos
  - eos
  - exos
  - ios
  - iosxr
  - junos
  - netconf
  - nos
  - nxos
  - onyx
  - routeros
  - tmos
  - voss
  - vyos

## Quick start

### Download from ansible galaxy

1) Download the role from ansible-galaxy into your roles directory
```BASH
ansible-galaxy install -p roles nmake.network_facts
```
2) Call the role by loading it at the top of your playbook

```YAML
- hosts: network
  gather_facts: no
  roles:
    - role: nmake.network_facts
```
3) Run your playbook

## Howto
### Variables
```YAML
---
# List of tasks to run before doing anything.
network_facts_pre_tasks: []

# List of roles to run before doing anything.
network_facts_pre_roles: []

# Network Operating System
network_facts_os: "{{ ansible_network_os | default(None, True) }}"

# Network Platform
network_facts_platform: "{{ ansible_network_platform | default(None, True) }}"

# Function to run: Only have main
network_facts_function: "{{ function | default('main') }}"

# List of tasks to run after doing anything.
# These will always run, regardless if there is any fail when rolling out operations.
network_facts_post_tasks: []

# List of roles to run after doing anything.
# These will always run, regardless if there is any fail when rolling out operations.
network_facts_post_roles: []
```

### Inventory Structure
we recommend to have an inventory group for each platform/os in order to apply variables that are specific for Network Operating Systems (`ansible_network_os`) and Platforms (`ansible_network_platform`). Follow below an Example of inventory file in YAML format:  

```YAML
---
all:
  hosts:
    lb01-tmos:
    rtr01-ios:
    rtr02-iosxr:
    rtr03-junos:
    rtr04-vyos:
    rtr05-netconf:
    sw01-nxos:
    sw02-eos:
    sw03-junos:
    sw04-netconf:
  children:
    network:
      children:
        tmos:
          hosts:
            lb01-tmos:
        ios:
          hosts:
            rtr01-ios:
        iosxr:
          hosts:
            rtr02-ios:
        junos:
          hosts:
            rtr03-junos:
            sw03-junos:
        vyos:
          hosts:
            rtr04-ios:
        netconf:
          hosts:
            rtr02-ios:
            rtr03-junos:
            rtr05-netconf:
            sw03-junos:
            sw04-netconf:
        nxos:
          hosts:
            sw01-nxos:
        eos:
          hosts:
            sw02-eos:

```
