# Solid Application Data Handling (draft)

## Data discovery

An app can discover the general location of a WebID owner's data by finding the location of:

- [Storages](./storage.md) associated with the WebID;
- or/and for more specific information, like the type of data and location of data, by loading the [Type Index](./typeIndex.md) documents.

### Discoverying Storages

### Discoverying Type Indexes

1. An app SHOULD look in the Profile Document (WebID or Extended profile documents) or in the Preference File for a triple that contains the `solid:publicTypeIndex` or the `solid:privateTypeIndex` predicate.
2. If a triple is not found, the app MAY offer to create the triple and the Type Index document. See example at [Type Index section for examples](./typeIndex.md#examples).
3. If the triple is found, the app SHOULD load the object of that triple, which is the Type Index document.

## Data reading

An app interested in reading data stored by itself or other apps SHOULD read the [Type Indexes](./typeIndex.md) to find the data location(s) and types of data.

Assuming a Type Index document exsits:

1. The app MUST load the document.
2. The app SHOULD read the Type Index registration entries which are of type `solid:TypeRegistration`. Further, each entry MAY maps to a Solid resource or a Solid container described by triples that contain `solid:instance` and/or `solid:instanceContainer`.

## Data writing

An app that writes data SHOULD use the Type Indexes to register information about where it writes the specific data and what type of data it writes.

Assuming a Type Index document exsits:

1. The app MUST load the document.
2. The app MUST write a triple of type `solid:TypeRegistration`.
3. The same triple SHOULD map to a Solid resource and/or a Solid container by writing triples that contain the predicates  `solid:instance` and respectively `solid:instanceContainer`. See example at [Type Index section for examples](./typeIndex.md).

If there is no Type Index document, refer to the [Handling missing type indexes section](#Handling-missing-type-indexes).

## Changing privacy of data

Changing the privacy of data involves working with the private and public Type Indexes.

The app MUST change the status of a registration entry in the Type Index. This involves *removing* that registration entry (typically via a SPARQL-based HTTP PATCH) from the one and *adding* it (also via a PATCH) to the other type index document.

## Handling missing Type Indexes

If there is no Type Index, assuming the app writes a specific type of data, it SHOULD create a Type Index. The following steps are recommended:

1. Ask the WebID owner if the data to be written is public or private and create the according link as mentioned in the [Where to find the Type Indexes](./typeIndex.md#where-to-find-the-type-indexes).
2a. For a public Type Index document, create the document with the specification mentioned under [Public Type Index](./typeIndex.md#public-type-index-usually-called-publictypeindexttl).
2b. For a private Type Index document, create the document with the specification mentioned under [Private Type Index](./typeIndex.md#private-type-index-usually-called-privatetypeindexttl).
