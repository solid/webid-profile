## Introduction

This section is non-normative.

A central goal of Solid and of the Web is to "empower an equitable, informed and interconnected society" [ETHICAL-WEB].  In order to work together online towards these goals people and communities need a voice - a way to describe who they are; a discovery mechanism - a way to find the resources they need; and an identifying mechanism - a way to be found and trusted for purposes of collaboration. A person or organization's `Solid Profile` is a set of statements which fulfill or point to resources which fulfill one or more of those functions - identification and description of the profile owner, discovery of their resources and the resources of their social network. A Solid Profile is thus an entryway to a person or organization's presence in the "vast inter-twingled tapestry" [NELSON] that is the collaborative and empowering web.

### Identification

A profile is the center of a person or organization's social web, and it is also the center of their web of meaning.  In the semantic web, every thing can have a URI that provides a definition of that thing.  In Solid, the `WebID` is a URI that provides the definition of an agent. A social agent - a person or organization - can [obtain one or more WebIDs]() and thereafter they are considered the owner of the WebID and the WebID is understood to refer to them. 

A WebID is a URI that, when dereferenced, leads to a a set of statements that have the WebID as subject. Taken together, these statements with the WebID as subject form a Solid Profile.  The WebID denotes the agent in the sense that statements in the profile and elsewhere asserting that WebID has a given property assert that the person or organization the WebID denotes has that property. See [WebID]().

In order for a WebID to be used to verifiably identify its owner, a Solid Profile must contain one or more statements pointing to a service capable of authenticating the WebID owner and of verifying their login status using an authentication protocol. For example, the  `solid:oidcIssuer` predicate points to services using the [SOLID-OIDC]() protocol.  The same pointer used to login is also used by Solid Storage Servers to identify and authorize the WebID owner to access resources on their servers, so is central to both authentication and authorization of the WebID owner

The profile can also hold other kinds of statements used to identify the owner - for example, public-keys used for payments.

See documentation on the relevant authentication/identification protocol for details.

### Description

For profile owners who want to have an outward-facing description of themselves, the Solid Profile is a place a person can share with audiences of their choosing details such as name, pronouns, skills and an organization can share details such as purpose, code of conduct, and logo. 

Descriptions of the profile owner can include details meant for humans and/or for machines.  For example descriptions of the language(s) a person speaks can be used by a human to know how to communicate with the owner and by a machine to know what language-specific UI to present to them. Profiles can also store other configurations and settings meant for applications as well as details used to auto-fill forms.

The current document does not specify individual predicates to be used in descriptions although the [vcard]() and [foaf]() vocabularies are recommended as a starting point.

### Discovery : the profile owner's resources

The [`pim:storage`]() predicate is used for discovering the general location of the owner's resources - their storage(s). A Solid storage can be a place a person or organization stores resources for their own use or use by others; a place others store resources for them; a public data repository; or a combination of any of those. A social agent may own multiple storages hosted in the same location as their profile or at other locations. See [use cases]() for scenarios such as blocking discovery of a storage, owning multiple storages, and sharing control of a storage.

A more fine-grained approach to finding resources is provided by the `solid:publicTypeIndex` and `solid:privateTypeIndex` predicates used to point from an owner's profile to indexes - sets of statements describing the location of specific types of resources belonging to the profile owner.  See [TYPE-INDEXES-SPEC]().

### Discovery - the profile owner's social network

A person can use a Solid Profile to store links to the profiles of people they know and organizations they are affiliated with. An Organization can store links for its members, employees, and partners.  A number of ontologies provide predicates to indicate membership and a variety of kinds of affiliation between one person and another; between a person and an organization; between one organization and another. These kinds of affiliation links can be followed by apps as needed, for example to provide a UI to chat with a person's friends or a table of an organizations' volunteers, donors, and partners.

This document will specify only two predicates related to social networking : [ldp:inbox]() supports communication and [`solid:community`]() supports profile-to-profile links indicating to applications that some resources in the organization's profile are available for use by the profile owner.

### Discovery - selective visibility based on social agent audience

People and organizations share different information with different audiences - your work colleagues don't know the same things about you that your family does; a company's HR department knows things the public doesn't; some things are for your eyes only.  This is the "gradient of intimacy" that Sir Tim speaks of in the introductory quote - spaces formed by differing levels of permissions for differing audiences.
 
Solid supports the gradient of intimacy through access control systems such as [WAC]() and [ACP]() which allow users to assign permissions to resources based on intended audience. This document recommends a strategy for permissioning which views three broad categories of social agent audience - 1) no audience - private, for the profile owner's use only 2) all audiences - public, for the use of anyone on the network where the profile is located 3) some audiences - restricted to specifically named audiences.

These three classes of audience are handled in the strategy detailed below by three kinds of documents - the [`Solid WebID Profile Document`]() for public information, the [`Preferences File`]() for private information, and files pointed to from one of those two documents using the [`rdfs:seeAlso`] predicate for restricted information and for information more voluminous than one wants to store in the Solid WebID Profile Document itself. This separation into documents means that public information is almost always the first thing an app sees, that private resources can be easily ignored without having to test the access of each resource, that materials that are very large or only of interest to selected audiences are segregated so they can be accessed only when needed.

### Discovery - selective visibility based on software agent audience

Solid provides permissioning based on both social and software agents.  The documents linked to the profile with the rdfs:seeAlso predicate can be permissioned for use by a single software application or by a group of applications. A profile can also contain statements related to the permissioning of applications using the [acl:trustedApp]() predicate.

### Profiles and identities

A person's Solid Profile is an online presentation of self, of identity. People present different parts of their identity to different audiences and these differences in presentation can often be handled in a single profile by the selective visibility strategies mentioned above. 

However there are many reasons why a social agent might want or need multiple online identities and therefore multiple WebIDs and multiple profiles. Anonymous and pseudonymous profiles need to be completely separate online identities from a profile which publicly lists a person's name. A company might provide an employee with a work profile intended for things to be shared with colleagues. 

In cases where the owner of two profiles wants to link the identities together, the [`owl:sameAs`]() predicate can be used.  These links can occur in either or both of the profiles i.e. the relationship between the identities can be either uni-directional or bi-directional.  For example a person might want to link from their personal profile to their work profile to have access to a CV in the work profile but might not want a link from the work profile to their personal profile in order to keep their home identity and work identity separate.  See [use cases]().

### Other kinds of profiles

This specification and the phrase `Solid WebID Profile Document` refer to documents located on Solid storages. WebID Profile Documents not located on Solid storages may be considered functionally equivalent to a Solid WebID Profile Document for purposes of reading if they conform to this specification in all respects other than sections on writing to the Solid WebID Profile Document. See [use cases]().
