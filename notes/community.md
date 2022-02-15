From the [chat](https://gitter.im/solid/profile-spec?at=6209476003f270478232fa54)
Jeff Zucker @jeff-zucker Feb 13 10:01
@timbl - In your type index diagram the solid:Community class refers to something labeled as "An Organization". Do you see "Community" as a replacement for "Orgainization" or are they two different things? Would an enterprise list its Agent type as solid:Community?
Tim Berners-Lee @timbl Feb 13 10:02
They are slightly differeent
A solid:Communty is about the relationship with me, i am involved in it. Maybe it would be clearer to say <#me> solid:involvedIn solidProject:me with a single arc.
If I( put it in my type index then I ant my type index implicitly to contain also the type indexes of the community.
A solid:Communkiy is a schema:Organization which shares its solid pod resources with the its members.
Jeff Zucker @jeff-zucker Feb 13 10:07
I think a predicate might be better unless you see an Organization and a Community as two different classes of agents.
Tim Berners-Lee @timbl Feb 13 10:07
Community is a subclass of Organization.
Organization is quite broad.
Jeff Zucker @jeff-zucker Feb 13 10:09
It seems from your description is that a Community is an Organization that I belong to. How else would they differ?
I suppose an Organization would not necessarily have open addressBooks so a Community would be an Organization that you could expect to be able to link to other address books with?
Tim Berners-Lee @timbl Feb 13 10:16
Portnand Oregon (say) at the moment is something which you belong to but it doesn’t share resources and action with you on a solid pod. So it is a community but it is not solidCommunity. https://www.wikidata.org/wiki/Q6106 does not have type indexes.
Its about whether it supoorts the protocol of finding community resources thriugh typeindexes
Jeff Zucker @jeff-zucker Feb 13 10:19
That makes a lot of sense. So how would this relate to agent type in the org/community's profile. Would they have <#us> a solid:Organization, solid:Community.
Tim Berners-Lee @timbl Feb 13 10:20
" a Community would be an Organization that you could expect to be able to link to other address books with” - yes exactly Address book, Task lists, Chats, ...
Jeff Zucker @jeff-zucker Feb 13 10:21
Because profiles can belong to several types of agents and are not necessarily a "Personal Profile Document", I propose the following :
solid:ProfileDocument
solid:Person
solid:Organization
solid:Community
solid:SoftwareAgent
Tim Berners-Lee @timbl Feb 13 10:21
I suppose we could me exlsit with a link like <#me> alsoUsesResourcesOf org:me . What predicate would would there
Jeff Zucker @jeff-zucker Feb 13 10:22
schema:affiliation?
no, not quite
solid:participatesIn
Tim Berners-Lee @timbl Feb 13 10:24
solid:involvedIn ?
Jeff Zucker @jeff-zucker Feb 13 10:24
partOf?
Tim Berners-Lee @timbl Feb 13 10:25
No, prtOf is for subOorganizations
Jeff Zucker @jeff-zucker Feb 13 10:27
in the typeIndex it could be simply `a solid:TypeIndex; solid:forClass solid:Community; solid:instance <CommunityWebID>.
Because I might belong to many
Tim Berners-Lee @timbl Feb 13 10:27
I agree Personal Profile Document does seem limited to Persons.
Jeff Zucker @jeff-zucker Feb 13 12:06
An Organization might have open resources only for its executive board and not for the general public or employees. That Organization is not a Community for the public but is a Community for the board members. So I would be in favor of having only a single class and using a predicate/inverse to show the community relationship.

In the board member's profile
<#Me> solid:communityMember [a solid:Organization] .

In the organization's profile
<#Us> a solid:Organization; solid:communityMember <#BoardMember> .

In the board member's typeIndex

   [                                                                            
     a solid:TypeRegistration ;                                                 
     forClass solid:Organization;                                                 
     solid:instance [a solid:Organization]                                      
   ] .
Tim Berners-Lee @timbl Feb 13 12:12
                                                                            # yes well
   <#Me> is solid:communityMember of [a solid:Organization] .
Jeff Zucker @jeff-zucker Feb 13 12:15
So, [a solid:Organization] solid:communityMember <#Me>. and <#Me> solid:communityMemberOf [a solid:Organization].
Timea @theRealImy 01:54
Interesting. I follow what you say, I agree with the concluded predicate names. Question: is this already implemented/existent anywhere or is this something new we are currently defining? Are we supposed to define new stuff?
Timea
@theRealImy
02:29
Follow up question: if we continue this - is this part of the Type Index description or better-off part of the 'Organization' profile description?

Tim Berners-Lee
@timbl
03:00
We have not implemnted the communities

Tim Berners-Lee
@timbl
03:00
The rest of the type index we have.

Tim Berners-Lee
@timbl
03:00
SO I’me happy for the spec to only have that in

Tim Berners-Lee
@timbl
03:01
and keep the cmmunities but until we haev implmented it

Timea
@theRealImy
03:02
Thank you for clarifying. We shall discuss it in the team tonight.
The decision should be about:
1) release 1st version of profile spec with WHAT IS now implemented
2) add Communities to 1st profile spec because it was already underway

Tim Berners-Lee
@timbl
03:02
“Organization” .. I don’t think so as the act of being in a commnty is a propoerty of an indivudal — well an organization can too you first think aout communitye members s ing for people

Timea
@theRealImy
03:03
ok, agree.
