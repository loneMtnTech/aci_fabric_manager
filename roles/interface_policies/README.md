interface_policies
=========

Variables are used to create the policy-groups.  The individual policies that are true/false toggles are hardcoded in the playbooks.  If there is one you would like added please submit an issue or pull request

Role Variables
--------------

intPolicyGroups(list): policy-groups to be created

Current Attributes
-------------------

cdp
lldp
mcp
bpdu-guard
speed=[auto,1G,10G]

