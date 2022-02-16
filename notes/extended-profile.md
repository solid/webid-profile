# Loading the full profile

It is possible to have all profile information in a single WebID document but a more likely situation is that some information is in the WebID document and some is in extended profile documents. There might be multiple documents simply because that's the way a given Solid server operates. Another reason is that the WebID document is a public document and therefore resources and preferences that are private need to be described in private documents. The public WebID document becomes the starting point for a "follow-your-nose" process to look at the private and other extended profile documents.

In order to retrieve a complete profile, an app MUST load the WebID document and MUST look for triples with the WebID as subject and `foaf:isPrimaryTopicOf` as predicate and, if found, load the object(s) of the triple(s).

Once those steps are taken and regardless of whether any `foaf:primaryTopicOf` triples were found, the app should examine the objects of all triples found in what has been loaded so far that have the WebID as subject and a predicate consisting of one of `pim:preferencesFile`, `owl:sameAs`, or `rdfs:seeAlso`. For each object found that dereferences to a document accessible to the app, the app MUST load that document if it wishes to obtain a complete profile.

The object of the `pim:preferencesFile` predicate can be assumed to be a private document. The other documents may be either public or private. These are the key kinds of triples that point to extended profile documents:

* <#WebID> **foaf:isPrimaryTopicOf** <?AnyDocument> .
* <#WebID> **pim:preferencesFile**   <?PrivateDocument> .
* <#WebID> **owl:sameAs**            <?AnyDocument#SomeSubject> .
* <#WebID> **rdfs:seeAlso**          <?AnyDocument#SomeSubject> .

Developers should be aware that each of these predicates can take multiple objects.

Once all of the accessible extended profile documents have been loaded, the profile will consist of all statements with the WebID as subject, regardless of which document the statements occurred in. An app with only public permissions will see only statements from publicly accessible extended profile documents while other apps will see statements from public and also private extended profile documents they have access to.
