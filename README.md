# Discovery based on Solid Social Agent WebID

[![Join the chat at https://gitter.im/solid/webid-profile](https://badges.gitter.im/solid/webid-profile.svg)](https://gitter.im/solid/webid-profile?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## Overview

When an app allows a user to log in to a Solid Identity Provider, the app receives back data that establishes the identity of the user in terms of a WebID URI.  An app can also obtain a WebID URI through input from a user or by parsing documents that contain the URI.  The general purpose of this document is to describe what an app can discover based on the WebID.  More specific purposes are described below.

A Social Agent - a person, group, or organization - may own one or more WebIDs.  Each WebID is a URI that can be dereferenced to point to exactly one document, hereafter called a `WebID document`.  The term 'WebID document', will be used here to refer only to a `Social Agent WebID Document`. WebID documents, in general, can be for software or other kinds of Agents but, in this document, it will only apply to WebID documents associated with Social Agents.

WebID documents may optionally point, through various means, to other documents whose intent is to identify the WebID owner and/or describe details about their data.  These other documents, whatever predicates are used to indicate them, and however they are named, will be referred to collectively as `extended profile documents`.  

The term `profile` will be used here to refer to the data that can be gathered in the WebID document and all extended profile documents it points to.

## Structure of document

Profiles contain two major types of information: 1) identifiying information such as a person or organization's name, purpose, contacts, etc. and 2) infrastructure information such as what types of data the user has, where it is stored, and who has access to it. 
This document will focus on the latter - the information that an app can use to learn how to interact with the data associated with the Social Agent's WebID.  

This repo will eventually contain three documents - 
1) this one on infrastructure discovery, 
2) one on personal profile information, 
3) and one on organizational profiles.  

These may be combined into a single document in the future.

## Content addressed

There is great variability in what information is found in the profile and which documents the information is stored in.  The [interoperability panel](https://solid.github.io/data-interoperability-panel/specification/) is in the process of developing alternative discovery processes. So, there can be more than one discovery process. This specification aims to **describe the discovery process as it is currently in use**.

To sum up, this document aims to cover

* Social Agents, not Software Agents
* WebID documents and extended profile documents
* Discovery of data structures, not personal/group identifying information
* Current practices, not future specifications

The following predicates will be the main foucs of the documention :

- [ ] foaf:primaryTopic
- [ ] foaf:isPrimaryTopicOf
- [ ] solid:oidcIssuer
- [ ] solid:account (have not yet found anyone using this, maybe omit?)
- [ ] solid:publicTypeIndex
- [ ] solid:privateTypeIndex
- [ ] pim:storage
- [ ] pim:preferencesFile
- [ ] acl:trustedApp (possibly omit this for this spec?)
- [ ] owl:sameAs
- [ ] rdfs:seeAlso

## Related Resources
* [Minutes of meetings](https://github.com/solid/webid-profile/tree/main/meetings)
* Prior work : [Solid WebID Profiles](https://github.com/solid/solid-spec/blob/master/solid-webid-profiles.md)
* Prior work : [Applications Discovery](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)
* Prior work : [Original Interop Issue](https://github.com/solid/data-interoperability-panel/issues/209)
* Prior work : [WebID Draft Specification](https://www.w3.org/2005/Incubator/webid/spec/identity/)
* Prior work : [Universal Identity for the Web](https://csarven.ca/linked-research-decentralised-web#universal-identity-for-the-web)

## Team
* Virginia Balseiro
* Timea Turdean
* Jeff Zucker
