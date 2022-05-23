# Discovering Resources in a Solid Profile

## Pre-Draft 2022-04-01

> "A city has public places where I can do all kinds of things, and also a
> private house with a private room which may be just mine. In that house there
> are spaces where I do things with family, friends, colleagues. The web must,
> like a well-designed building, provide a gradient of intimacy between the
> private and the public, so I can easily recognize the difference, easily know
> which I am in, and easily welcome people to come into the more intimate parts,
> as I want to." -- timbl

If the web is this well-designed building, the Solid Profile is its lobby - the
place the public can discover which parts of the interior  the building's owner
has given consent for them to see, where the owner can hide or put on display
parts of their identity as they see fit.

### Editors

* Virginia Balseiro
* Tim Berners-Lee
* Sarven Capadisli
* Timea Turdean
* Jeff Zucker

### Created

* 2022-04-01

### Language

* English

### License

* [MIT License](http://purl.org/NET/rdflicense/MIT1.0)

## Status of this document

This is a draft meant for the editors to discuss among themselves what belongs
in the public draft which will almost certainly differ from this.  We intend to
release a public draft once we have finished our discussions.  At the time of
the public draft release, we will welcome and solicit feedback and PRs but ask
commenters other than members of the editorial team wait until that release to
comment.

## Audience and Scope

This document is aimed at application developers. It describes the process Solid
applications can use to discover information about the structure of resources in
a social agent's profile.

This document does not cover the WebID authentication process (see [OIDC
specification]TBD) or data specific to a given type of social agent (see
forthcoming [Creating Personal Profiles]TBD and [Creating Organizational
Profiles]TBD)). Alternate discovery processes (for example [proposed
Interoperability-Spec]TBD) are also out of scope for this document although they
will be mentioned where relevant.

## 1. Introduction

This section is non-normative.

A social agent - a person or organization - can establish an identity in the
Solidverse by obtaining a unique identifier called a `WebID` and by describing
themselves in a set of documents associated with the WebID. These documents,
referred to here collectively as a `Solid Profile`, both describe the social
agent and provide links to their resources.  

A Solid profile, like most Solid resources, can include a combination of
publicly readable data, data restricted to named audiences, and data meant only
for the social agent themselves. For example, Keisha's Solid profile might list
her name publicly, her phone number only for friends, and the configuration
settings for her "notes to self" only for apps operating on her own behalf.
Solid supports these options by splitting the profile into documents with
separate access restrictions. So the first task of an application is to load all
of the profile documents it has access to.

Profiles can contain, and apps are free to follow, any kind of links to related
documents. In order to promote interoperability and limit the burden on apps,
this specification recommends a limited number of related documents which a
well-behaved profile ought to contain and a well-behaved app ought discover.

The discovery process starts with the WebID, a URI that points to exactly one
document, referred to here as a [WebID Profile Document](TBD). This document can
be expected to contain pointers to a `Preferences File Document` containing
settings & resources meant only for the WebID owner, `Type Index Documents`
containing links to specific types of resources, and might also contain
[Extended Profile Documents](TBD) containing additional information about the
WebID owner. The documents which make up a Solid Profile are illustrated below
and described in more detail in [Section 2](TBD).

<img
src="https://github.com/solid/webid-profile/blob/main/notes/discovery2.png">

Once an app has loaded all of the needed profile documents, it can then look for
a fixed set of predicates holding information about the structure and location
of the WebID owner's resources. These predicates are listed below and described
in further detail in [Sections 3-8](TBD).

|predicate|information conveyed|
|-|-|
|solid:oidcIssuer | location(s) where the WebID owner logs in |
|acl:trustedApp | origin & permissions of applications the WebID owner has given access to  |
|solid:storage | location(s) of the WebID owner's storage space(s) |
|solid:instance, solid:instanceContainer | locations of specific types of resources|
|ldp:inbox| location of the WebID owner's Solid inbox |

## 2. Discovering a complete Solid profile

It is possible to have all profile information in a single WebID Profile
Document, but a more likely situation is that some information is in that
document and some is in extended profile documents. With a couple of exceptions
noted below, there is no guarantee that any specific piece of data is located in
a given document. Therefore it is almost always best to load all profile
documents, to do so in a recommended order, and to treat the `Solid Profile` as
a graph assembled by loading documents rather than as a set of documents.

When an app wants to retrieve a complete profile, it SHOULD

1. Load the [WebID Profile Document](TBD) reachable at the WebID URI.
2. If the app is operating on behalf of the WebID owner, find a triple in that
   document with the WebID as subject and `pim:preferencesFile` as predicate,
   then load the object of that triple.
3. Find all triples loaded so far that have the WebID as subject, and either
   `rdfs:seeAlso`, `solid:publicTypeIndex`, or `solid:privateTypeIndex` as
   predicate and load the objects of those triples.

Once all of the accessible extended profile documents have been loaded, the
profile will consist of all statements with the WebID as subject, regardless of
which document the statements occurred in. An app with only public permissions
will see only statements from publicly accessible extended profile documents
while other apps will see statements from public and also private extended
profile documents they have access to.

## 3. Private Preferences - pim:preferencesFile

