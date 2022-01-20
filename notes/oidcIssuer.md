# solid:oidcIssuer (draft WiP)

An OIDC Issuer is the service which provides login and authentication for a Solid WebID owner. An OIDC Issuer may be run as part of a Solid`Identity Provider` or `IdP`. 

An app operating on behalf of a WebID owner that wants to login needs to find the OIDC Issuer. Such an app  SHOULD look for triples in the WebID document in the form `<?WebID> solid:oidcIssuer <?Issuer>.

If the app needs to login on behalf of the WebID owner and can not find a triple with the solid:oidcIssuer predicate, it SHOULD inform the WebID owner that their WebID document is broken.

If a single Issuer triple is found, the app wanting to login SHOULD redirect to the URL specifed by the value of ?Issuer.  

If multiple Issuer's are found, the app should offer the user a choice and redirect to the chosen Issuer.


## Background notes

### Implicit Trust
See [revant discussion about having an implicitly trusted issuer](https://github.com/solid/solid-oidc/issues/51) (i.e. server determines it rather than it occurring in the WebID document).  

Aaron sums up in gitter :
<blockquote>
Jeff Zucker @jeff-zucker 13:38
So with implicitly trusted issuers if such continues to be a thing, the app finds no oidcIssuer triple so then munges the WebID URI to find the Issuer? and/or looks in a link header?

Aaron Coburn @acoburn 14:02
We have discussed implicitly trusted issuers in the authN panel, and the general consensus at present is to not allow such implicit trust. There are a number of cases where that implicit trust becomes a major problem. Generally, when defining trust models, any "implicit trust" that is outside of the user's control becomes (potentially) really problematic from a security perspective.

Short answer: don't rely on implicit trust. If you do, don't expect that you will be able to interoperate with other Solid servers
</blockquote>

### One WebID with Multiple oidcIssuers

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
  
### Zero oidcIssuers
<bockquote>  
Jeff Zucker @jeff-zucker 14:03
Again, very helpful, thanks.
So if I'm an app and I don't find any solid:oidcIssuer triles in the WebID document, all I can do is ask the user

Aaron Coburn @acoburn 14:05
Yes, the fallback can always be "ask the user"
  </blockquote>
