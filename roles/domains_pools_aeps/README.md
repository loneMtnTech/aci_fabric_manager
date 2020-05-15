domains_pools_aeps
=========

Manages cisco aci domains,pools and aeps and the relationship between them


Role Variables
--------------

**Required Variables**

vlanPools([]): list of vlan pools.  by default will create the following:

```
prod:
    vlan: 1000-1002
dev:
    vlan: 50-52
vmm:
    vlan: 1500-1502
legacy:
    vlan: 500,501,502
l3-out:
    vlan: 4000-4002
```

domains([]): list of domains.  By default will create the following domains and map to the corresponding vlan pool:

```
prod
dev
vmware-aci
l3-out
legacy
```

aeps([]): list off Attachable entity profiles.  By default will create the following aeps:

```
phy:
    domains: prod,dev
vmware:
    domains: vmm
l3-out:
    domains: l3-out
```
