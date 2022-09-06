## Solid WebID Profile Data Model

### An interoperably usable Solid Profile Document will

* be locatable by dereferencing the WebID URI as described in the WebID spec
* provide a link to a verifying authority (for one example, solid:oidcIssuer, see Solid-OIDC spec)
* provide zero or more rdfs:seeAlso links to extended profile documents (also defined in the WebID spec)
* be modifiable by the Agent the WebID denotes (denotes as defined in the WebID spec, modifiable to be defined, but without specifying any particular method)

### An interpoperably usable Solid Profile Document will probably also

* contain one ldp:inbox link (or it could be in an extended document)
* contain one or more space:storage links (or they could be in extended documents)

### When reading an interoperably usable Solid Profile, apps should load

1. the document the WebID URI dereferences to
2. the object of a space:preferencesFile triple in that document if the triple exists and is accessible
3. the objects of all rdfs:seeAlso triples in the documents loaded so far if the objects are accessible

The Solid Profile will now consist of all triples loaded in this process with the WebID as subject.  Apps are recommended to check after each of these steps whether they've already found the information they need and if so, omit the following steps.

### When writing an ineteroperably usable Extended Profile Document, apps

* can not depend on other apps following rdfs:seeAlso links inside Extended Profile Documents
* can not depend on apps reading triples in Extended Profile Documents that do not have WebID as subject
