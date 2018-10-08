# COALA Intellectual Property JSON-LD / IPLD Definitions

The reference [JSON-LD](http://json-ld.org/) and [IPLD](https://github.com/ipld/specs/tree/master/ipld)
entity schemata (and their Linked Data contexts) for [COALA IP](../README.md).

Includes:

- [Overview](#overview)
    - [Vocabularies](#vocabularies)
    - [UX Considerations](#ux-considerations)
    - [IPLD Considerations](ipld-considerations)
    - [Immutable Data Considerations](#immutable-data-considerations)
    - [Cryptographically Signed Entities](#cryptographically-signed-entities)
- [Entities](#entities)
    - [Party](#rrm-party)
        - [Person](#person)
        - [Organization](#organization)
    - [Place](#rrm-place)
    - [Creation](#rrm-creation)
        - [Licensing](#licensing)
        - [Fingerprinting](#fingerprinting)
    - [Right](#rrm-right)
    - [RightsAssignment](#rrm-rightsassignment)
    - [Assertion](#rrm-assertion)
    - [RightsConflict](#rrm-rightsconflict)


## Overview

An overview and explaination of each representation can be found in the [COALA IP Specification,
under the section "Implementing the LCC RRM with Linked Data"](../README.md#coala-ip-implementing-the-lcc-rrm-with-linked-data).

### Vocabularies

Whenever possible, we try to use the direct [schema.org](schema.org) representation of an entity. If
this is not possible, we define our own classes and properties (see [top-level context](./coala_context.json)
and [definitions](./vocab/coala.jsonld)) with the goal of eventually integrating them as a hosted
extension under schema.org. Self defined properties and classes are built preferring existing
vocabularies, such as [OWL](https://www.w3.org/OWL/) and [DC](http://dublincore.org/), over custom
implementations.

### UX Considerations

There are a number of properties in the schemata that are either meant to be or can be abstracted
away from an end user by the implementation. These are usually convenience properties that help
implementations query for or understand entities; an implementation is free to decide its interface
for a user but must make sure to conform to the defined models when communicating with others or
persisting entities.

### IPLD Considerations

Although JSON-LD and IPLD are similar, there are a number of specific differences that prevent the
two from being used together at the moment (JSON-LD parsers are currently missing an IPLD resolving
mechanism; specifically, `"@id"` cannot contain a merkle-link object). However, as we expect the two
to be compatible in the future and have already relied on various IPLD properties throughout the
[COALA IP Specification](../README.md), we provide the IPLD schemata in addition to the JSON-LD
schemata.

In general, the IPLD schemata are identical to the JSON-LD schemata with the exception of [terms](https://www.w3.org/TR/json-ld/#dfn-term)
that are of have type `"@id"` (i.e. holding a [node identifier](https://www.w3.org/TR/json-ld/#node-identifiers)):
instead of holding a URI pointing to the referenced object, these terms' values instead hold
[merkle-links](https://github.com/ipld/specs/tree/master/ipld#what-is-a-merkle-link) to the object.

### Immutable Data Considerations

As the [specification foresees implementations backed by immutable ledgers](#link-me) and leverages
IPLD's guarantees of immutable data structures, all entity models have been designed to factor in
their immutability post-creation. However, there are still a few recommended patterns to follow when
working with, or extending, the models:

- Prefer flat models and register nested models separately from their parents
- Link any separated nested models (from above) back to their parents by adding "backward" links to
  the nested models (rather than "forward" links to the parents)

Adhering to these patterns helps you to avoid the creation of models that are too constrained within
an immutable context. For example, consider the [`Creation`](#rrm-creation) entity: although the
schema allows a `Work` to declare its manifestations through the [workExample property](http://schema.org/workExample),
doing so would inherently lock the `Work` into only these `Manifestation`s. You can avoid this
problem by declaring your `Work`s and `Manifestation`s separately, using the `Manifestation` class
and then link to `AbstractWork` using the `manifestationOf` (alias of [`exampleOfWork`](http://schema.org/exampleOfWork))
property.

When relying on inter-object links rather than nested objects, you may find that some properties
from schema.org expect an object or value rather than a link. To handle this, you can either use an
expanded term with an `"@id"` key:

```javascript
// For example, an Organization:
{
    "@context": "http://schema.org/",
    "@type": "Organization",
    "founder": {
        "@id": "<URI pointing to a Person object>"
    },
    ...
}
```

Or override the property's type in your own context:

```javascript
// For example, an Organization:
{
    "@context": [
        "http://schema.org/",
        {
            "schema": "http://schema.org/",
            "founder": {
                "@id": "schema:founder",
                "@type": "@id"
            }
        }
    ],
    "@type": "Organization",
    "founder": "<URI pointing to a Person object>",
    ...
}
```

For all schemata defined below, our [top-level context](./coala_context.json) has already overridden
any necessary properties to have `"@type": "@id"`. You can always override these again if you prefer
to use a nested object.

### Cryptographically Signing Entities

Although left optional for now, in the future we expect to make the signing of all COALA IP models
mandatory. This can either be done by including properties based on the [Web of Trust RDF ontology](http://xmlns.com/wot/0.1/),
or by using the properties made available by the Web Payments Community Group in their [Linked Data
Signatures schema](https://web-payments.org/specs/source/ld-signatures/).

## Entities

### RRM Party

`Party`s are represented simply by their corresponding schema.org vocabulary, [Person](http://schema.org/Person)
and [Organization](http://schema.org/Organization).

**Note**: `Party`s are still a work-in-progress ([see the list of requirements](../README.md#the-lcc-party-entity))
and are likely to be modified in the future. Additional properties or even new class definitions may
be used to create better representations in the future.

#### Person

See the [schema.org/Person definition](http://schema.org/Person) for the Linked Data context.

An example of a `Person`:

```javascript
// In JSON-LD
{
    "@context": "http://schema.org/",
    "@type": "Person",
    "@id": "<URI pointing to this object>",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to schema.org's context>" }, // For now, could also be a URI to the context
    "@type": "Person",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```

#### Organization

See the [schema.org/Organization definition](http://schema.org/Organization) for the Linked Data
context.

An example of an `Organization`:

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "Organization",
    "@id": "<URI pointing to this object>",
    "name": "World Wide Web Consortium",
    "founder": "<URI pointing to a Person object>",
    "member": [
        "<URI pointing to a Person or Organization object>",
        "<URI pointing to a Person or Organization object>",
        "<URI pointing to a Person or Organization object>"
    ]
}

// In IPLD
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "Organization",
    "name": "World Wide Web Consortium",
    "founder": { "/": "<hash pointing to a Person object>" },
    "member": [
        { "/": "<hash pointing to a Person or Organization object>" },
        { "/": "<hash pointing to a Person or Organization object>" },
        { "/": "<hash pointing to a Person or Organization object>" }
    ]
}
```

### RRM Place

Localizable `Place`s are represented simply by their corresponding schema.org vocabulary, [Place](schema.org/Place).
Virtual `Place`s are simply URIs or merkle-links. See the [schema.org/Place definition](http://schema.org/Place)
for the Linked Data context.

An example of a localizable `Place`:

```javascript
// In JSON-LD
{
    "@context": "http://schema.org/",
    "@type": "Place",
    "@id": "<URI pointing to this object>",
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    }, // Could also be a URI to a GeoCoordinate
    "name": "Empire State Building"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to schema.org's context>" }, // For now, could also be a URI to the context
    "@type": "Place",
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    }, // Could also be a merkle-link to a GeoCoordinate
    "name": "Empire State Building"
}
```

### RRM Creation

`Creation`s are represented by [schema.org/CreativeWork](http://schema.org/CreativeWork)s and its
subtypes (such as [schema.org/Book](http://schema.org/Book)). To differentiate between the different
`CreationMode`s (`lcc:Manifestation` or `lcc:Work`), we define

- our own `AbstractWork` class (subclassed from schema.org's CreativeWork) for in-perceivable creations;
- and a `Manifestation` class (subclassed from `coala:AbstractWork`) for perceivable creations.


`Manifestation`s that include a [`url`](http://schema.org/url) property are considered digital
`Manifestation`s while all others are physical `Manifestation`s. See the [schema.org/CreativeWork
definition](http://schema.org/CreativeWork) for the Linked Data context.

An additional `manifestationOf` (equivalent to [`exampleOfWork`](http://schema.org/exampleOfWork))
property is defined to link `Manifestation`s back to their `AbstractWork`. `Manifestation`s must contain
a valid link in this property, and `Work`s must not contain this property.

Our own vocabulary definitions for `Creation`s:

```javascript
// AbstractWork Class
{
    "@id": "<coalaip placeholder>/AbstractWork",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:CreativeWork"
    },
    ...
}

// Manifestation Class
{
    "@id": "<coalaip placeholder>/Manifestation",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "coala:AbstractWork"
    },
    ...
}

// ManifestationOf Property
{
    "@id": "<coalaip placeholder>/manifestationOf",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "schema:CreativeWork"
    },
    "schema:rangeIncludes": {
        "@id": "coala:AbstractWork"
    },
    "owl:equivalentProperty": {
        "@id": "schema:exampleOfWork"
    },
    ...
}
```

An example of a `Work`, and its physical and digital `Manifestation`s:

```javascript
// Note: We assume that the data will be put on an immutable ledger, forcing all links to point "backwards."
//       E.g. The Work is registered first and then the Manifestations are linked back to the Work.

// In JSON-LD
// Work
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "AbstractWork",
    "@id": "<URI pointing to this object>",
    "name": "Lord of the Rings",
    "creator": "<URI pointing to a Person or Organization object>"
}

// Digital Manifestation
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "Book",
    "@id": "<URI pointing to this object>",
    "name": "The Fellowship of the Ring",
    "manifestationOf": "<URI pointing to a AbstractWork object>",
    "isPartOf" "<URI pointing to a CreativeWork object>",
    "author": "<URI pointing to a Person or Organization object>",
    "datePublished": "29-07-1954",
    "locationCreated": "<URI pointing to a Place object>",
    "url": "<URI pointing to digital representation>"
}

// Physical Manifestation
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "Book",
    "@id": "<URI pointing to this object>",
    "name": "The Fellowship of the Ring",
    "manifestationOf": "<URI pointing to a AbstractWork object>",
    "isPartOf" "<URI pointing to a CreativeWork object>",
    "author": "<URI pointing to a Person or Organization object>",
    "datePublished": "29-07-1954",
    "locationCreated": "<URI pointing to a Place object>"
}

// In IPLD
// Work
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "AbstractWork",
    "name": "Lord of the Rings",
    "creator": { "/": "<hash pointing to a Person or Organization object>" },
}

// Digital Manifestation
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "Book",
    "name": "The Fellowship of the Ring",
    "manifestationOf": { "/": "<hash pointing to a AbstractWork object>" },
    "isPartOf" { "/": "<hash pointing to a CreativeWork object>" },
    "author": { "/": "<hash pointing to a Person or Organization object>" },
    "datePublished": "29-07-1954",
    "locationCreated": { "/": "<hash pointing to a Place object>" },
    "url": { "/": "<hash pointing to a file on e.g. IPFS>" }
}

// Physical Manifestation
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "Book",
    "name": "The Fellowship of the Ring",
    "manifestationOf": { "/": "<hash pointing to a AbstractWork object>" },
    "isPartOf" { "/": "<hash pointing to a CreativeWork object>" },
    "author": { "/": "<hash pointing to a Person or Organization object>" },
    "datePublished": "29-07-1954",
    "locationCreated": { "/": "<hash pointing to a Place object>" },
    "url": { "/": "<hash pointing to a file on e.g. IPFS>" }
}
```

Note that, elsewhere in the text, we use `Creation` to encompass both `Work`s and `Manifestation`s.

#### Licensing

Although CreativeWorks are able to hold licensing information through a [`license`](http://schema.org/license)
property, we recommend implementations to ignore this property and instead use the RRM's [`Right`
entity](#rrm-right) with their [suggested ownership semantics](../README.md#the-notion-of-ownership).
This recommendation comes especially strong if `Creation`s will be registered on an immutable
ledger.

#### Fingerprinting

Fingerprinting information can be registered for `Creation`s separately by using our own vocabulary
definitions:

```javascript
// DigitalFingerprint Class (should be subtyped to define more specific fingerprinting types, e.g. image-match)
{
    "@id": "<coalaip placeholder>/DigitalFingerprint",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:Intangible"
    },
    ...
}

// FingerprintOf Property
{
    "@id": "<coalaip placeholder>/fingerprintOf",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:DigitalFingerprint"
    },
    "schema:rangeIncludes": [
        {
            "@id": "schema:CreativeWork"
        },
        {
            "@id": "schema:URL"
        }
    ],
    "owl:equivalentProperty": {
        "@id": "schema:object"
    },
    ...
}

// Fingerprint Property
{
    "@id": "<coalaip placeholder>/fingerprint",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:DigitalFingerprint"
    },
    "schema:rangeIncludes": [
        {
            "@id": "schema:PropertyValue"
        },
        {
            "@id": "schema:Text"
        }
    ],
    "owl:equivalentProperty": {
        "@id": "schema:value"
    },
    ...
}
```

An example of adding fingerprinting information for a digital `Manifestation`:

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "DigitalFingerprint",
    "@id": "<URI pointing to this object>",
    "fingerprintOf": "<URI pointing to a CreativeWork object or media blob>",
    "fingerprint": "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY"
}

// In IPLD
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "DigitalFingerprint",
    "fingerprintOf": "<hash pointing to a CreativeWork object or media blob>",
    "fingerprint": "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY"
}
```

### RRM Right

As no existing schema.org vocabulary fits the RRM's notion of a `Right` or [COALA IP's notion of a
`Copyright`](#link-me), we define our own classes with the following vocabulary:

```javascript
// Copyright Class
{
    "@id": "<coalaip placeholder>/Copyright",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:Intangible"
    },
    ...
}

// Right Class
{
    "@id": "<coalaip placeholder>/Right",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:Intangible"
    },
    "owl:equivalentClass": {
        "@id": "dc:RightsStatement"
    },
    ...
}

// Exclusive Property
{
    "@id": "<coalaip placeholder>/exclusive",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Boolean"
    },
    ...
}

// NumberOfUses Property
{
    "@id": "<coalaip placeholder>/numberOfUses",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Number"
    },
    ...
}

// PercentageShares Property
{
    "@id": "<coalaip placeholder>/percentageShares",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Number"
    },
    ...
}

// RightContext Property
{
    "@id": "<coalaip placeholder>/rightContext",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Text"
    },
    ...
}

// RightsOf Property
{
    "@id": "<coalaip placeholder>/rightsOf",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Copyright"
    },
    "schema:rangeIncludes": {
        "@id": "schema:CreativeWork"
    },
    "owl:equivalentProperty": {
        "@id": "dc:rights"
    },
    ...
}

// Source Property
{
    "@id": "<coalaip placeholder>/source",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "coala:Copyright"
    },
    "owl:equivalentProperty": {
        "@id": "dc:source"
    },
    ...
}

// Territory Property
{
    "@id": "<coalaip placeholder>/territory",
    "@type": "rdf:Property",
    "schema:domainIncludes": [
        {
            "@id": "coala:Copyright"
        },
        {
            "@id": "coala:Right"
        }
    ],
    "schema:rangeIncludes": {
        "@id": "schema:Place"
    },
    "owl:equivalentProperty": {
        "@id": "schema:spatialCoverage"
    },
    ...
}

// UsageType Property
{
    "@id": "<coalaip placeholder>/usageType",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Text"
    },
    ...
}

// ValidFrom Property
{
    "@id": "<coalaip placeholder>/validFrom",
    "@type": "rdf:Property",
    "schema:domainIncludes": [
        {
            "@id": "coala:Copyright"
        },
        {
            "@id": "coala:Right"
        },
        {
            "@id": "schema:ReviewAction" // See section on `Assertion`s
        }
    ],
    "schema:rangeIncludes": [
        {
            "@id": "schema:Date"
        },
        {
            "@id": "schema:DateTime"
        }
    ],
    "owl:equivalentProperty": {
        "@id": "schema:validFrom"
    },
    ...
}

// ValidThrough Property
{
    "@id": "<coalaip placeholder>/validThrough",
    "@type": "rdf:Property",
    "schema:domainIncludes": [
        {
            "@id": "coala:Copyright"
        },
        {
            "@id": "coala:Right"
        },
        {
            "@id": "schema:ReviewAction" // See section on `Assertion`s
        }
    ],
    "schema:rangeIncludes": [
        {
            "@id": "schema:Date"
        },
        {
            "@id": "schema:DateTime"
        }
    ],
    "owl:equivalentProperty": {
        "@id": "schema:validThrough"
    },
    ...
}
```

**Notes**:

- Although the range of `rightsOf` includes any CreativeWork, we expect that, for the most part,
  only `Manifestation`s will be provided.
- `percentageShares` must be a number between 0 and 100.
- `rightContext` and `usageType` contain arbitrary strings that should be classified by the
  registrar of a `Right`
- `schema.org/license` can be used to provide a URL to the legal license document covering a `Right`

An example of a `Copyright` and a derived `Right`:

```javascript
// Note: We assume that the data will be put on an immutable ledger, forcing all links to point "backwards."
//       E.g. The Copyright is registered first and then the Right is linked back to the Copyright.

// In JSON-LD
// Copyright
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "<coalaip placeholder>/Copyright",
    "@id": "<URI pointing to this object>",
    "rightsOf": "<URI pointing to a CreativeWork object (usually should be a Manifestation)>",
    "territory": "<URI pointing to a Place object>",
    "validFrom": "2016-01-01",
    "validThrough": "2066-01-01"
}

// Right
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "<coalaip placeholder>/Right",
    "@id": "<URI pointing to this object>",
    "source": "<URI pointing to a Copyright object>",
    "usageType": ["all", "copy", "play"],
    "territory": "<URI pointing to a Place object>",
    "rightContext": ["inflight", "inpublic", "commercialuse"],
    "exclusive": true,
    "numberOfUses": 10,
    "percentageShares": 30,
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01",
    "license": "<URI pointing to a CreativeWork object or file (ideally on an immutable ledger)>" // Uses schema.org/license property
}

// In IPLD
// Copyright
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "<coalaip placeholder>/Copyright",
    "rightsOf": { "/": "<hash pointing to a CreativeWork object (that should be a Manifestation)>" },
    "territory": { "/": "<hash pointing to a Place object>" },
    "validFrom": "2016-01-01",
    "validThrough": "2066-01-01"
}

// Right
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "<coalaip placeholder>/Right",
    "source": { "/": "<hash pointing to a Copyright object>" },
    "usageType": ["all", "copy", "play"],
    "territory": { "/": "<hash pointing to a Place object>" },
    "rightContext": ["inflight", "inpublic", "commercialuse"],
    "exclusive": true,
    "numberOfUses": 10,
    "percentageShares": 30,
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01",
    "license": { "/": "<hash pointing to a CreativeWork object or file on e.g. IPFS>" } // Uses schema.org/license property
}
```


### RRM RightsAssignment

The COALA IP Specification models `RightsAssignment`s primarily through the transfer of assets
(`Right`s) between parties. In addition to the information inherent to these transfers, a transfer
may also contain a payload with more specific contract information concerning the exchanging
parties; this is modelled as a `RightsTransferAction` class with the following vocabulary:

```javascript
// RightsTransferAction Class
{
    "@id": "<coalaip placeholder>/RightsTransferAction",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:TransferAction"
    },
    ...
}

// TransferContract Property
{
    "@id": "<coalaip placeholder>/transferContract",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:RightsTransferAction"
    },
    "schema:rangeIncludes": [
        {
            "@id": "schema:CreativeWork"
        },
        {
            "@id": "schema:URL"
        },
        {
            "@id": "schema:Text"
        }
    ],
    ...
}
```

An example of an `RightsAssignment` payload:

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@id": "<URI pointing to this object>",
    "@type": "RightsTransferAction",
    "transferContract": [
        "<URI pointing to a CreativeWork object or file (ideally on an immutable ledger)>",
        {
            "@value": "<Contract text>"
        }
    ]
}

// In IPLD
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "RightsTransferAction",
    "transferContract": [
        { "/": "<hash pointing to a CreativeWork object or file on e.g. IPFS>" },
        {
            "@value": "<Contract text>"
        }
    ]
}
```

**Notes**:

- `transferContract` is defined to be of type `"@id"` for ease of use with URLs. A string value can
  be added by using an expanded property containing `"@value"`.
- In the context of transferring rights through a ledger, the `object`, `agent`, `participant`,
  `endTime`, and `startTime` properties are unnecessary as they are already implicitly defined in
  the containing transaction.

### RRM Assertion

[schema.org/ReviewAction](http://schema.org/ReviewAction)s are similar in design to RRM's
`Assertion`s but lack the convenience of a shared terminology with the RRM. To provide a more
familiar vocabulary for implementors, we expand `ReviewActions`s with the following properties:

```javascript
// Asserter Property
{
    "@id": "<coalaip placeholder>/asserter",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "schema:ReviewAction"
    },
    "schema:rangeIncludes": [
        {
            "@id": "schema:Person"
        },
        {
            "@id": "schema:Organization"
        }
    ],
    "owl:equivalentProperty": {
        "@id": "schema:agent"
    },
    ...
}

// AssertionSubject Property
{
    "@id": "<coalaip placeholder>/assertionSubject",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "schema:ReviewAction"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Thing"
    },
    "owl:equivalentProperty": {
        "@id": "schema:object"
    },
    ...
}

// AssertionTruth Property
{
    "@id": "<coalaip placeholder>/assertionTruth",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "schema:ReviewAction"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Boolean"
    },
    "owl:equivalentProperty": {
        "@id": "schema:result"
    },
    ...
}
```

An example of an `Assertion`:

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        <coalaip placeholder>
    ],
    "@type": "ReviewAction",
    "@id": "<URI pointing to this object>",
    "asserter": "<URI pointing to a Person or Organization object>",
    "assertionTruth": false,
    "assertionSubject": "<URI pointing to a schema:Thing object>",
    "error": "author",
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01"

}

// In IPLD
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "ReviewAction",
    "asserter": { "/": "<hash pointing to a Person or Organization object>" },
    "assertionTruth": false,
    "assertionSubject": { "/": "<hash pointing to a schema:Thing object>" },
    "error": "author",
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01"
}
```

**Note**: For falsy `Assertion`s, the `error` property is left to the asserting party to explain the
reason the subject was invalid (e.g. the `author` property was incorrect).

### RRM RightsConflict

TBD
