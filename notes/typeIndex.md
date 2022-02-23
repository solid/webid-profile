# Type Indexes (draft)

## What is a Type Index?

To interact with data, the app needs to know what type of data the WebID owner has and where to find it. For this purpose, the Type Indexes SHOULD exit.

A well-behaved profile will include exactly one public Type Index (readable by all) called `publicTypeIndex.ttl` and one private Type Index (readable only be apps acting on behalf of the WebID owner) called `privateTypeIndex.ttl`.

## Where to find the Type Indexes

To find the Type Indexes, an app SHOULD load Profile Document (WebID document, WebID Helper Document, Extended profile document) or the Preference File. In these loaded documents one can find triples where the WebID as subject and the following predicates: for the Public Type Index document the `solid:publicTypeIndex` predicate, and for the Private Type Index document the `solid:privateTypeIndex` predicate.

### Examples

An example for a Public Type Index Resource linked from the profile is:
```
   <#WebID> <http://www.w3.org/ns/solid/terms#publicTypeIndex> </settings/publicTypeIndex> .
```
An example for a Private Type Index Resource linked from the profile is:
```
   <#WebID> <http://www.w3.org/ns/solid/terms#privateTypeIndex> </settings/privateTypeIndex> .
```

Here is a diagram showing an example set of Type Indexes:
![Type Registry Index diagram](../diagrams/type-indexes.svg)

## Public Type Index (publicTypeIndex.ttl)

The Public Type Index document MUST be of type `solid:ListedDocument` and `solid:TypeIndex`.

The Public Type Index document contains registration entries that are *discoverable by
outside users and applications*. For example, think of a registration like a phone number in
a public phonebook, which contains publicly-discoverable mappings of people's
names to phone numbers and addresses.

Example of a Public Type Index document. This contains a public resource of type `vcard:AddressBook` located at the resource address `</public/contacts/myPublicAddressBook.ttl>` and a resources of type `bk:Bookmark` located in the container address `</public/myBookmarks/>`:

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

## Private Type Index (privateTypeIndex.ttl)

The Private Type Index document contains registration entries that are private
to the user and their apps, for types that are *not* publicly discoverable.

The Private Type Index document MUST be of type `solid:UnlistedDocument` and `solid:TypeIndex`.

Example of a Private Type Index document. This contains a private resource of type `vcard:AddressBook` located at the resource address `</private/contacts/myPublicAddressBook.ttl>` and a resources of type `bk:Bookmark` located in the container address `</private/myBookmarks/>`:

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

## Type Index registration entries

The type index documents MAY contain any number of statements of type `solid:TypeRegistration` which map RDF classes/types to their locations in a WebID owner's dataspace/root storage.

The registration entries map a type to a location and MUST use one of two predicates:

##### `solid:instance`
maps a type to an individual Solid *resource*, typically an index or a directory listing resource such as an Address Book.

##### `solid:instanceContainer`
maps a type to a Solid *container* which the client would have to list to get the instances of that type.
## Reference
* [link 1](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)