switch_provision
=========

Stages switches by pre assigning node IDs to serial number


Role Variables
--------------

**Required Variables**

**switches**([]): list of switches.  each requires the following keys
* name(str): switch name
* serial(str): serial number
* nodeId(str): node id number of switch
* role(str): whether leaf or spine
* podId(str): pod ID number

##Example##

```yaml
---
switches:
- name: leaf101
  serial: ""
  nodeId: "101"
  role: leaf
  podId: "1"
- name: spine200
  serial: ""
  nodeId: "200"
  role: spine
  podId: "1"
```
