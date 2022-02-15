# Solid Application Data Discovery  (draft)

Next, the information that an application can use to learn how to interact with the data associated with the Social Agent's WebID will be presented. To interact with data, the app needs to know what type of data the Social Agent has and where to find it. For this purpose, the Type Registry Index SHOULD exit.

The Type Registry Index MAY consists of two separate registry index resources: one Listed (public readable by default) called `publicTypeIndex.ttl` and one Unlinsted (private, readable by the owner only, by default) called `privateTypeIndex.ttl`. They are linked from the `profile` (WebID document or Extended profile document) through the following predicates: for the Listed Type Index Resource  `solid:publicTypeIndex` and for the Unlisted Type Index Resource `solid:privateTypeIndex`.

An example for a Listed, public Type Index Resource linked from the profile is:
```
   <#WebID> <http://www.w3.org/ns/solid/terms#publicTypeIndex> </settings/publicTypeIndex> .
```
An example for a Unlisted, private Type Index Resource linked from the profile is:
```
   <#WebID> <http://www.w3.org/ns/solid/terms#privateTypeIndex> </settings/privateTypeIndex> .
```

A diagram about the Type Registry Index is presented next:
![Type Registry Index diagram](diagrams/type-indexes.svg)

### Listed Type Index (publicTypeIndex.ttl)

The Listed Type Index is intended for registrations that are *discoverable by
outside users and applications*. For example, think of a listed phone number in
a public phonebook, which contains publicly-discoverable mappings of people's
names to phone numbers and addresses.

The Listed (public) Type Index MUST have the following properties:

* Is linked to from the WebID Profile using the `solid:publicTypeIndex` predicate
* Is created in `/settings/publicTypeIndex.ttl` by default
* Has a public-readable ACL by default
* Is of type `solid:ListedDocument` and `solid:TypeIndex`

Example Listed Type Index Resource containing one registration entry:

```ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
# ...
<>
    a solid:TypeIndex ;
    a solid:ListedDocument.

```

### Unlisted Type Index (privateTypeIndex.ttl)

The Unlisted Type Index Resource is intended for registrations that are private
to the user and their apps, for types that are *not* publicly discoverable.

The Unlisted (private) Type Index MUST have the following properties:

* Is typically linked to from the Preferences file (or some other private
  section of an Extended Profile) using the `solid:privateTypeIndex` predicate
* Is created in `/settings/privateTypeIndex.ttl` by default
* Has a private ACL by default (readable only by the owner)
* Is of type `solid:UnlistedDocument` and `solid:TypeIndex`

Example Unlisted Type Index Resource:

```ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
@prefix sioc: <http://rdfs.org/sioc/ns#>.
# ...
<>
    a solid:TypeIndex ;
    a solid:UnlistedDocument.

```

## Type Index Registry Resource content

Both the Listed and Unlisted Type Index Resources SHOULD contain registry entries that map a specific resource type to a location in the user's data space.
An example in the Listed Type Index Resource (publicTypeIndex.ttl) that shows where data related to an Address Book exists is:

```ttl
@prefix solid: <http://www.w3.org/ns/solid/terms#>.
# ...
<>
    a solid:TypeIndex ;
    a solid:ListedDocument.

# Maps the type vcard:AddressBook to an index document
<#ab09fd> a solid:TypeRegistration;
    solid:forClass vcard:AddressBook;
    solid:instance </contacts/myPublicAddressBook.ttl>.
```

### Type Index Registrations

The type index resources MUST contain any number of statements of type `solid:TypeRegistration` which map RDF classes/types to their locations in a user's dataspace/root storage.

The registration entries map a type to a location and MUST use one of two predicates:

##### `solid:instance`
maps a type to an individual Solid *resource*, typically an index or a directory listing resource such as an Address Book.

##### `solid:instanceContainer`
maps a type to a Solid *container* which the client would have to list to get the instances of that type.

## Working with the Type Index Registry

### Changing from Listed to Unlisted or vice-versa

Changing the status of a registration from a Listed index to an Unlisted or vice-versa involves *removing* that registration (typically via a SPARQL-based HTTP PATCH) from the one and *adding* it (also via a PATCH) to the other.


## Reference
* [link 1](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)