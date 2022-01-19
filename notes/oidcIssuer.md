# solid:oidcIssuer

See [revant discussion about having an implicitly trusted issuer](https://github.com/solid/solid-oidc/issues/51) (i.e. server determines it rather than it occurring in the WebID document).  

Also note that @acoburn confirms in gitter that a single WebID can have multiple issuers and describes the app discovery process when there are multiple:
<blockquote>
Jeff Zucker @jeff-zucker 12:01 Is it possible to have multiple oidc:Issuers associate with a single WebID? Is there a use case for it? I mean something like <#me> soid:oidcIssuer <#a>, <#b>.

Sarven Capadisli @csarven 12:05
With respect to RDF, of course. As for its definition, probably not. "preferred" issuer and all. Besides, I'm not sure if/how more 
than one authority can issue a WebID.
So, I'm wrong:
OpenID Connect supports multiple Issuers per Host and Port combination.
https://openid.net/specs/openid-connect-core-1_0.html#IssuerIdentifier

Jeff Zucker @jeff-zucker 12:07
Okay, was wondering if I had two and one was down an app could try another

Sarven Capadisli @csarven 12:07
I'll defer to @acoburn to make sense of what's intended =)
https://solid.github.io/solid-oidc/#resource-access-validation acknowledges multiple values.

Aaron Coburn @acoburn 12:10
It is entirely possible (and even expected) that a single WebID would have multiple OIDC issuers. This is how we enable federated trust
across identity providers in Solid. There is no concept of "preferred" issuer, only that a certain set of issuers is trusted
From an app/user interaction perspective, the pattern I would follow is this: first, ask a user for their WebID. Next, use 
the set of OIDC issuers to provide them a login flow -- if there is a single issuer, immediately direct a user to that IdP;
if there are multiple issuers, ask the user to choose from that set (but there is no need to offer a login flow with an IdP
that is not trusted for a given WebID)
  </blockquote>
