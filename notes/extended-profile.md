# Loading the full profile

It is possible to have all profile information in a single profile document but a more likely situation is that some information is in the profile document and some is in extended profile documents.  One key reason for this is that the profile document is a public document and therefore resources and prefernces that are private need to be described in private documents.  The public profile document becomes the starting point for a "follow-your-nose" process to look at the private and other extended profile documents. 

In order to retrieve a complete profile, An app MUST load the profile document and MUST look for extended profile documents by examining the objects of all triples found in that document that have the WebID as subject and a predicate consisting of one of `foaf:isPrimaryTopicOf`, `pim:preferencesFile`, `owl:sameAs`, or `rdfs:seeAlso`.  For each object found that dereferences to a document accessible to the app, the app MUST load that document.

The object of the `pim:preferencesFile` predicate can be assumed to be a private document.  The other documents may be either public or private.  

  * <#WebID> **foaf:isPrimaryTopicOf** <?AnyDocument> .
  * <#WebID> **pim:preferencesFile**   <?PrivateDocument> .
  * <#WebID> **owl:sameAs**            <?AnyDocument#SomeSubject> .
  * <#WebID> **rdfs:seeAlso**          <?AnyDocument#SomeSubject> .

Developers should be aware that each of these predicates can take multiple objects.

Once all of the accessible extended profile documents have been loaded, the profile will consist of all statements with the WebID as subject, regardless of which document the statements occurred in.  An app with only public permissions will see only statements from publicly accessible extended profile documents while other apps will see statements from public and also private extended profile documents they have access to.


