AAA
=========

Configures cisco Aci authentication method


Role Variables
--------------

**Required Variables**

**extAuthMethod(str)**: the ext auth source to configure, choices are radius,ldap,tacacs,saml,rsa.  *Only radius is working*

oobEpg(str): default=default

**radiusSrvs**([str]): required if extAuthMethod is radius

radiusAuthProtocol(str): default=pap options are pap,chap,ms-chap

**radiusPwd**(str): required if extAuthMethod is radius.  It is the preshare key to the radius servers

To configure radius server please read [security config guide](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/aci/apic/sw/2-x/Security_config/b_Cisco_APIC_Security_Configuration_Guide/b_Cisco_APIC_Security_Guide_chapter_01011.html#concept_39DDAF14865F4ACB801724133305E228) in regards to the correct cisco-av-pair usage

To use local auth once this is complete you need to use this format in UserID of GUI

```apic:fallback\\local username```