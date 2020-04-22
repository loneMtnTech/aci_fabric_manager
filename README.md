# Ansible Collection for Cisco ACI Access Policies
A framework to deploy cisco ACI access policies

The collection includes roles and playbooks to achieve a uniform and quickly deployed fabric.
It is a framework that takes some liberties that you must accept to use.  It is customizable via varibles

*framework maxims*

* Switch profile = interface profile topology
    * You set the start and end range of your leaf nodes, it will deploy the single and vpc pair of switch profiles
    * Format for those is *nodeId* and *nodeId-nodeId*
* Int Policies aren't included in var files, the are hardset in the playbook because the are just on/off switches
    * Format for those are all lower case with - separate.  ex: cdp-true, cdp-false
and int profiles
![switch policies](/images/fabric_policies.JPG)


## Requirements
* ansible v2.9 or later
* cisco.aci collection
* python 3
* ACI user with certificate auth enables
    * https://docs.ansible.com/ansible/latest/scenario_guides/guide_aci.html#signature-based-authentication-using-certificates

## Installation
Ansible must be installed.  I will only be testing against python3

```pip3 install ansible```

Installation via galaxy with download any collections this collection depends on if its not already installed

```ansible-galaxy collection install lonemtntech.aci_fabric_policies```


## Configuration
I would encourage a first deployment to a sandbox with the default settings to see how the project is designed to be deployed.

There are 2 required variables to be set on the main playbook
* intent - controls if infrastructure is deployed or deleted
* varprompt_private_key - controls auth to aci.
    * should name as *username.key*
    * place in same directory as your root playbook.



example playbook

```
- hosts: apic
  collections:
  - lonemtntech.aci_fabric_policies
  gather_facts: no
  vars_prompt:
    - name: intent
      prompt: "add/remove APIC Policy"
      default: add
      private: no
    - name: varprompt_private_key
      prompt: "private key name (ex admin.key)"
      private: no
  roles:
  - role: interface_policies
  - role: domains_pools_aeps
  - role: switch_profiles
```

## Customization

while you can modify the roles directly its designed to be modified via variables.
You can access the default yaml variable files inside the collection.  The default path is

`~/.ansible/collections/ansible_collections/lonemtntech`

You will find the default files inside /roles/rolename/default/main.yml. copy that to your inventory variables and any changes you make will override the defaults.
I do make use of yaml anchors to add links between variables.  For example, a domain can reference a vlan pool this way.  The rest is standard yaml

```
vlanPools:
- &prodVlanPool
  name: prod
  alloc: static
  vlans:
  - blockStart: 1000
    blockEnd: 1099

domains:
- &prodDomain
  name: prod
  domain_type: phys
  vlan:
    *prodVlanPool
```
This allows me to place the vlanPools object inside the vlan mapping inside the domain

Most of the plays in the roles are loops so you can create however many domains you want and they will be created accordingly
