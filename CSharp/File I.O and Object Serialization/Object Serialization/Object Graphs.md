- The .NET Runtime will account for all related objects to ensure that public data is persisted correctly when an object is serialized. This set of related objects is referred to as an object graph. Object graphs provide a simple way to document how a set of items refer to each other. Object graphs are not denoting OOP is-a or has-a relationships. Rather, you can read the arrows in an object diagram as “requires” or “depends on".
- Each object in an object graph is assigned a unique numerical value. Keep in mind that the numbers assigned to the members in an object graph are arbitrary and have no real meaning to the outside world. Once you assign all objects a numerical value, the object graph can record each object’s set of dependencies.
![[Pasted image 20240724091238.png|center|300]]