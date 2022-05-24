## Applications - acl:trustedApp

The `acl:trustedApp` predicate is used by some Solid Servers to grant pod-wide access to apps. It prevents the app from being blocked by [CORS](TBD) and also authorizes the app for specified access modes. In most cases, the predicate is managed by the server or by by the user (for example with the [SolidOS Databrowser](https://github.com/SolidOS/userguide#manage-your-trusted-applications)).

On servers which support the `acl:trustedApp` predicate, the first time a user visits the server, they are greeted with a popup prompting the user to grant the app one or more access control modes - Read, Append, Write, and/or Control (see [WAC Specification](TBD)). The server then stores the user's response as an `acl:trustedApp` triple in their WebID Profile Document.

A Pod Management App can provide forms for users to manage their trusted apps.  These forms should write triples in this format :
```
<#me> acl:trustedApp
  [
    acl:mode acl:Append, acl:Read, acl:Write;
    acl:origin <https://example.com>
  ].
```
The `acl:mode` can include one or more of the access control modes.  The `acl:origin` should be the origin of the app.  The origin is automatically sent by browsers and used by servers to allow or block access.  It is the domain of the server on which the app is located, not a path so therefore should not have a trailing slash.

Apps MUST NOT create new or update existing `acl:trustedApp` triples except through forms submitted by users.  The whole point of the triple is that the **user** gets to decide which apps to trust and what to trust them for.  So apps should never self-declare that a user trusts them.  

For apps other than pod-mangagement apps, there are only two things the app might need to do in realation to `acl:trustedApp` : 1) the app might want to request the user to select specific access modes before the popup appears. 2) In the rare instance a mistake is made, the app might want to delete the `acl:trustedApp` triple for your app. 

If a mistake is made, the app MAY delete the `acl:trustedApp` triple for itself.  This will cause the server to prompt the user the next time they use the app and a new `acl:trustedApp` triple will be created for the app.

When access modes are granted to an app using the `acl:trustedApp` triple, the grant is pod-wide.  This means that apps SHOULD NOT request users to grant Control or Write access unless they are intended as a Pod Management App. General purpose apps such as a photo app, will not need Control/Write access over the entire pod and can, instead, request access to specific resources.  

The method to automatically request app access to specific resources is not currently covered by any specification and varies from server to server.  The only method sure to work on all servers is for your app to prompt the user to use a Pod Management App to manually create the containers and set the access modes for your app.  The upcoming [Interoperability Specification](TBD) will propose a solution to this problem.  

