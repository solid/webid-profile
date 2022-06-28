# Discovering Resources in a Solid Profile

## Version 1.0

## Pre-Draft 2022-04-01

> "A city has public places where I can do all kinds of things, and also a
> private house with a private room which may be just mine. In that house there
> are spaces where I do things with family, friends, colleagues. The web must,
> like a well-designed building, provide a gradient of intimacy between the
> private and the public, so I can easily recognize the difference, easily know
> which I am in, and easily welcome people to come into the more intimate parts,
> as I want to." -- timbl

If the web is like a well-designed building, the Solid Profile is its lobby - the
place the public can discover which parts of the interior the building's owner
has given consent for them to see, where the owner can hide or put on display
parts of their identity as they see fit.

### Editors

* Virginia Balseiro
* Timea Turdean
* Jeff Zucker

### Contributors

* Tim Berners-Lee
* Sarven Capadisli

### Created

* 2022-04-01

### Modified

* 2022-05-31

### Language

* English

### License

* [MIT License](http://purl.org/NET/rdflicense/MIT1.0)

## Status of this document

This is an Unofficial Draft of editors that describes the work on WebID
profiles in Solid. It does not represent consensus of the W3C Solid Community
Group. An initial Editor's Draft of this document is intended to be proposed
as a Work Item to the W3C Solid Community Group once we have finished our
discussions and gathered public review.

## Audience and Scope

This document is aimed at application developers. It describes the process Solid
applications can use to discover information about the structure of resources in
a social agent's profile.

This document does not cover the WebID authentication process (see [Solid
Protocol's
Authentication](https://solidproject.org/TR/protocol#authentication))
or data specific to a given type of social agent (see
forthcoming [Creating Personal Profiles]TBD and [Creating Organizational
Profiles]TBD)). Alternate discovery processes (for example [proposed
Interoperability-Spec]TBD) are also out of scope for this document although they
will be mentioned where relevant.

## 1. Introduction

This section is non-normative.

A social agent - a person or organization - can establish an identity in the
Solid ecosystem by obtaining a unique identifier called a `WebID` and by describing
themselves in a set of documents associated with the WebID. These documents,
referred to here collectively as a `Solid Profile`, both describe the social
agent and provide links to their resources.  

A Solid profile, like most Solid resources, can include a combination of
publicly readable data, data restricted to named audiences, and data meant only
for the social agent themselves. For example, Keisha's Solid profile might list
her name publicly, her phone number only for friends, and the configuration
settings for her "notes to self" only for apps operating on her own behalf.
Solid supports these options by splitting the profile into documents with
separate access restrictions. So the first task of an application may be to load all
of the profile documents it has access to or load them on a need to basis.

Profiles can contain, and apps are free to follow, any kind of links to related
documents. In order to promote interoperability and limit the burden on apps,
this specification recommends a limited number of related documents which a
profile ought to contain so that apps can discover them.

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
|pim:storage | location(s) of the WebID owner's storage space(s) |
|solid:instance, solid:instanceContainer | locations of specific types of resources|
|ldp:inbox| location of the WebID owner's Solid inbox |
<!-- |acl:trustedApp | origin & permissions of applications the WebID owner has given access to  | -->

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
accessible to an app that is logged in and authenticated as the WebID owner
or as an agent delegated by the WebID owner to act in their behalf.  The preference
Document can include settings and preferences, and pointers to the owner's
personal, private data, be it contacts, pictures or health data.

An app operating on behalf of the owner can gather configuration settings from
the owner, store them in the `Preferences Document`, and then read them there on
subsequent visits. Such an app might also record private information (for
example, a driver's license number) and later, at the direction of the owner,
retrieve the information to fill out a form.

A Solid `WebID Profile Document` MUST have exactly one triple with the WebID
as subject, `pim:preferencesFile` as predicate, and the location of the `Preferences Document` as object.

For example:

```
<?WebID> <http://www.w3.org/ns/pim/space#preferencesFile> <?PreferencesDocument> .
```

When an app operating on behalf of the WebID owner cannot discover a
`pim:preferencesFile` triple, and has write and control access, and wishes to
write preference data, it could create a document accessible only to the WebID
owner.  An app that creates a Preference Document can insert a triple in the
WebID Profile Document with the WebID as subject, `pim:preferencesFile` as predicate,
and the URL of the created document as object.

## 3. Extended Profile Documents - rdfs:seeAlso

Solid Profile owners may use the `rdfs:seeAlso` predicate to link to extended
profile documents which contain information that they do not want in the [WebID
Profile Document](TBD) or in the `Preferences Document`.  This can be done to
help organize information - for example to keep all friends (objects of
`foaf:knows` predicates) in a separate `rdfs:seeAlso` document. It can also be
done to limit access to the data - for example to store a phone number where
only trusted friends can view it.  

### 3.a Reading Extended Profile Documents

An app wanting to load a complete `Solid Profile` MUST examine statements in
the [WebID Profile Document](TBD) and the [Preferences Document](TBD) that have
the WebID as subject, `rdfs:seeAlso` as predicate and the URL of an `Extended
Profile Document` as object. When the app has loaded those two documents, it
SHOULD load the documents specified in the URLs of all `rdfs:seeAlso` triples
found and SHOULD treat all statements in the linked documents that have the
WebID as subject as part of the `Solid Profile`.  An app may but can not be expected
to load the objects of `rdfs:seeAlso` triples found in documents that are themselves
the object of an `rdfs:seeAlso` triple.  In other words, look in the WebID Profile Document and in the Preferences Document for `rdfs:seeAlso` triples but don't necessarily follow any additional `rdfs:seeAlso` triples found in those documents.  An app can examine any statement found in the linked documents for other purpose, which are not described by this specification.

### 3.b Writing Extended Profile Documents

When an app wants to write data in an [Extended Profile Document](TBD), it
SHOULD give the document appropriate permissions depending on the needs of the
WebID owner. If the document is meant only for the WebID owner, the app SHOULD create an `rdfs:seeAlso` triple pointing to it in the [Preferences File](TBD). If the document is meant for the public or for a restricted audience, the triple SHOULD be created in the [WebID Profile Document](TBD).

## 4. Type Indexes

### What is a Type Index?

A Type Index is a document containing statements that link specific types of
data to specific locations. An app might ask a user where they want to store
items of type Bookmark and then write that location in the Type Index.
Thereafter another app can find the bookmarks by looking for the location in the
Type Index.

Apps that want to remember user's choices will read from and write to the Type Index.
Since users will undoubtedly have data they want
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

An example for a Private Type Index Resource located in `settings` and is called
`privateTypeIndex.ttl` linked from the profile is:

```
   <#WebID> <http://www.w3.org/ns/solid/terms#privateTypeIndex> </settings/privateTypeIndex.ttl> .
```

Here is a diagram showing an example set of Type Indexes: ![Type Registry Index
diagram](../diagrams/type-indexes.svg)

### Public Type Index

The Public Type Index document contains registration entries that are *discoverable* by outside users and applications but which are not necessarily *accessible* to all applications or users.  For example, suppose I have one address book that is only for myself, one that is for a specific group of colleagues, and one that is for the general public.  The one that is only for me should be listed in the private type index.  The other two should be listed in the public type index. The fully public listing will point to a publicly accessible resource while the one just for my colleagues will point to a resource restricted by access control methods.  Both the public and restricted resources are discoverable in the public type index because an app that is not operating as the WebID owner can not see the private type index.

Example of a Public Type Index that is a Linked Document. This contains a public resource of type
`vcard:AddressBook` located at the resource address
`</public/contacts/myPublicAddressBook.ttl>` and a resources of type
`bk:Bookmark` located in the container address `</public/myBookmarks/>`:

```publicTypeIndex.ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix vcard: <http://www.w3.org/2006/vcard/ns#>.
@prefix bk: <http://www.w3.org/2002/01/bookmark#>.

<>
    a solid:TypeIndex ;
    a solid:ListedDocument.

<#ab09fd> a solid:TypeRegistration;
    solid:forClass vcard:AddressBook;
    solid:instance </public/contacts/myPublicAddressBook.ttl>.

<#bq1r5e> a solid:TypeRegistration;
    solid:forClass bk:Bookmark;
    solid:instanceContainer </public/myBookmarks/>.
```

### Private Type Index

The Private Type Index document contains registration entries that are private
to the user and their apps. It is intended for types of resources meant only to be accessed by the WebID owner.  

Example of a Private Type Index that is an Unlisted Document, This contains a private resource of
type `vcard:AddressBook` located at the resource address
`</private/contacts/myPublicAddressBook.ttl>` and a resources of type
`bk:Bookmark` located in the container address `</private/myBookmarks/>`:

```privateTypeIndex.ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix vcard: <http://www.w3.org/2006/vcard/ns#>.
@prefix bk: <http://www.w3.org/2002/01/bookmark#>.

<>
    a solid:TypeIndex ;
    a solid:UnlistedDocument.

<#ab09fd> a solid:TypeRegistration;
    solid:forClass vcard:AddressBook;
    solid:instance </private/contacts/myPublicAddressBook.ttl>.

<#bq1r5e> a solid:TypeRegistration;
    solid:forClass bk:Bookmark;
    solid:instanceContainer </private/myBookmarks/>.

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

If one or both of the type indexes are missing, an app needing to write to them that doesn't have Write and Control access to the pod MAY warn the user that their indexes are missing and that they should use a Pod Management App to fix their profile.

If one or both of the type index documents are missing and the app does have Write and Control access to the pod, the app MAY create documents.  The public type index document SHOULD be publicly readable,
writable by the user (or a group delegated by the user), and a pointer to it MUST be placed in the WebID Profile Document.  The private type index SHOULD be readable and writeable only by the WebID owner and a pointer to it MUST be placed in the Preferences Document.

(Note if the type index file is created but the pointer to it not stored, then this risks, on
a future occasion, another separate empty resource being created, leading to confusion and possible loss of data.)

## Communities

Note: This section is "at risk" in that is more recent and less tested than the rest of this spec.

A user may use not only their own type indexes to find and remember things, but also may use the type indexes of users which are organizations, such as projects, to which they are affiliated. To enable this, a triple
```
  {user} solid:community {org} .
```
is placed in either the user's private preferences or public profile.  The `org` node's URI is the WebID URI of the project, etc.

### Reference

* [link 1](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)

## 5. Identity Provider - solid:oidcIssuer

The `solid:oidcIssuer` predicate is used to indicate the address of a Solid Identity Provider capable of authenticating the WebID owner.  Apps wanting to facilitate login will need to look for this predicate. Apps not needing to facilitate login can ignore the predicate.

As stated in the [Solid OIDC specification](https://solidproject.org/TR/oidc), "The WebID Profile Document MUST include one or more statements matching the OIDC issuer pattern." This means that a `WebID Profile Document` MUST contain at least one triple with the WebID as subject, `solid:oidcIssuer` as predicate, and the URL of the domain of an OIDC Issuer (Identity Provider) as the object. For example :

```
<?WebID> <http://www.w3.org/ns/solid/terms#oidcIssuer> <?Issuer> .
```

If a single statement with the `solid:oidcIssuer` predicate is found, the app
wanting to login SHOULD redirect to the URL specified by the value of `?Issuer`.

If multiple Issuers are found, the app SHOULD offer the user a choice and
redirect to the chosen `?Issuer`.


## 6. Storage - pim:storage

The `pim:storage` predicate is used to indicate where a WebID owner stores their data. An app wanting to access the WebID owner's Pod should find its location using the `pim:storage` predicate.  Apps looking to access particular types of data should look for specific locations in the [typeIndex typeIndex documents](TBD) rather than looking for the `pim:storage` predicate, which indicates instead the Pod location, not the location of particular resources in the Pod.

A `Solid Profile` SHOULD contain at least one triple with the WebID as subject, `pim:storage` as predicate, and the root container of the storage as the object.  For example:
```
<?WebID> <http://www.w3.org/ns/pim/space#storage> <?StorageSpace>.
```
If no storage is found within the Solid Profile, an app MAY determine the location of a WebID in the context of a WebID Profile Document hosted on a Solid server as per [Solid Protocol 0.9](https://solidproject.org/TR/protocol), but this method is not guaranteed to be usable by the app in cases where the WebID Profile Document is hosted elsewhere.

If no storage space is found through either the profile triples or finding the
closest storage, an app MAY prompt the user to find a location to access data.

A Pod Management App MAY offer to create a triple by prompting the user for the Pod location and desired discoverability of the Pod.  For example, Jose might want their Pod only to store private data and not even let anyone discover that they own the Pod.  In that case the app should write the `pim:storage` triple in the `pim:preferencesFile` rather than in the `WebID Profile Document` or a publicly available extended profile document.

Applications SHOULD NOT depend on guessing a storage location based on the WebID URI because the WebID Profile Document may or may not be on the same server as the Pod.

## 7. Inbox - ldp:inbox

A Solid inbox is a Solid-specific messaging system that is similar to but not
the same as an email inbox.

An app wanting to read or write notifications WebID owner's Solid inbox SHOULD look
in the profile for a single triple with the WebID as subject, `ldp:inbox` as
predicate, and a container intended to hold messages as object. For example:

```
<?WebID> ldp:inbox <?Container>.
```

If no inbox is found a Pod Management App MAY create an inbox by creating a container. In that case, the app SHOULD also create access controls for the container that give read and write permissions to the WebID owner and append but not read or write permissions to everyone else. The app should also place a triple in the `WebID Profile Document` or in an Extended Profile Document with the WebID as subject, `ldp:inbox` as predicate and the newly created container as object.

## Other predicates

XXX: Revisit:

A user, a server or an app with appropriate permissions can add triples to profiles using any predicates.  This specification covers only those predicates related to resource infrastructure that are in common use at the time of this writing and which we recommend as a standard.  Some of the other predicates you might encounter include [`acl:trustedApp`](TBD) and [cert:key](TBD).  The Interoperability panel is also working on proposals which will add additional infrastructure related predicates to Solid profiles.

Note : The background for the decision not to include trustedApp in this spec is [here](https://github.com/solid/webid-profile/blob/main/meetings/2022-05-24.md) and the section Jeff wrote on trustedApp but removed is [here](https://github.com/solid/webid-profile/blob/main/notes/trustedApp-jeff-notes.md).