The `Preferences Document` is a resource intended to hold information only
accessible to an app that is logged in and authenticated as the WebID owner,
including settings and preferences, language and display preferences, and so on
and also user's personal data, be it contacts, pictures or health data.

An app operating on behalf of the owner can gather configuration settings from
the owner, store them in the `Preferences Document`, and then read them there on
subsequent visits. Such an app might also record private information (for
example, a driver's license number) and later, at the direction of the owner,
retrieve the information to fill out a form.

An app operating on behalf of the WebID owner that wants to read or write
preference data SHOULD look in the [WebID Profile Document](TBD) for the
location of the `Preferences Document`. To determine its location the app SHOULD
look for a single triple with the WebID as subject, `pim:preferencesFile` as
predicate and the URL of a document as object. The object containing the URL of
a document is then the location of the `Preferences Document`. If the app finds
a `pim:preferencesFile` triple, it MAY read and/or write to the file as needed
if it has the right permission.

When an app operating on behalf of the WebID owner cannot discover a
`pim:preferencesFile` triple, and has write and control access, and wishes to
write preference data, it MAY create a document accessible only to the WebID
owner and SHOULD insert a triple in the [WebID Profile Document](TBD) with the
WebID as subject, `pim:preferencesFile` as predicate, and the URL of the created
document as object.

When an app wants to store data only accessible to itself, or only to a
specified audience, it SHOULD create an [Extended Profile Document](TBD), give
it the appropriate permissions, and create a triple in the `Preferences
Document` with the WebID as subject, `rdfs:seeAlso` as predicate and the URL of
the created document as object.

## 3. Extended Profile Documents - rdfs:seeAlso

Solid Profile owners may use the `rdfs:seeAlso` predicate to link to extended
profile documents which contain information that they do not want in the [WebID
Profile Document](TBD) or in the `Preferences Document`.  This can be done to
help organize information - for example to keep all friends (objects of
`foaf:knows` predicates) in a separate `rdfs:seeAlso` document. It can also be
done to limit access to the data - for example to store a phone number where
only trusted friends can view it.  

### 3.a Reading Extended Profile Documents

An app wanting to load a complete `Solid Profile` SHOULD examine statements in
the [WebID Profile Document](TBD) and the [Preferences Document](TBD) that have
the WebID as subject, `rdfs:seeAlso` as predicate and the URL of an `Extended
Profile Document` as object. When the app has loaded those two documents, it
SHOULD load the documents specified in the URLs of all `rdfs:seeAlso` triples
found and SHOULD treat all statements in the linked documents that have the
WebID as subject as part of the `Solid Profile`. An app MAY, but is not required
to, examine other statements in the linked documents.

### 3.b Writing Extended Profile Documents

When an app wants to write data in an [Extended Profile Document](TBD), it
SHOULD give the document appropriate permissions depending on the needs of the
WebID owner. If the document is private, the app SHOULD create an `rdfs:seeAlso`
triple pointing to it in the [Preferences File](TBD). If the document is public
or for a restricted audience, the triple SHOULD be created in the [WebID Profile
Document](TBD).

## 4. Type Indexes

### What is a Type Index?

A Type Index is a document containing statements that link specific types of
data to specific locations. An app might ask a user where they want to store
items of type Bookmark and then write that location in the Type Index.
Thereafter another app can find the bookmarks by looking for the location in the
Type Index.

Well-behaved apps will always read from and write to the Type Index so that the
user's choices are remembered. Since users will undoubtedly have data they want
to keep private, there are two Type Indexes - one readable by any app (publicly
discoverable) and one only readable by apps operating on behalf of the WebID
owner (not publicly discoverable).

### Where to find the Type Indexes

To find the Type Indexes, an app SHOULD load the WebID Profile Document and if the app has access, also the Preferences Document. In these loaded documents one can find triples where the WebID as subject and the following predicates: for the Public Type Index document the
`solid:publicTypeIndex` predicate, and for the Private Type Index document the
`solid:privateTypeIndex` predicate.

#### Examples

An example for a Public Type Index Resource that is located in `settings` and is
called `publicTypeIndex.ttl` linked from the profile is:

```
   <#WebID> <http://www.w3.org/ns/solid/terms#publicTypeIndex> </settings/publicTypeIndex.ttl> .
```

An example for a Private Type Index Resourcelocated in `settings` and is called
`privateTypeIndex.ttl` linked from the profile is:

```
   <#WebID> <http://www.w3.org/ns/solid/terms#privateTypeIndex> </settings/privateTypeIndex.ttl> .
```

Here is a diagram showing an example set of Type Indexes: ![Type Registry Index
diagram](../diagrams/type-indexes.svg)

### Public Type Index (usually called publicTypeIndex.ttl)

The Public Type Index document contains registration entries that are *discoverable* by outside users and applications but which are not necessarily *accessible* to all applications or users.  For example, suppose I have one address book that is only for myself, one that is for a specific group of colleagues, and one that is for the general public.  The one that is only for me should be listed in the private type index.  The other two should be listed in the public type index. The fully public listing will point to a publicly accessible resource while the one just for my colleagues will point to a resource restricted by access control methods.  Both the public and restricted reesources are discoverable in the public type index because an app that is not operating as the WebID owner can not see the private type index.

Example of a Public Type Index document. This contains a public resource of type
`vcard:AddressBook` located at the resource address
`</public/contacts/myPublicAddressBook.ttl>` and a resources of type
`bk:Bookmark` located in the container address `</public/myBookmarks/>`:

```publicTypeIndex.ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
# ...
<>
    a solid:TypeIndex ;
    a solid:ListedDocument.

<#ab09fd> a solid:TypeRegistration;
    solid:forClass vcard:AddressBook;
    solid:instance </public/contacts/myPublicAddressBook.ttl>.

<#bq1r5e> a solid:TypeRegistration;
    solid:forClass bk:Bookmark;
    solid:instance </public/myBookmarks/>.
```

### Private Type Index (usually called privateTypeIndex.ttl)

The Private Type Index document contains registration entries that are private
to the user and their apps. It is intended for types of resources meant only to be accessed by the WebID owner.  

Example of a Private Type Index document. This contains a private resource of
type `vcard:AddressBook` located at the resource address
`</private/contacts/myPublicAddressBook.ttl>` and a resources of type
`bk:Bookmark` located in the container address `</private/myBookmarks/>`:

```privateTypeIndex.ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.

<>
    a solid:TypeIndex ;
    a solid:UnlistedDocument.

<#ab09fd> a solid:TypeRegistration;
    solid:forClass vcard:AddressBook;
    solid:instance </private/contacts/myPublicAddressBook.ttl>.

<#bq1r5e> a solid:TypeRegistration;
    solid:forClass bk:Bookmark;
    solid:instance </private/myBookmarks/>.

```

### Type Index registration entries

The type index documents MAY contain any number of statements of type
`solid:TypeRegistration` which map RDF classes/types to their locations in a
WebID owner's dataspace/root storage.

The registration entries map a type to a location and MUST use one of two
predicates:

#### `solid:instance`

maps a type to an individual Solid *resource*, typically an index or a directory
listing resource such as an Address Book.

#### `solid:instanceContainer`

maps a type to a Solid *container* which the client would have to list to get
the instances of that type.

### Supplying missing type index documents

If one or both of the type indexes are missing, an app needing to write to them that doesn't have Write and Control access to the pod SHOULD warn the user that their indexes are missing and that they should use a Pod Management App to fix their profile

If one or both of the type index documents are missing and the app does have Write and Control access to the pod, the app MAY create documents.  The public type index document SHOULD be publicly readable and writable and a pointer to it SHOULD be placed in the WebID Profile Document.  The private type index SHOULD be readable and writable only by the WebID owner and a pointer to it SHOULD be placed in the Preferences Document.

### Reference

* [link 1](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)

## 5. Identity Provider - solid:oidcIssuer

When an app operating on behalf of a WebID owner wants to login, it SHOULD look
for statements with the WebID as subject, `solid:oidcIssuer` as predicate and
the URL of an OIDC Issuer (Identity Provider) as the object. For example:

```
<?WebID> <http://www.w3.org/ns/solid/terms#oidcIssuer> <?Issuer> .
```

If a single statement with the solid:oidcIssuer predicate is found, the app
wanting to login SHOULD redirect to the URL specified by the value of `?Issuer`.

If multiple Issuers are found, the app SHOULD offer the user a choice and
redirect to the chosen `?Issuer`.

If the WebID Profile document does not contain any statements with the
`solid:oidcIssuer` predicate, the application MAY inform the WebID owner that
their WebID document is broken.

## 6. Storage - pim:storage

An app wishing to find out about the WebID owner's storages SHOULD look for
triples with the WebID as subject, `pim:storage` as predicate, and the container
at the root of the storage as the object.  For example:

```
<?WebID> pim:storage <?StorageSpace>.
```

If no storage space is found through profile triples, the app MAY use the
process described in [Solid Protocol 0.9](https://solidproject.org/TR/protocol)
to find the closest storage to the WebID document and check for a link header
marking the storage as owned by the owner of the WebID. There is no guarantee
this will succeed.

If no storage space is found through either the profile triples or finding the
closest storage, the only option is to ask the WebID owner.

Applications SHOULD NOT try guessing a storage location based on the WebID URI.

## 7. Inbox - ldp:inbox

A Solid inbox is a Solid-specific messaging system that is similar to but not
the same as an email inbox.

An app wanting to read or write mail in a WebID owner's Solid inbox SHOULD look
in the profile for a single triple with the WebID as subject, `ldp:inbox` as
predicate, and a container intended to hold messages as object. For example:

```
<?WebID> ldp:inbox <?InboxContainer>.
```

If no inbox is found and the app is operating on behalf of a WebID owner, and
the app has both write and control access, the app MAY offer to create an inbox.
If a WebID owner confirms inbox creation, the app SHOULD create a container and
access control for it that gives read and write permissions to the WebID owner
and append but not read or write permissions to everyone else. The app should
also place a triple in the `WebID Profile Document` or in an `Extended Profile
Document` with the WebID as subject, `ldp:inbox` as predicate and the newly
created container as object.

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

