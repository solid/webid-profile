# Solid Application Data Handling (draft)

## Data discovery

An app can discover the general location of a WebID owner's data by finding the location of:

- [Storages](./storage.md) associated with the WebID;
- or/and for more specific information, like the type of data and location of data, by loading the [Type Index](./typeIndex.md) documents.

### Discoverying Storages

### Discoverying Type Indexes

1. An app SHOULD look in the Profile Document (WebID or Extended profile documents) or in the Preference File for a triple that contains the `solid:publicTypeIndex` or the `solid:privateTypeIndex` predicate.
2. If a triple is not found, the app MAY offer to create the triple and the Type Index Registry. See example at [Type Index Resource links example](#type-index-resource-links-example).
3. If the triple is found, the app SHOULD load the object of that triple, which MUST look like (`setting/publicTypeIndex.ttl` or `settings/privateTypeIndex`)

## Data reading

An app interested in reading data stored by itself or other apps SHOULD read the [Type Indexes](./typeIndex.md) to find the data location(s) and types of data.

Assuming a Type Index Registry resource exsits:

1. The app MUST load the resource.
2. The app SHOULD read the Type Index registration entries which are of type `solid:TypeRegistration`. Further, each entry MAY maps to a Solid resource or a Solid container described by triples that contain `solid:instance` and/or `solid:instanceContainer`.

## Data writing

An app that writes data SHOULD use the [Type Indexes](./typeIndex.md) to register information about where it writes the specific data and what type of data it writes.

Assuming a Type Index Registry resource exsits:

1. The app MUST load the resource.
2. The app MUST write a triple of type `solid:TypeRegistration`.
3. The same triple SHOULD map to a Solid resource and/or a Solid container by writing triples that contain the predicates  `solid:instance` and respectively `solid:instanceContainer`. See example at [Type Index Registry Resource content](#type-index-registry-resource-content)

## Changing privacy of data

Changing the privacy of data involves working with the private and public [Type Indexes](./typeIndex.md).

The app MUST change the status of a registration entry in the Type Index. This involves *removing* that registration entry (typically via a SPARQL-based HTTP PATCH) from the one and *adding* it (also via a PATCH) to the other type index document.
