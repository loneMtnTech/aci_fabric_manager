switch_profiles
=========

Creates switch and interface profiles for a given range of nodes.Assumes leaf nodes are sequential and you want even-odd vpc peers.  Supports fex attachments.  For example, with a range of 101-104 you would get:

101
101-102
102
103
103-104
104

## Fex support
fex support creates a int profile such as 101-fex.  It assumes that 101-fex is attached to leaf 101.  There is no customization to this.  Also, leaf node ranges do not support the 200 range so this requires your leaf IDs to be in the 100 range to use this feature.


Role Variables
--------------

leafStart(int): first node that is a leaf

leafEnd(int): last node that is a leaf

fexLeaf(list): leaf nodes that have fex attached.  Fex int profile will be named *nodeID-fex*

## Examples

![switch profiles](/images/switch_profiles.JPG)
