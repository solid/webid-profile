### Discovering Storage Spaces

An app wishing to find out about the WebID owner's storages should look for triples with the WebID as subject, `pim:storage` as predicate, and the container at the root of the storage as the object.  For example :
```
   <#WebID> <http://www.w3.org/ns/pim/space#storage> </> .
```
In this example the storage space and the document which references it are located on the same host.  This is not required, the storage space may be on any host.

If no storage space is found through profile triples, the app MAY use the process described in [Solid Protocol 0.9](https://solidproject.org/TR/protocol#storage) to find the closest storage to the WebID document and check for a link header marking the storage as owned by the owner of the WebID.  There is no guarantee this will succeed.

If no storage space is found through either the profile triples or finding the closest storage, the only option is to ask the WebID owner. 

Applications SHOULD NOT try guessing a storage location based on the WebID URI.

Developers should be aware that the WebID owner can make `pim:storage` statements in either public or private documents and that therefore, the app can only ever know about the existence of storages the WebID owner has given them consent to know about.  Knowing about the existence of a storage does not imply permission to read or write to that storage.
