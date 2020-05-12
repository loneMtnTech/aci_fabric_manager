Global_Settings
=========

Enables various global settings that are considered standard best practice


Role Variables
--------------
*any booleans should be quoted to make string*

mcp: [enabled|disabled] mis cabling protocol

mcpPass: password for mis cabling protocol

domainValidation: [yes|no] forces aci to check that an EPG is linked to a domain.  Once it is on you cannot undo

remoteEpLearning: [yes|no] Controls remote *IP* endpoint learning for border leaf switches

enforceSubnetCheck: [yes|no] Limits both local and remote endpoint learning to IP that belong to a bridge domain

isisMetric: "32" prevents blackholes during spine reboots

bfd: [enabled|disabled] Bidrectional forwarding allows sub second recovery on Layer 3 links

ipAging: [enabled|disabled] ages unused IP address for an endpoint.  Uses Arp request to make sure IP is still in use

rogueEpCtrl: [enabled|disabled] quarntines an endpoint that is causing a loop

forceCoopAuth: [yes|no] forces md5 authentication to access the COOP database

changeForwardingProfile: [yes|no] changes forwarding profile to ipv4 which allows for up to 48000 ipv4 endpoints and no ipv6 endpoints.  The default is 12000ipv6 and 24000 ipv4.  *This requires a switch reboot to go into effect, it will not reboot on its on*
