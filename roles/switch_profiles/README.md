switch_profiles
=========

Creates switch and interface profiles for a given range of nodes.Assumes leaf nodes are sequential and you want even-odd vpc peers.  Supports fex attachments.  For example, with a range of 200,205 you would get:

200
200-201
201
202
202-203
203
204
204-205
205


Role Variables
--------------

leafStart(int): first node that is a leaf

leafEnd(int): last node that is a leaf

fexLeaf(list): leaf nodes that have fex attached.  Fex int profile will be named *nodeID-fex*

## Examples

![switch profiles](/images/switch_profiles.JPG)
