# COALA Intellectual Property JSON-LD / IPLD Definitions

*Ideally, this will be in its own separate repo under COALA with a better name. Make sure to fix
links when moving repos.*

The definitive [JSON-LD](http://json-ld.org/) and [IPLD](https://github.com/ipld/specs/tree/master/ipld)
entity schemata (and their Linked Data contexts) for [COALA IP](https://github.com/ascribe/specs/tree/master/coala-ip).

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
under the section "Remodeling the LCC RRM with Linked Data"](https://github.com/ascribe/specs/tree/master/coala-ip#coala-ip-remodeling-the-lcc-rrm-with-linked-data).

### Vocabularies

Whenever possible, we try to use the direct [schema.org](schema.org) representation of an entity. If
this is not possible, we define our own classes and properties (see [top-level context](./coala_context.json)
and [definitions](./vocab/coala.jsonld)) with the goal of eventually integrating them as a hosted
extension under schema.org. Self defined properties and classes are built preferring existing
vocabularies, such as [OWL](https://www.w3.org/OWL/) and [DC](http://dublincore.org/).

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
[COALA IP Specification](https://github.com/ascribe/specs/tree/master/coala-ip), we provide the IPLD
schemata in addition to the JSON-LD schemata.

In general, the IPLD schemata are identical to the JSON-LD schemata with the exception of [terms](https://www.w3.org/TR/json-ld/#dfn-term)
that have type `"@id"` (i.e. holding a [node identifier](https://www.w3.org/TR/json-ld/#node-identifiers)):
instead of holding a URI pointing to the referencd object, these terms' values instead hold
[merkle-links](https://github.com/ipld/specs/tree/master/ipld#what-is-a-merkle-link) to the object.

### Immutable Data Considerations

As the [Specification foresees implementations backed by immutable ledgers](link-me) and leverages
IPLD's guarantees of immutable data structures, all entity models have been designed to factor in
their immutability after being created. However, there are still a few recommended design patterns
to use when working with the models:

- Use flatter models and register nested models separately from their parents
- Link nested models and their parents by providing links from the nested models

Adhering to these patterns avoids the creation of models that become too constrained when used in an
immutable context. For example, consider the [`Creation`](#rrm-creation) entity: although the schema
allows a Work to declare its manifestations through the [workExample property](http://schema.org/workExample),
doing so would inherently lock the Work into only these Manifestations. You can avoid this problem
by declaring your Works and Manifestations separately, using the [exampleOfWork property](http://schema.org/exampleOfWork)
in the Manifestations to create a link to its Work. Another feature of this pattern is that you only
have to declare a nested model once, even if multiple parents should be linked to it.

When adhering to these patterns (more specifically, relying on inter-object links rather than nested
objects), you may find that some properties from schema.org expect an object or value rather than a
link. To handle this, you can either use an expanded term with an `"@id"` key:

```javascript
// For example, an Organization:
{
    "@context": "http://schema.org/",
    "@type": "Organization",
    "founder": {
        "@id": "<URI pointing to a Person object>",
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
            "founder": {
                "@id": "founder",
                "@type": "@id"
            }
        }
    ],
    "@type": "Organization",
    "founder": "<URI pointing to a Person object>",
    ...
}
```

For all schemata defined below, our [top-level context](./coala_context.json) has already overriden
any necessary properties to have `"@type": "@id"`. You can always override these again if you prefer
to use a nested object.

### Cryptographically Signed Entities

Although left optional for now, in the future we expect to make the signing of all COALA IP models
mandatory. This can either be done by including properties based on the [Web of Trust RDF ontology](http://xmlns.com/wot/0.1/),
or by using the properties made available by the Web Payments Community Group in their [Linked Data
Signatures schema](https://web-payments.org/specs/source/ld-signatures/).

## Entities

### RRM Party

`Party`s are represented simply by their corresponding schema.org vocabulary, [Person](http://schema.org/Person)
and [Organization](http://schema.org/Organization).

**Note**: `Party`s are still a work-in-progress (see the list of requirements in the
[spec](https://github.com/ascribe/specs/tree/master/coala-ip#the-lcc-party-entity)) and will be
modified in the future. We will likely be adding properties or even define a new class to better
represent them in the future.

##### Person

See the [schema.org/Person definition](http://schema.org/Person) for the Linked Data context.

An example of a Person:

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

##### Organization

See the [schema.org/Organization definition](http://schema.org/Organization) for the Linked Data
context.

An example of an Organization:

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

An example of a localizable Place:

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
`CreationMode`s (`lcc:Manifestation` or `lcc:Work`), we treat any CreativeWork that does not contain
an [`exampleOfWork`](http://schema.org/exampleOfWork) property as a Work and all other CreativeWorks
(or subtypes) as Manifestations. Manifestations that include a [`url`](http://schema.org/url)
property are considered digital Manifestations while all others are physical Manifestations. See the
[schema.org/CreativeWork definition](http://schema.org/CreativeWork) for the Linked Data context.

To conveniently identify Manifestations, we also suggest adding a `isManifestation` property
(*although this is not mandatory*):

```javascript
{
    "@id": "<coalaip placeholder>/isManifestation",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "schema:CreativeWork"
    },
    "schema:rangeIncludes": {
        "@id": "schema:Boolean"
    },
    ...
}
```

An example of a Creation, and its physical and digital Manifestations:

```javascript
// Note: We assume that the data will be put on an immutable ledger, forcing all links to point "backwards."
//       E.g. The Work is registered first and then the Manifestations are linked back to the Work.

// In JSON-LD
// Creation
{
    "@context": [
        "http://schema.org/",
        "<coalaip placeholder>"
    ],
    "@type": "CreativeWork",
    "@id": "<URI pointing to this object>",
    "name": "Lord of the Rings",
    "author": "<URI pointing to a Person or Organization object>"
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
    "exampleOfWork": "<URI pointing to a CreativeWork object>",
    "isManifestation": true,
    "isPartOf" "<URI pointing to a CreativeWork object>",
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
    "exampleOfWork": "<URI pointing to a CreativeWork object>",
    "isManifestation": true,
    "isPartOf" "<URI pointing to a CreativeWork object>",
    "datePublished": "29-07-1954",
    "locationCreated": "<URI pointing to a Place object>"
}

// In IPLD
// Creation
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "name": "Lord of the Rings",
    "author": { "/": "<hash pointing to a Person or Organization object>" }
}

// Digital Manifestation
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "Book",
    "name": "The Fellowship of the Ring",
    "exampleOfWork": { "/": "<hash pointing to a CreativeWork object>" },
    "isManifestation": true,
    "isPartOf" { "/": "<hash pointing to a CreativeWork object>" },
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
    "exampleOfWork": { "/": "<hash pointing to a CreativeWork object>" },
    "isManifestation": true,
    "isPartOf" { "/": "<hash pointing to a CreativeWork object>" },
    "datePublished": "29-07-1954",
    "locationCreated": { "/": "<hash pointing to a Place object>" },
    "url": { "/": "<hash pointing to a file on e.g. IPFS>" }
}
```

##### Licensing

Although CreativeWorks can hold licensing information through a [`license`](http://schema.org/license)
property, we ignore this property for the purposes of rights management as the RRM models rights
through a separate `Right` entity. Rather than using this property to register rights on a
CreativeWork, we instead recommend implementors to follow the [COALA IP Specification's suggested
flow for attaching rights](https://github.com/ascribe/specs/tree/master/coala-ip#the-notion-of-ownership).
This recommendation comes especially strong if `Creation`s will be registered on an immutable
ledger.

##### Fingerprinting

Fingerprinting information can be separately registered for `Creation`s by using our own vocabulary
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

An example of adding fingerprinting information for a digital Manifestation:

```javascript
// In JSON-LD
{
    "@context": "<coalaip placeholder>",
    "@type": "DigitalFingerprint",
    "@id": "<URI pointing to this object>",
    "fingerprintOf": "<URI pointing to a CreativeWork object or media blob>",
    "fingerprint": "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to COALA IP's context>" }, // For now, these could also be a URI to the context
    "@type": "DigitalFingerprint",
    "fingerprintOf": "<hash pointing to a CreativeWork object or media blob>",
    "fingerprint": "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY"
}
```

### RRM Right

As no existing schema.org vocabulary fits the RRM's notion of a `Right`, we define our own class
with the following vocabulary:

```javascript
// Right Class
{
    "@id": "<coalaip placeholder>/Right",
    "@type": "rdfs:Class",
    "rdfs:subClassOf": {
        "@id": "schema:Intangible"
    },
    ...
}

// Creation Property
{
    "@id": "<coalaip placeholder>/creation",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
    "schema:rangeIncludes": {
        "@id": "schema:CreativeWork"
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

// Territory Property
{
    "@id": "<coalaip placeholder>/territory",
    "@type": "rdf:Property",
    "schema:domainIncludes": {
        "@id": "coala:Right"
    },
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

- Although the range of `creation` includes any CreativeWork, we expect that, for the most
  part, only Manifestations will be provided.
- `percentageShares` must be a number between 0 and 100.
- `rightContext` and `usageType` contain arbitrary strings that should be classified by the
  registrar of the Right

An example of a Right:

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        <coalaip placeholder>
    ],
    "@type": "<coalaip placeholder>/Right",
    "@id": "<URI pointing to this object>",
    "usageType": ["all", "copy", "play"],
    "territory": "<URI pointing to a Place object>",
    "rightContext": ["inflight", "inpublic", "commercialuse"],
    "exclusive": true,
    "numberOfUses": 10,
    "percentageShares": 30,
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01",
    "creation": "<URI pointing to a CreativeWork object (that should be a Manifestation)>",
    "license": "<URI pointing to a CreativeWork object or file (ideally on an immutable ledger)>" // Uses schema.org/license property
}

// In IPLD
{
    "@context": [ // For now, these could also be a URI to the context
        { "/": "<hash pointing to schema.org's context>" },
        { "/": "<hash pointing to COALA IP's context>" }
    ],
    "@type": "<coalaip placeholder>/Right",
    "usageType": ["all", "copy", "play"],
    "territory": { "/": "<hash pointing to a Place object>" },
    "rightContext": ["inflight", "inpublic", "commercialuse"],
    "exclusive": true,
    "numberOfUses": 10,
    "percentageShares": 30,
    "validFrom": "2016-01-01",
    "validThrough": "2016-02-01",
    "creation": { "/": "<hash pointing to a CreativeWork object (that should be a Manifestation)>" },
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

An example of an RightsAssignment payload:

```javascript
// In JSON-LD
{
    "@context": <coalaip placeholder>,
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
    "@context": { "/": "<hash pointing to COALA IP's context>" }, // For now, these could also be a URI to the context
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
  be added with expanded property containing `"@value"`.
- In the context of transferring rights through a ledger, no `object`, `agent`, or `participant` is
  required to be specified as they are already implicitly defined in the containing transaction.

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

```javascript
// In JSON-LD
{
    "@context": [
        "http://schema.org/",
        <coalaip placeholder>
    ],
    "@type": "ReviewAction",
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

Note that, in the event the assertion is false, the `error` property is left to the asserting party
to explain the reason the subject is invalid (e.g. the `author` property is incorrect).

### RRM RightsConflict

TBD
