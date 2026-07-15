---
title: "Identity Verification Methods Values"
#abbrev: ""
category: std

docname: draft-skyfire-oauth-id-verification-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
#number:
date:
consensus: true
v: 3
area: Security
workgroup: Web Authorization Protocol
keyword:
 - agent
 - identity
 - agentic
 - payment
 - commerce
venue:
  github: "skyfire-xyz/draft-skyfire-oauth-id-verification"
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  latest: "https://skyfire-xyz.github.io/draft-skyfire-oauth-id-verification/draft-skyfire-oauth-id-verification.html"

author:
-
  name: Ankit Agarwal
  organization: Skyfire Systems Inc.
  email: ankit_agarwal@yahoo.com
  uri: https://skyfire.xyz
-
  ins: M. Jones
  name: Michael B. Jones
  organization: Self-Issued Consulting
  email: michael_b_jones@hotmail.com
  uri: https://self-issued.info/
-
  name: Nash Ali
  organization: Experian
  email: nash.ali@experian.com
-
  name: Srinivasa Thumma
  organization: Akamai
  email: sthumma@akamai.com

normative:
  RFC7519:

informative:
  RFC5226:
  OpenID.Core:
    target: https://openid.net/specs/openid-connect-core-1_0.html
    title: "OpenID Connect Core 1.0 incorporating errata set 2"
    date: 2023-12-15
    author:
      - name: Nat Sakimura
      - name: John Bradley
      - name: Michael B. Jones
      - name: Breno de Medeiros
      - name: Chuck Mortimore
  IANA.JWT.Claims:
    author:
    - name: IANA
    target: https://www.iana.org/assignments/jwt
    title: JSON Web Token Claims
  IANA.AMR:
    author:
    - name: IANA
    target: https://www.iana.org/assignments/authentication-method-reference-values
    title: Authentication Method Reference Values

...

--- abstract

Knowing how a person's identity was verified can be important when making trust decisions.
This specification defines a claim and values for declaring how the person's identity was verified.


--- middle

# Introduction

Knowing how a person's identity was verified can be important when making trust decisions.
This specification defines the Identity Verification Methods (ivm) claim and values for it
for declaring how the person's identity was verified.
It also creates a registry for Identity Verification Methods Values
and initializes the registry with the values defined in this specification.

