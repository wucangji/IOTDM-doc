How to add a new resource in iotdm
============

[Note] Here we only discuss how to add a new <resource> without affecting the Yang model. Some complicated <resource> such as "AccessControlPolicy" may need to update the yang model.


##1. Overall Procedure

Let's take the example of how to support a new <Node> resource in the iotdm:

* Read TS 0001 about the node resource part.

In table 9.6.1.1 we find

ResourceType | short description | child resourceType | parent resourceType
----- | --- | --- | ---- |
node	|  Represents specific Node information |  mgmtObj,subscription | CSEBase, remoteCSE

then we have a basic understanding where this <Node> resource might be stored in our iotdm. According to the above message, this <Node> resource is similar to <Container> resource. [This part of information may not be 100% up-to-date in the TS]


* look at the XSD file or the generated JAVA file with such XSD.
[https://github.com/wucangji/iotdm-client-master/tree/master/src/main/java/org/opendaylight/iotdm/primitive]()

For example, if we want to create a new "Node" resource, we can find the file "Nod.java", then we can have a basic understanding of its attribuetes and child resoures. We will use the short name for its attributes.

* add a new resource java class under *"onem2m/onem2m-core/src/main/java/org/opendaylight/iotdm/onem2m/core/resource"*, similar to other java classes under the same foler.

* Look at the TS 00001 to see the detail information of this resource. In the <Node> example, see 9.6.18. According to Figure 9.6.18-2, we know that this <Node> resource contains some unique attributes that only belong to itself such as "nodeID", such attributes will show in the file "ResourceNode" we created.

## Implementaion
Take the example of <ResourceNode>:

1. Create a java class called ResourceNode under *"onem2m/onem2m-core/src/main/java/org/opendaylight/iotdm/onem2m/core/resource"*
![](AddResource.png)

2. Add a new Resourcetype Integer and ResourceType String (check TS0004 6.3.3.2.1, 8.2.4-1 )in "Onem2m.ResourceTypeString"
![](AddResourceTypeInOnem2m.png)

3. Add support in the "ResourceContent.java"
![](ResourceContentUpdate.png)
![](AddSupportInContent.png)
There are several other places need to be modified in this file.
Need to consider both json input and json output support.

2. Add status information for this new resource. This stats is used for some analysis in the future.


2. Add the logic part related with this new Resource. In the <Node> example, the related resource is <CSEBase> and <RemoteCSE> etc, we may need to modify the "CRUD" functions of affected resources inside these resources' java file.

3. 