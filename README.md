# Ansible Collection for Cisco ACI Access Policies
A framework to deploy cisco ACI fabric access policies

The goal of this collection is to achieve a uniform and quickly deployed base fabric.
It is an opinonated framework that can help you acheive a well deployed fabric as long as you agree with the fabric concepts

## Features

 * Creates vlan pools, domains, aeps and connects them based on your variable set. [See Role](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/roles/domains_pools_aeps)
 * Creates Switch,Int and Fex Profiles.  [See Role](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/roles/switch_profiles)
 * Creates Interface policies and groups .  [See Role](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/roles/interface_policies)
 * Enable global best practice settings. [See Role](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/roles/global_settings)
 * Configure dns,ntp,bgp for fabric. [See Role](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/roles/fabric_basics)

## Requirements
* ansible v2.9 or later
* python 3
* ACI user with certificate auth enables
    * https://docs.ansible.com/ansible/latest/scenario_guides/guide_aci.html#signature-based-authentication-using-certificates

## Installation

```pip3 install ansible```

Installation via galaxy.  Use --force to upgrade to latest version

```ansible-galaxy collection install lonemtntech.aci_fabric_policies```

## Variables

**Required Variable**

* **intent**(str: *add/remove* default: add) - controls if infrastructure is deployed or deleted.  
* **varprompt_private_key**(str) - path to authentication certificate
    * should name as *username.key* where username is your aci user acount
    * place in same directory as your root playbook or specify the path in the variable
* apic_proxy(str: default: no)
* apic_validate_certs(str: default: no)
* apic_use_ssl(str: default: yes)
* In addition, check each role's documentation that you are using for required variable. See `defaults/main.yml` 

## Deployment

**Please test against a sandbox**

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
  - role: global_settings
  - role: fabric_basics
```
## Role Dependencies

You may only run specific roles.  Any dependencies will be listed here

<table>
<tr>
<th>Role</th>
<th>Depends On</th>
</tr>
<tr>
<td>domains_pools_aeps</td>
<td>None</td>
</tr>
<tr>
<td>fabric_basics</td>
<td>None</td>
</tr>
<tr>
<td>global_settings</td>
<td>None</td>
</tr>
<tr>
<td>interface_policies</td>
<td>None</td>
</tr>
<tr>
<td>switch_profiles</td>
<td>None</td>
</tr>
</table>

## Configuration

Collections are installed locally here

`~/.ansible/collections/ansible_collections/lonemtntech`

You will find the default variables inside /roles/*rolename*/default/main.yml
The easiest way to customize is to create a [host_vars folder](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#organizing-host-and-group-variables) and override the default variables

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

## [Examples](https://github.com/loneMtnTech/aci_fabric_policies/tree/master/examples)


