Fabric_basics
=========

Equivalent to the quick start wizard.  Configures dns,ntp and bgp route reflectors

*This role does not populate defaults, because you do not want to accidentally change your bgp AS #.  You will need to define variables before it will run


Role Variables
--------------
**Required Variables**

**bgpAs**(str): BGP autonomous system number

**ntpServers**([str]): list of IP addresses for ntp servers

**dnsServers**([str]): list of IP addresses for dns servers

dnsDomains([str]): list of dns suffixes to appead to relative dns queries

**routeReflectors**([str]): list of node IDs to become bgp route reflectors
