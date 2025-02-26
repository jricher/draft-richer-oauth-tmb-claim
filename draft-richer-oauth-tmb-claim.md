---
title: "Deferred Key Binding for OAuth"
abbrev: "TODO - Abbreviation"
category: info

docname: draft-richer-oauth-tmb-claim-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Security"
workgroup: "Web Authorization Protocol"
keyword:
 - security
 - trust
venue:
  group: "Web Authorization Protocol"
  type: "Working Group"
  mail: "oauth@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/oauth/"
  github: "jricher/draft-richer-oauth-tmb-claim"
  latest: "https://jricher.github.io/draft-richer-oauth-tmb-claim/draft-richer-oauth-tmb-claim.html"

author:
 -
    fullname: Justin Richer
    organization: Bespoke Engineering
    email: ietf@justin.richer.org
 -
    fullname: Brian Campbell
    organization: Ping Identity
    email: bcampbell@pingidentity.com
 -
    fullname: Dean Saxe
    organization: Beyond Identity
    email: dean@thesax.es

normative:
- RFC9449
- RFC8705
- RFC7800

informative:


--- abstract

Sometimes you want to get a token that's tied to a public key but you can't prove possession of that public key. That's when you need to trust me, bruh.

--- middle

# Introduction

The JWT confirmation claim "cnf" {{RFC7800}} allows an RS to determine PoP semantics for a token. However, methods like mTLS {{RFC8705}} and DPoP {{RFC9449}} assume that the requester of that token can prove possession of the key that is bound to the token. Sometimes that's just too hard, and so here we define a claim to allow a requester to ask for a token that is bound to a key that the requester cannot, does not want to, or does not feel like proving possession of at the time of request.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Token Meta-key Binding Claim

The token meta-key binding "tmb" claim is passed in an OAuth token request parameter of the same name. Its value is the form-encoded value that the requester wants the confirmation claim to be in the issued token. If the requester is making a DPoP or mTLS request, any keys there MUST be ignored.

The same parameter can be used in a token exchange request, with the same results.

If the issued token is a JWT, the value of the "tmb" claim is copied to the "cnf" claim of the resulting token. If the token is introspected, the value of "tmb" is returned in the "cnf" claim of the introspection response. 

The requester SHOULD give the resulting token to the holder of the key represented in the "tmb" claim.

The presenter of the token MUST prove possession of the key in the resulting "cnf" claim using the appropriate key validation method for the type of token.

# Security Considerations

The security of this specification is entirely based on trust. If you have trust issues, it's not going to be secure.

# IANA Considerations

This document registers the "tmb" claim to the IANA JWT Claims registry and OAuth request parameter.


--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
