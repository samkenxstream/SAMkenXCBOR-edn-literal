---
title: >
  Application-Oriented Literals in CBOR Extended Diagnostic Notation
abbrev: CBOR EDN Literals
docname: draft-bormann-cbor-edn-literals-latest
date: 2021-10-06

stand_alone: true

ipr: trust200902
keyword: Internet-Draft
cat: info
# consensus: true

pi: [toc, sortrefs, symrefs, compact, comments]

author:
  -
    ins: C. Bormann
    name: Carsten Bormann
    org: Universität Bremen TZI
    street: Postfach 330440
    city: Bremen
    code: D-28359
    country: Germany
    phone: +49-421-218-63921
    email: cabo@tzi.org

venue:
  mail: cbor@ietf.org
  github: cabo/edn-literal

normative:
  RFC8610: cddl
  RFC8949: cbor
  I-D.ietf-core-href: cri
  RFC3339: datetime
  RFC3986: uri
  RFC3987: iri
informative:
  RFC4648: base
  I-D.ietf-core-coral: coral

--- abstract

The Concise Binary Object Representation, CBOR (RFC 8949), [^abs1-]

[^abs1-]: defines a "diagnostic notation" in order to
    be able to converse about CBOR data items without having to resort to
    binary data.

[^abs3-]: This document specifies how to add application-oriented extensions to
    the diagnostic notation.  It then defines two such extensions for the use of
    CBOR diagnostic notation with CoRAL and Constrained Resource Identifiers

​[^abs3-] (draft-ietf-core-coral, draft-ietf-core-href).


--- to_be_removed_note_

The content of this draft may preferably be
distributed to a number of different documents.
This is to be decided.

--- middle

Introduction        {#intro}
============

For the Concise Binary Object Representation, CBOR,
{{Section 8 of -cbor}} in conjunction with {{Appendix G of -cddl}}
[^abs1-]
Diagnostic notation is based on JSON, with extensions
for representing CBOR constructs such as binary data and tags.
[^abs2-]

[^abs2-]: (Standardizing this together with the actual interchange format does
    not serve to create another interchange format, but enables the use of
    a shared diagnostic notation in tools for and documents about CBOR.)

[^abs3-] {{-coral}} {{-cri}}.



Application-Oriented Extension Literals
=======================================

This document extends the syntax used in diagnostic notation for byte
string literals to also be available for application-oriented extensions.

As per {{Section 8 of -cbor}}, the diagnostic notation can notate byte
strings in a number of {{-base}} base encodings, where the encoded text
is enclosed in single quotes, prefixed by an identifier (>h< for
base16, >b32< for base32, >h32< for base32hex, >b64< for base64 or
base64url).

This syntax can be thought to establish a name space, with the names
"h", "b32", "h32", and "b64" taken, but other names being unallocated.
The present specification defines additional names for this namespace,
which we call *application-extension identifiers*.
For the quoted string, the same rules apply as for byte strings.
In particular, the escaping rules of JSON strings are applied
equivalently for application-oriented extensions, e.g., `\\` stands
for a single backslash and `\'` stands for a single quote.

An application-extension identifier is a name consisting of a
lower-case ASCII letter (a-z) and zero or more additional ASCII
characters that are either lower-case letters or digits (a-z0-9).

Application-extension identifiers are registered in a registry
({{sec-iana}}).
Prefixing a single-quoted string, an application-extension identifier
is used to build an application-oriented extension literal, which
stands for a CBOR data item the value of which is derived from the
text given in the single-quoted string using a procedure defined in
the specification for an application-extension identifier.

Examples for application-oriented extensions to CBOR diagnostic
notation can be found in the following sections.


The "cri" Extension
===================

The application-extension identifier "cri" is used to notate a
Constrained Resource Identifier literal as per {{-cri}}.

The text of the literal is a URI Reference as per {{-uri}} or an IRI
Reference as per {{-iri}}.

The value of the literal is a CRI that can be converted to the text of
the literal using the procedure of {{Section 6.1 of -cri}}.
Note that there may be more than one CRI that can be converted to the
URI/IRI given; implementations are expected to favor the simplest
variant available and make non-surprising choices otherwise.


As an example, the CBOR diagnostic notation

~~~ CBORdiag
cri'https://example.com/bottarga/shaved'
~~~

is equivalent to

~~~ CBORdiag
[-4, ["example", "com"], ["bottarga", "shaved"]]
~~~


The "dt" Extension
==================

The application-extension identifier "dt" is used to notate a
date/time literal that can be used as an Epoch-Based Date/Time as per
{{Section 3.4.2 of -cbor}}.

The text of the literal is a Standard Date/Time String as per
{{Section 3.4.1 of -cbor}}.

The value of the literal is a number representing the result of a
conversion of the given Standard Date/Time String to an Epoch-Based
Date/Time.
If fractional seconds are given in the text (production
`time-fraction` in {{Section A of -datetime}}), the value is a
floating-point number; the value is an integer number otherwise.

As an example, the CBOR diagnostic notation

~~~ CBORdiag
dt'1969-07-21T02:56:16Z'
~~~

is equivalent to

~~~ CBORdiag
-14159024
~~~


IANA Considerations {#sec-iana}
===================

IANA is requested to create a registry [[where?]] for
application-extension identifiers, with the initial content shown in
{{tab-iana}}.

| application-extension identifier | description                     | reference |
|----------------------------------|---------------------------------|-----------|
| h                                | Reserved                        | RFC8949   |
| b32                              | Reserved                        | RFC8949   |
| h32                              | Reserved                        | RFC8949   |
| b64                              | Reserved                        | RFC8949   |
| cri                              | Constrained Resource Identifier | RFCthis   |
| dt                               | Date/Time                       | RFCthis   |
{: #tab-iana title="Initial Content of application extension
identifier registry"}

[^todo1]

[^todo1]: (Define policy; detailed template)

Security considerations
=======================

The security considerations of {{-cbor}} and {{-cddl}} apply.

[^todo2]

[^todo2]: Anything else meaningful to say here?

--- back

Acknowledgements
================
{: numbered="no"}

The concept of application-oriented extensions to diagnostic notation,
as well as the definition for the "dt" extension were inspired by the
CoRAL work by Klaus Hartke.

<!--  LocalWords:  dedenting dedented
 -->
