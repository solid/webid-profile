## An informal summary of our recommendations

### A Solid WebID Profile Document
  * MUST be publicly readable
  * MUST contain one or more `solid:oidcIssuer` triples
  * SHOULD contain exactly one `pim:preferencesFile` triple
  * SHOULD contain exactly one `solid:publicTypeIndex` triple
  * MAY contain one or more `rdfs:seeAlso` triples
  * MAY contain one or more `acl:trustedApp` triples

### A Solid Preferences Document
  * MUST NOT be available to anyone other than the WebID owner or their rep
  * SHOULD contain exactly one `solid:privateTypeIndex` triple
  * MAY contain one or more `rdfs:seeAlso` triples

### A Solid Public Type Index
  * MUST be publicly readable
  * SHOULD contain one or more `solid:TypeRegistration` triples

### A Solid Private Type Index
  * MUST NOT be available to anyone other than the WebID owner or their rep
  * SHOULD contain one or more `solid:TypeRegistration` triples

### The Full Solid Profile
  * SHOULD contain one or more `ldp:inbox` triples
  * SHOULD contain one or more `solid:storage` triples

### Apps wanting to assemble a full profile
  * MUST load the WebID Profile Document
  * MUST load the Preferences Document if they have access
  * MUST load the TypeIndes Documents if they have access
  * MUST load docs from all `rdfs:seeAlso` triples in the WebID or Prefs docs
  * MAY decline to follow seeAlso links inside seeAlso documents
  * MAY decline to read statements in seeAlso documents where WebID is not the subject;

### Apps with appropriate access that find missing profile information
  * MAY create any missing documents
  * MAY add missing triples with this exception :
    * `acl:trustedApp` MUST NOT be modified without a user-controlled form

### Apps other than Pod Management Apps
  * MUST NOT request pod-wide Write or Control access
