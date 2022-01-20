# WebIDs and Social Agent

A Social Agent MAY have multiple WebIDs.  Each WebID belongs to exactly one Agent.  .

## Background Notes

There isn't a way (or reason) to forbid people from having multiple WebIDs.  So this should be a MAY.

I had this interesting chat with Fred Gibson of Trinpod.  I don't think it impacts what we should say about multiple WebIDs, but is related.

```
Jeff Zucker @jeff-zucker 11:38
Hey Fred, I'm am working on a spec for app knowledge discovery from profile and/or WebID documents. Could you take a look at this diagram and let me know in what ways, if any, this would conflict with trinpod ? https://github.com/solid/webid-profile/blob/main/notes/profile-infrastructure.png
Fred Gibson @gibsonf1 11:45
:thumbsup:
We actually treat the webid as the uri that represents that person (or company, or project, or building, or car etc) in the world
So it looks from the diagram that you have a completely different thing elsewhere that represents the person...
image.png
Jeff Zucker @jeff-zucker 11:47
represents that persona of the social agent? e.g. if I have three webids, they could represent three parts of me
Fred Gibson @gibsonf1 11:47
the social agent I think
Jeff Zucker @jeff-zucker 11:48
yes social agent = person , group, or organizatin
Fred Gibson @gibsonf1 11:48
Yes, you could have a webid for your left arm, one for your right arm, and one main one for you that has the others as parts
Jeff Zucker @jeff-zucker 11:48
yep
but each webid only relates to one of them
except for the main one
Fred Gibson @gibsonf1 11:48
So I'm of the opinion that the webid is the uri that represents that thing
Jeff Zucker @jeff-zucker 11:49
yes
Fred Gibson @gibsonf1 11:49
I will only have one webid that represents me as a person. Then I'll have permissions and links to many others, like companies I'm in, groups im in etc
I guess your diagram makes it look like the webid is a separate object from the social agent
Jeff Zucker @jeff-zucker 11:50
yes but I'm going to have two webids that represent me as a person , one for my public self and one for my private self
Fred Gibson @gibsonf1 11:50
<webid> rdf:type <agent>
Jeff Zucker @jeff-zucker 11:50
nope
Fred Gibson @gibsonf1 11:51
See,thats crazy, why not just use permissions?
Jeff Zucker @jeff-zucker 11:51
which part is crazy?
Fred Gibson @gibsonf1 11:51
that you silo your public from your private in different locations
you can just use acls
Jeff Zucker @jeff-zucker 11:52
what if I don't even want the keeper of my pubic webid to even know I have a second?
Fred Gibson @gibsonf1 11:52
as what you want public vs private can change alot
Jeff Zucker @jeff-zucker 11:52
I have the right to be the only person on the planet who knows all my webids
Fred Gibson @gibsonf1 11:52
There is nothing preventing anyone from having a secret webid etc
But in theory, having a webid doesn't give anyone access to anything you don't want them to access
Jeff Zucker @jeff-zucker 11:54
If both my pubic and private webids are on the same IdP, that IdP knows I own both
Fred Gibson @gibsonf1 11:54
How? If you use different emails for example
we just have a verified email address per pod, so you can go crazy making them and we wouldn't know a thing
Jeff Zucker @jeff-zucker 12:02
Well, as a dev I have about 15 webids to test various stuff. There are others who already have multiple for other reasons. ISo the spec will probably say that an Agent MAY have multiple WebIDS which would not in any way conflict with Trinpods insistence that they only have one since they still MAY get another webid from another provider.
I don't think it's possible to say one MUST have only a single webid at the spec level though at he implementation level, go for it!
Does that make sense?
BTW, would you mind if I copy this conversation into the notes section of the spec repo?
Fred Gibson @gibsonf1 12:13
Oh no, we don't insist that people only have 1, we just basically treat every pod as a space/time model of the world
I'm just saying semantically that the uri of the webid and the agent are the same
Jeff Zucker @jeff-zucker 12:14
Ah!
Fred Gibson @gibsonf1 12:15
that way we can sematically link with webids to other webids and effectively have a global knowledge graph of the world that makes sense
so I can give access to a whole company's current employees by giving access to the webid of that company for example
and I can have a Site Pod (with webid) contain a Building Pod (with webid) etc
For me personally, I want just one personal pod that represents me so I don't have to remember x number of passwords etc, but most importantly I can track my states of everything about me linked through space time
Jeff Zucker @jeff-zucker 12:18
In the diagram that's the one side of the one-to-many between Agent and WebID - the WebID always represents exactly one Agent, which if it's an Organization, includes its members
Fred Gibson @gibsonf1 12:18
It just looks like the diagram represents triples, and so the agent would have a different uri than the webid (thats how I read it)
Jeff Zucker @jeff-zucker 12:19
Oh no sorry, the Agent in this case is the actual person or organization, I didn't make that very cear
Fred Gibson @gibsonf1 12:20
maybe have a dashed line and a person icon?
In that case, no issues with it
Jeff Zucker @jeff-zucker 12:21
yeah, that would work, did you see my question about copying this convo into the repo? would that be ok with you?
Fred Gibson @gibsonf1 12:21
Oh sure, np
Jeff Zucker @jeff-zucker 12:22
Thanks. Anything else in the diagram that might conflict?
Fred Gibson @gibsonf1 12:23
image.png
What does this mean for the issuer side?
Also, I think it would be good to make crystal clear that all the triples are using the webid rather than the profile uri
Oh I see, dereferences to
But I would make all that stuff MUST :)
what does the double arrowhead mean to the oidc issuer?
Jeff Zucker @jeff-zucker 12:29
Yes, for sure , I should specify webid as subject, excellent point, The double arrow on Issuer means that one IdP is the issuer of multiple WebIDs and that each WebID can have multiple Issuers who can authenticate it. Aaron says multiple Issuers per webid is a key of federated trust.
Fred Gibson @gibsonf1 12:38
I saw that discussion, and I don't quite understand how you could have multiple issuers on a webid as the webid is a specific place on the web you go to
Jeff Zucker @jeff-zucker 12:42
I'm not entirely clear how it would work either. Oh good, you asked him :-)
Fred Gibson @gibsonf1 12:42
Thanks - I've been so busy launching that I havent had time to dive in as much lately
Jeff Zucker @jeff-zucker 12:43
I hear that, good luck on the launch!Fred Gibson @gibsonf1 12:46
Thanks :)
```
