# Discovery based on Solid Social Agent WebId

## Overview

When an app allows a user to log in to a Solid Identity Provider, the app receives back an authenticated identifier for the user - their WebId.  The general purpose of this document is to describe what an app can discover based on the WebId.  More specific purposes are described below.

A Social Agent - a person, group, or organization - may own one or more WebIds.  Each WebId is a URI that can be dereferenced to point to exactly one document, hereafter called a `profile document`.  The term 'profile document', as used here, means a `Social Agent WebId Document` and does not include WebId documents associated with Software Agents.  

Profile documents may optionally point, through various means, to other documents whose intent is to identify the WebId owner and/or describe details about their data.  These other documents, whatever predicates are used to indicate them, and however they are named, will be referred to collectively as `extended profile documents`.  

The term `profile` will be used here to refer to the data that can be gathered in the profile document and all extended profile documents it points to.

Profiles contain two major types of information : 1) identifiying information such as a person or group's name, purpose, contacts,etc. and 2) organizing information such as what types of data the user has, where it is stored, and who has access to it. This document will focus on the latter - the information that an app can use to learn how to interact with the data associated with the Social Agent's WebId.

There is great variability in what information is found in the profile and which documents the information is stored in.  The [interoperability panel](https://solid.github.io/data-interoperability-panel/specification/) is in the process of creating new organization of the profile information. So, there can be more than one discovery process.  This specification aims to describe the discovery process as it is currently in use.

To sum up, this document aims to cover

* Social Agents, not Software Agents
* Profile documents and extended profile documents
* Discovery of data structures, not personal/group identifying information
* Current practices, not future specifications

The following predicates will be the main foucs of the documention :

* foaf:primaryTopic
* foaf:isPrimaryTopicOf
* solid:oidcIssuer
* solid:account
* solid:publicTypeIndex
* solid:privateTypeIndex
* pim:storage
* pim:preferencesFile
* acl:trustedApp (possibly omit this for this spec?)
* owl:sameAs
* rdfs:seeAlso

## Related Resources
* [Minutes of meetings](https://github.com/solid/webid-profile/tree/main/meetings)
* Prior work : [WebId Specifcation](https://www.w3.org/2005/Incubator/webid/spec/identity/)
* Prior work : [Original Interop Issue](https://github.com/solid/data-interoperability-panel/issues/209)
* Prior work : [Solid WebId Profiles](https://github.com/solid/solid-spec/blob/master/solid-webid-profiles.md)
* Prior work : [Applications Discovery](https://github.com/solid/solid/blob/main/proposals/data-discovery.md)

## Team
* Virginia Balseiro
* Timea Turdean
* Jeff Zucker

