# Ansible Collection for Cisco ACI Access Policies
A framework to deploy cisco ACI fabric access policies

The collection includes roles and playbooks to achieve a uniform and quickly deployed fabric.
It is a framework.  The number #1 rule of frameworks is 'don't fight the framework".  That means if you are deploying a different type of fabric this may not work at all for you.

## Features

* Creates your vlan pools, domains, aeps and connects them based on your variable set. [See Role](/roles/domains_pools_aeps/README.md)
* Creates your Switch and Int Profiles.  [See Role](/roles/switch_profiles/README.md)
* Creates your Interface policies and groups .  [See Role](/roles/interface_policies/README.md)

## Example

![fabric policies](/images/fabric_policies.JPG)


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


## Deployment

**Please test against a sandbox**

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

## Configuration

If you have installed via galaxy you can access the default yaml variable files inside each role.  The default path is

`~/.ansible/collections/ansible_collections/lonemtntech`

You will find the default variables inside /roles/*rolename*/default/main.yml
The easiest way to customize is to create a [host_vars folder](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#organizing-host-and-group-variables),paste in the default yaml file from the role then make changes

yaml anchors are used sometime to add relationship between obj in the variable files.  For example, a domain can reference a vlan pool this way.  The rest is standard yaml

*anchor example map prodVlanPool inside the vlan mapping of prod domain*

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

Almost all plays in the roles are loops so you can create however many domains,vlanPools, aeps, switches, etc that you want and they will be created according to your inventory variable file
