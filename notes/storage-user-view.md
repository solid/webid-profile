**Note** This is a first attempt at recommendations for users.  It's unclear if these belong in this document or not.

### Making Storages Discoverable

A WebID owner MAY have zero or more storage spaces associated with the WebID and MAY choose, for each one, to make it discoverable by the general public, by a selected audience, or not at all. If a storage space is discoverable by someone, that only means they can know it exists.  It does not imply that they have read or write access to it - those permissions are defined elsewhere.

WebID owners who wish their storage spaces to be discoverable by the general public, SHOULD advertise the space publicly by having a triple like this one in their profile document or in a publicly accessible extended profile document.
```
   <#me> <http://www.w3.org/ns/pim/space#storage> </> .
```
The object of the triple may be on the same host as the document the triple occurs in or on a different host.

A storage triple is usually created by the pod provider on pod creation but if it is missing, the WebID owner SHOULD create it if they want the storage discoverable.  

A WebID owner wishing to disallow the public from discovering a storage SHOULD NOT use pim:storage predicates for that storage in any publicly accessible document and should remove any such statements put there by a pod provider.  

A WebID owner wishing to make a private storage space available to selected Agents, SHOULD create a pim:storage triple in a document only accessible by those Agents.

A WebID owner MAY add as many public or private storages as they want.

A WebID owner also has the option to register for a WebID but not have it associated with a storage space.  The WebID could be used for accessing other people's sites without storing any information on a site owned by that identity.
