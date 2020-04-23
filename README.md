# Ansible Collection for Cisco ACI Access Policies
A framework to deploy cisco ACI access policies

The collection includes roles and playbooks to achieve a uniform and quickly deployed fabric.
It is a framework that takes some liberties that you must accept to use.  It is customizable via varibles

*framework maxims*

![fabric policies](/images/fabric_policies.JPG)

* Switch profile = interface profile
    * You set the start and end range of your leaf nodes, it will deploy the single and vpc pair of switch profiles
    * Format for those is *nodeId* and *nodeId-nodeId*.  For example, if you have 4 leafs and you start at 200 for the node id
    you will get: 200,200-201,201,202,202-203,203.
    * Control the relationship between domains,pool, and aeps in the variable file via yaml anchors
    * Int Selectors are not created by the roles, this is handled via a playbook that will be added to this collection later.  The idea is to provide an interface for the team racking and stacking that would deploy the Int Sel since they are the one who knows where it is plugged in

![switch profiles](/images/switch_profiles.JPG)


## Requirements
* ansible v2.9 or later
* cisco.aci collection
* python 3
* ACI user with certificate auth enables
    * https://docs.ansible.com/ansible/latest/scenario_guides/guide_aci.html#signature-based-authentication-using-certificates

## Installation
Ansible must be installed.

```pip3 install ansible```

Installation via galaxy will download any collections this collection depends on if its not already installed.  Use --force to upgrade to latest version

```ansible-galaxy collection install lonemtntech.aci_fabric_policies```


## Configuration
I would encourage a first deployment to a sandbox with the default settings to see how the project is designed to be deployed.

There are 2 required variables to be set on the main playbook
* intent - controls if infrastructure is deployed or deleted
* varprompt_private_key - controls auth to aci.
    * should name as *username.key* same as your aci username
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

Designed to be modified via variables.
If you have installed via galaxy you can access the default yaml variable files inside each role.  The default path is

`~/.ansible/collections/ansible_collections/lonemtntech`

You will find the default files inside /roles/*rolename*/default/main.yml
The easiest way to customize is to create a [host_vars folder](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#organizing-host-and-group-variables),paste in the default yaml file from the role then make changes

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