While this claim and values are general purpose
and can be used in any JSON Web Token (JWT) {{RFC7519}},
one use case for them is use in KYAPay tokens {{?I-D.skyfire-oauth-kyapay-token}}.


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Identity Verification Methods {#ivm}

This section defines the "ivm" (Identity Verification Methods) claim and
values used with it to indicate that particular identity verification methods
were used.
In many ways, this parallels the "amr" (Authentication Methods References) claim
defined in {{OpenID.Core}}.
Like "amr", "ivm" is a JWT claim whose value is
an array that lists a set of methods that were used,
in this case, as identity verification methods,
rather than authentication method references.

## "ivm" (Identity Verification Methods) Claim {#ivmClaim}

The "ivm" (Identity Verification Methods) claim is a
JSON array of case-sensitive strings that are identifiers for
identity verification methods used.
For instance, values might indicate that both
physical document verification and database PII verification methods were used.
Values used in the "ivm" claim SHOULD be from those registered in the
IANA "Identity Verification Methods" registry
established by (#ivmRegistry);
parties using this claim will need to agree upon the meanings of
any unregistered values used, which may be context specific.

## Identity Verification Method Values {#ivmValues}

The following Identity Verification Method values
are defined by this specification.

### "dbv" (Database Verification of PII) Method {#dbvMethod}

dbv:
: Database Verification of PII (match of name/address/dob/ssn/nid, etc.)
  using an unspecified number of consumer reporting data sources

### "dbv1" (Database Verification of PII One Source) Method {#dbv1Method}

dbv1:
: Database Verification of PII (match of name/address/dob/ssn/nid, etc.)
  using one consumer reporting data source

### "dbvm" (Database Verification of PII Multiple Sources) Method {#dbvmMethod}

dbvm:
: Database Verification of PII (match of name/address/dob/ssn/nid, etc.)
  using Multiple consumer reporting data sources

### "dig" (Digital ID Document Verification) Method {#digMethod}

dig:
: Digital ID Document Verification (for example Mobile Driver's Licenses)

### "phy" (Physical ID Document Verification) Method {#phyMethod}

phy:
: Physical ID Document Verification (for example via a real time capture of the front and back of DL or Passport photo page)

### "sec" (Secondary Document Verification) Method {#secMethod}

sec:
: Secondary Document Verification (for example via bank statements, financial statements, utility bills, government-issued papers, etc.)


# Security Considerations

The security considerations defined in
JSON Web Token (JWT) {{RFC7519}}
apply to this specification.


# Privacy Considerations

The privacy considerations defined in
JSON Web Token (JWT) {{RFC7519}}
apply to this specification.


# IANA Considerations

## JSON Web Token Claims Registration

This specification registers the following Claim in the
IANA "JSON Web Token Claims" {{IANA.JWT.Claims}}
established by {{RFC7519}}.

### "ivm" Claim

* Claim Name: ivm
* Claim Description: Identity Verification Methods
* Change Controller: Michael B. Jones - michael_b_jones@hotmail.com
* Reference: (#ivmClaim) of this specification

## Identity Verification Methods Registry {#ivmRegistry}

This specification establishes the IANA "Identity Verification Methods" registry
for `ivm` claim array element values.
The registry records the Identity Verification Method value
and a reference to the specification that defines it.
This specification registers the Identity Verification Method values
defined in (#ivmValues).

Values are registered on an Expert Review
{{RFC5226}} basis after a three-week review period on the &lt;jwt-reg-review@ietf.org&gt; mailing
list, on the advice of one or more Designated Experts.
To increase potential interoperability, the Designated Experts are requested to encourage
registrants to provide the location of a publicly accessible specification
defining the values being registered,
so that their intended usage can be more easily  understood.

Registration requests sent to the mailing list for review should use
an appropriate subject
(e.g., "Request to register Identity Verification Method value: example").

Within the review period, the Designated Experts will either approve or
deny the registration request, communicating this decision to the review list and IANA.
Denials should include an explanation and, if applicable, suggestions as to how to make
the request successful.
Registration requests that are undetermined for
a period longer than 21 days can be brought to the IESG's attention
(using the <iesg@ietf.org> mailing list) for resolution.

IANA must only accept registry updates from the Designated Experts and should direct
all requests for registration to the review mailing list.

It is suggested that the same Designated Experts evaluate these
registration requests as those who evaluate registration requests
for the IANA "Authentication Method Reference Values" registry {{IANA.AMR}}.

Criteria that should be applied by the Designated Experts include
determining whether the proposed registration duplicates existing functionality;
whether it is likely to be of general applicability
or whether it is useful only for a single application;
whether the value is actually being used;
and whether the registration description is clear.

### Registration Template

Identity Verification Method Name:
: The name requested (e.g., "dig") for the authentication method
  or family of closely related authentication methods.
  Because a core goal of this specification is for the resulting
  representations to be compact, it is RECOMMENDED that the name be short
  -- that is, not to exceed 8 characters without a compelling reason to do so.
  To facilitate interoperability, the name must use only
  printable ASCII characters excluding double quote ('"') and backslash ('\\')
  (the Unicode characters with code points U+0021, U+0023 through U+005B, and
  U+005D through U+007E).
  This name is case sensitive.
  Names may not match other registered names in a case-insensitive manner
  unless the Designated Experts state that there is a compelling reason
  to allow an exception.

Identity Verification Method Description:
: Brief description of the Identity Verification Method
  (e.g., "Physical ID Document verification").

Change Controller:
: For Standards Track RFCs, state "IETF". For others, give the name of the
  responsible party. Other details (e.g., postal address, email address, home page
  URI) may also be included.

Specification Document(s):
: Reference to the document or documents that specify the parameter,
  preferably including URIs that
  can be used to retrieve copies of the documents.
  An indication of the relevant
  sections may also be included but is not required.


### Initial Registry Contents

#### "dbv" Method

* Identity Verification Method Name: dbv
* Identity Verification Method Description: Database Verification of PII
* Change Controller: IETF
* Reference: (#dbvMethod) of this specification

#### "dbv1" Method

* Identity Verification Method Name: dbv1
* Identity Verification Method Description: Database Verification of PII One Source
* Change Controller: IETF
* Reference: (#dbv1Method) of this specification

#### "dbvm" Method

* Identity Verification Method Name: dbvm
* Identity Verification Method Description: Database Verification of PII Multiple Sources
* Change Controller: IETF
* Reference: (#dbvmMethod) of this specification

#### "dig" Method

* Identity Verification Method Name: dig
* Identity Verification Method Description: Digital ID Document Verification
* Change Controller: IETF
* Reference: (#digMethod) of this specification

#### "phy" Method

* Identity Verification Method Name: phy
* Identity Verification Method Description: Physical ID Document Verification
* Change Controller: IETF
* Reference: (#phyMethod) of this specification

#### "sec" Method

* Identity Verification Method Name: sec
* Identity Verification Method Description: Secondary Document Verification
* Change Controller: IETF
* Reference: (#secMethod) of this specification


--- back

# Document History
{: numbered="false"}

[[ to be removed by the RFC Editor before publication as an RFC ]]

-00

* Initial draft.
