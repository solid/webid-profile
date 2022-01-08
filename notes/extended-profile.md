# Loading the full profile

In order to retrieve a complete profile, An app MUST load the profile document and MUST look for extended profile documents by examining the objects of all triples found in that document that have the WebID as subject and a predicate consisting of one of `foaf:isPrimaryTopicOf`, `pim:preferencesFile`, `owl:sameAs`, or `rdfs:seeAlso`.  For each object found that dereferences to a document accessible to the app, the app MUST load that document.

The object of the `pim:preferencesFile` predicate can be assumed to be a private document.  The other documents may be either public or private.  

  * <#WebID> **foaf:isPrimaryTopicOf** <?AnyDocument> .
  * <#WebID> **pim:preferencesFile**   <?PrivateDocument> .
  * <#WebID> **owl:sameAs**            <?AnyDocument> .
  * <#WebID> **rdfs:seeAlso**          <?AnyDocument> .

Developers should be aware that each of these predicates can take multiple objects.

Once all of the accessible extended profile documents have been loaded, the profile will consist of all statements with the WebID as subject, regardless of which document the statements occurred in.  An app with only public permissions will see only statements from publicly accessible extended profile documents while other apps will see statements from public and also private extended profile documents they have access to.


