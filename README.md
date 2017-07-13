# COALA IP Spec / Whitepaper

## TL;DR

[COALA IP](https://www.coalaip.org) is a blockchain-ready, community-driven protocol for
intellectual property licensing.

[This presentation](presentations/COALA%20IP%20-%20short.pdf) gives a quick summary. [Here's](presentations/COALA%20IP%20-%20long.pdf)
an extended version.

[This academic paper](https://github.com/COALAIP/specs/blob/master/presentations/COALA%20IP%20Report%20-%20May%202016.pdf)
is about how blockchains can support, complement, or supplement IP, authored by the COALA IP working
group.

## Resources

Links to reference code implementations can be found [here](IMPLEMENTATIONS.md).

COALA IP is a community effort. [Here's](CONTRIBUTING.MD) how to contribute to this doc, or to the
code. Main contributors are listed at the bottom; thanks everyone! Don't be shy -- please give us
your questions, suggestions or feedback :).

What follows here is the whitepaper, including full-on specifications. Here we go...

## Abstract

Content creators on the internet are getting a raw deal. They get a fraction of the revenue earned
by hosting and distribution platforms, even though their work is what keeps these sites filled with
traffic-driving content. It's hard for a creative to make a living. Licensing is hard: the user
experience is bad, with lawyers and middlemen extracting the most value. In many areas, more money
goes to the distributors than to the creators. Even though many consumers would be happy to pay the
people who made the content they love, they aren't given the chance–instead, they are surveilled and
served ads. It doesn't have to be this way.

The [Coalition of Automated Legal Applications — Intellectual Property](http://coala.global/working-groups/)
(COALA IP) group was formed to address these problems. COALA IP's goal is to establish free, open,
and easy-to-use methods of recording attribution and related metadata about works, assigning or
licensing rights, mediating disputes, and authenticating claims by others. We believe there should
be a global standard at the data level, without the need for centralized control.

This document details an approach of representing intellectual property claims on distributed
ledgers (or blockchains). It is an effort to transform the [Linked Content Coalition](http://www.linkedcontentcoalition.org/)
(LCC)'s implementation-agnostic [Rights Reference Model](http://www.linkedcontentcoalition.org/phocadownload/framework/The%20LCC%20Rights%20Reference%20Model%20v1.0.pdf)
(RRM) into a free and open standard by outlining technologies that could be leveraged for an
implementation. This document aims to represent the interests of all involved stakeholders:
creators, rightsholders, distributors, consumers, developers, and so on.

COALA IP's vision will be realized through three key efforts:

1. Authoring a guide to provide an overview of the field and the need for a technical specification
   to represent intellectual property rights on distributed ledgers (see [Introduction](#introduction));
1. Defining a technology-specific, but ledger-agnostic, free and open messaging and communication
   protocol for intellectual property rights and licensing transactions (see [Implementing the
   RRM](#coala-ip-implementing-the-lcc-rrm-with-linked-data)); and
1. Building a community to define a minimally-viable set of data for the description of intellectual
   property rights and licensing agreements.


## Introduction

This section describes the technological concepts used in this document to model a generic and
extensible protocol for managing digital rights. Each write-up is meant only as a brief overview; to
gain a more comprehensive understanding, we encourage you to explore the reference materials
provided as embedded links or in the *Sources* subsections.


### The LCC Framework

The [LCC Framework](http://www.linkedcontentcoalition.org/index.php/rights-data-network/lcc-framework)
is a set of documents published by the [Linked Content Coalition](http://linkedcontentcoalition.org/)
(LCC) to "unify digital rights data management." The key documents include:

- [The LCC Manifesto and Ten Targets for the Rights Data Network](#the-lccs-ten-targets)
- [The LCC Entity Model](#the-lcc-entity-model)
- [The LCC Rights Reference Model](#the-lcc-rights-reference-model)
- [The LCC Principles of Identification](#the-lcc-principles-of-identification)


#### The LCC's Ten Targets

The LCC's foremost goals are to enable, for all parties, the widest possible access to accurate
rights information, as well as the automation of rights licensing and assignment for both
commercial and free use. The LCC released the "[LCC Ten Targets for the Rights Data Network](http://doi.org/10.1000/290)"
as a general guide toward achieving these goals. It asks for the following:

1. Every Party has a unique global identifier;
1. Every Creation has a unique global identifier;
1. Every Right has a unique global identifier;
1. All identifiers are associated with a [URI](https://www.w3.org/Addressing/URL/uri-spec.html) that
   will persistently resolve them within the internet;
1. Links between identifiers are platform agnostic and non-proprietary
1. Metadata is platform agnostic or interoperable; mappings should be available to translate
   between schemata authorized by multiple parties;
1. The provenance of rights has to be made explicit;
1. Any participant has the ability to make standardized, machine-readable statements about
   rightsholdings in Creations;
1. Conflicts in rights declarations should be automatically identifiable; and
1. Registered Creations are associated with corresponding digital "fingerprints" or "watermarks".


For more in-depth information about the goals of the LCC, see the "[LCC Ten Targets for the Rights
Data Network](http://doi.org/10.1000/290)".


**Sources:**

- [LCC: Manifesto and Ten Targets for the Rights Data Network](http://doi.org/10.1000/290), May 2016


#### The LCC Entity Model

*Note: You don't need to know the LCC Entity Model to understand the rest of this document. The
Entity Model is a meta-model used by the LCC to model their higher-level [Rights Reference Model](#the-lcc-rights-reference-model).*

The [LCC Entity Model](http://doi.org/10.1000/285) (LCC EM) specification defines an `Entity` model
composed of five attribute types:

- **Category:** A broad category the Entity belongs to (e.g. `Language=iso3166-1a2:EN ("English")`)
- **Descriptor:** The name of the Entity (e.g. `Name="Andy Warhol"`)
- **Quantity:** A numeric value related to the Entity (e.g. `Height=20cm`)
- **Time:** A time or date related to the Entity (e.g. `DateOfCreation=1999`)
- **Link:** A link to another Entity (e.g. `"Andy Warhol"` ––isCreator––> `"32 Campbell's Soup
  Cans"`)


These attributes are each represented as sub-models in the specification and, together with
unidirectional links, make up the actual `Entity` model. An entity can be linked to
other entities bidirectionally, as the attached figure shows:


![](media/entity_model.png)


The attributes of the `Entity` model are designed to be generic to allow for more complex data
models (e.g. the [LCC RRM](#the-lcc-rights-reference-model)) to be built on top.


**Sources:**

- [LCC: Entity Model](http://doi.org/10.1000/285), May 2016


#### The LCC Rights Reference Model

The [LCC Rights Reference Model](http://doi.org/10.1000/284) (LCC RRM) is a formal framework of
representing intellectual property rights. The RRM describes a high-level data model built on top
of the [LCC Entity Model](#the-lcc-entity-model), composed of the following entity types:

- **Party:** A person or an organization (e.g. "Richard Prince", "American Apparel", or "Sky
  Ferreira")
- **Creation:** Something created by a Party (e.g. "Untitled Instagram Portrait")
- **Place:** A virtual or physical location (e.g. "New York City" or "http://www.newyorkcity.com")
- **Right:** A set of permissions that entitle a Party to do something with a Creation (e.g.
  production and sale of t-shirts bearing the Creation)
- **RightsAssignment:** A decision by a Party resulting in the existence of a Right (e.g. "Richard
  Prince grants American Apparel the right to produce and sell t-shirts bearing Untitled Instagram
  Portrait in North America")
- **Assertion:** A claim made about the substance of a Right (e.g. "Richard Prince claims he has
  copyright to Untitled Instagram Portrait", or "Sky Ferreira claims she has copyright to Untitled
  Instagram Portrait")
- **RightsConflict:** A statement of disagreement over a Right (e.g. "Sky Ferreira and Richard
  Prince both claim copyright to Untitled Instagram Portrait")


*Note: For the sake of simplicity, the `Context` type has been left out. It is defined by the RRM
only as a parent/categorizing class of Right, RightsAgreement, Assertion, and RightsConflict and
holds no significant value on its own.*

These seven entity types are the building blocks of a global digital rights ontology. They can be
linked to each other through specific, unidirectional relationships. The figure below specifies the
total possible relationships between `Entities`:


![](media/rights_reference_model.png)


**Sources:**

- [LCC: Rights Reference Model](http://doi.org/10.1000/284), May 2016


#### The LCC Principles of Identification

- TODO:
    - Summarize briefly (as done in the other sections) what the document talks about, without going
      into too much detail


**Sources:**

- [LCC: Principles of identification](http://doi.org/10.1000/287), May 2016


### The Semantic Web

The world wide web (web) is an information space for sharing information through linked documents.
The web is mostly used by humans, with information published and accessed in human-readable formats
(e.g. a webpage). Although machines are, in theory, capable of understanding this kind of
information, in practice, this is usually difficult and inefficient. For example, while humans may
easily understand a webpage with a table listing national populations, a machine would likely not
understand or be able to deduce new information–even the appropriate context was given by naming a
column as "Country Population".

To solve this, the semantic web introduces methods of publishing information in formats that are
capable of holding semantic meaning for both humans and machines. This allows humans to publish
human-readable information in a way that is also understandable to machines. The key underlying
building block of these formats is the [Resource Description Framework](#the-resource-description-framework-rdf).


**Sources:**

- A. Granzotto (2009): Exploiting spatio–temporal linked data to improve backlinks retrieval


#### The Resource Description Framework (RDF)

The [Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/) is a framework for
describing ontologies. It uses the [Universal Resource Identifier](https://tools.ietf.org/html/rfc3986)
(URI), a generalization of the [Universal Resource Location](https://tools.ietf.org/html/rfc1738)
(URL), to address resources. This allows RDF to be exceptionally interoperable and extensible.

RDF's core data structure is a graph-based model that uses sets of triplets to construct graph
subsets. Each triplet consists of a **subject**, **predicate**, and **object**. In its smallest
form, an RDF graph can be just a single triplet. Visualized, it would look like this:


![](media/rdfgraph.png)


Each node would usually be expressed as an URI to a resource that provides further details about the
node's relationship to the graph.

The following example was used in the [Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/)
(ccREL) W3C submission:


![](media/rdfexamplegraph.png)

```
<rdf:RDF xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
        xmlns:xhtml="http://www.w3.org/1999/xhtml/vocab#">
    <rdf:Description rdf:about="http://www.lessig.org/blog/">
        <xhtml:license rdf:resource="http://creativecommons.org/licenses/by/3.0/" />
    </rdf:Description>
</rdf:RDF>
```


This example triplet states licensing information about Lawrence Lessig's blog in a machine-readable
way. Lessig's blog (the *subject*) is licensed (the *predicate*) under a [Creative Commons
Attribution 3.0](https://creativecommons.org/licenses/by/3.0/) license (the *object*). Provided by
Creative Commons, ccREL is just one example of a context-providing schema that is built on top of
RDF. Others, such as the [Open Digital Rights Language](https://www.w3.org/ns/odrl/2/) (ODRL), also
exist.

RDF itself includes multiple implementations, each varying in their underlying data structures.
Embedding RDF in HTML pages and RDF/XML syntax are two of the more popular implementations, albeit
with relatively heavy syntaxes and learning curves. In 2014, alongside the release of RDF 1.1, a new
RDF-compatible, [Javascript Object Notation](https://tools.ietf.org/html/rfc7159) (JSON)-based data
structure was accepted by the W3C: [JSON-LD](#json-linked-data). JSON-LD fit the semantic web's
[Linked Data](#linked-data) concept into the widely popular JSON format and provided a much more
approachable format to work with RDF.


**Sources:**

- [Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/), May 2016
- [W3C: Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/), May 2016


#### Linked Data

- TODO:
    - A brief section describing the concept of Linked Data
    - Suggested Resources:
        - https://www.w3.org/standards/semanticweb/data


#### JSON Linked Data

[JSON-Linked Data](https://www.w3.org/TR/json-ld/) (JSON-LD) is a data structure that merges the
concepts of [linked](#linked-data) and interoperable data into the [JSON](https://tools.ietf.org/html/rfc7159)
format with [RDF](#the-resource-description-framework-rdf) support. In the context of RDF, JSON-LD
allows users to link a JSON object's properties to a corresponding RDF schema through the concept of
a `context`.

Assume we have the following set of data, modelling a user:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```


For a human, it's obvious this is about a person named Andy Warhol, born August 6, 1928. However,
for a machine, which lacks the intuition and *context* of a human, resolving this representation
into the same conclusion is difficult.

JSON-LD solves this problem by introducing `context` to JSON documents. On a high level, this allows
data to be linked to already defined schemata. Adding a special `@context` key to the document
provides a reference to the schema of the underlying data. Transforming our previous example to use
JSON-LD:


```javascript
{
    "@context": "http://schema.org/Person",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```


Upon seeing this data, a JSON-LD parser could use the `@context` property and send a `GET` to
`http://schema.org/Person` to receive the defined schema and perform validation. If another
application developer were to handle this data, they could rely on the same schema definition
rather than their own. Over time, as more and more services use JSON-LD, data representations across
services would begin to unify to improve cross-service data interoperability.

Take, for example, our previous user model. Right now, each application or service might use their
own model definitions: one site could choose `birthday`, while another uses `day_of_birth` or
`birthDay`, to represent the user's birthday. These models might also be of different formats: some
could be in `YYYY-MM-DD` while others `DD-MM-YYYY`. Despite all of them containing the same semantic
meaning, custom logic would have to be written to not only handle the mapping between the different
keys, but also to convert their values to a standard format.

JSON-LD's `context` solves these problems by allowing for:

1. A unified mapping of keys that comply to base schemata; and
1. Value validation for [primitive data types](https://www.w3.org/TR/rdf11-concepts/#section-Datatypes).


To see how `context` achieves this, we need to explain how JSON-LD magically maps our example's
self-defined keys (`givenName`, `familyName` and `birthDate`) to their matching properties on the
`Person` schema. If you look at the [`Person` definition](http://schema.org/Person), you'll notice
that we didn't choose random keys–they were already part of the schema definition. Because of this,
JSON-LD parsers are able to automatically map and validate our example model's properties against
their schema definitions.

For more clarity, let's see how a JSON-LD parser would look at our example:

1. Notice `@context` contains `http://schema.org/Person`
1. `GET http://schema.org/Person` and add it to `@context` as part of the schema
1. For each of the model's keys, check if they map to any keys provided in the resolved `@context`
    1. For each matched key (a ["term"](https://www.w3.org/TR/json-ld/#dfn-term)), traverse the
       `@context` until a definition is found (a ["term definition"](https://www.w3.org/TR/json-ld/#the-context)),
       usually as a leaf node in the `@context`
    1. "Expand" the data, replacing keys' names with their URI definitions


Continuing with our example, this is the result after expansion:


```javascript
{
    "http://schema.org/givenName": [
        {
            "@value": "Andy"
        }
    ],
    "http://schema.org/familyName": [
        {
            "@value": "Warhol"
        }
    ],
    "http://schema.org/birthDay": [
        {
            "@value": "1928-08-06"
        }
    ],
}
```


The JSON-LD parser notices that the model contains keys matching the `Person` schema, and uses
`http://schema.org/Person` to replace these matches with an URI to their schema definition. The
result, termed as ["Expanded Document Form"](https://www.w3.org/TR/json-ld/#expanded-document-form),
is now a set of data that has been automatically mapped to the given schemata. This form can be
considered the canonical version of the data: anyone can now take this form and understand its
properties, regardless of the key names used in the original model. Moreover, as leaf `@value` nodes
are only allowed to define the most basic types (e.g. string, boolean, integer, etc), this expanded
form also enables a parser to easily traverse the document and validate each occurence of `@value`.

As the rest of this document relies heavily on JSON-LD, we encourage you to learn more by
reviewing the **Sources** below.

**Sources:**

- [W3C Recommendation: JSON Linked Data 1.0](https://www.w3.org/TR/json-ld/), May 2016
- [Wikipedia: JSON-LD](https://en.wikipedia.org/w/index.php?title=JSON-LD&oldid=715712992), May 2016
- [Codeship: JSON-LD: Building Meaningful Data APIs](http://blog.codeship.com/json-ld-building-meaningful-data-apis/),
  May 2016


#### Schema.org

- TODOs in this section:
    - Just describing schema.org is too narrow here. This section should be about linked
      data on the world wide web in general. schema.org is just a regular player when it comes to
      linked data and RDF. There are even search engines that users can lookup schemas
      (http://wiki.dbpedia.org/). Obviously mention schema.org as a preferred source though.


Schema.org is a collaborative initiative with the mission to create, maintain and promote schemata
for structured data on the internet. Its vocabulary is defined as an ontology, connecting different
concepts using links. It can be used with different encodings, including RDFa, Microdata, and
*JSON-LD*.


##### Available Schemas

Schema.org includes the following schemata that are closely related to LCC RRM's `Entity` types.
Potentially, these could be used later to help define the COALA IP specification:

- [schema.org/Person](http://schema.org/Person): See LCC RRM `Party`
- [schema.org/Organization](http://schema.org/Organization): See LCC RRM `Party` (A `Person` can be
  a member of an `Organization`)
- [schema.org/CreativeWork](http://schema.org/CreativeWork): See LCC RRM `Creation`
    - [schema.org/Article](http://schema.org/Article)
    - [schema.org/Blog](http://schema.org/Blog)
    - [schema.org/Book](http://schema.org/Book)
    - [schema.org/Clip](http://schema.org/Clip)
    - [schema.org/Dataset](http://schema.org/Dataset)
    - [schema.org/Game](http://schema.org/Game)
    - [schema.org/MediaObject](http://schema.org/MediaObject)
        - [schema.org/AudioObject](http://schema.org/AudioObject)
        - [schema.org/ImageObject](http://schema.org/ImageObject)
        - [schema.org/VideoObject](http://schema.org/VideoObject)
    - [schema.org/Movie](http://schema.org/Movie)
    - [schema.org/MusicComposition](http://schema.org/MusicComposition)
    - [schema.org/Painting](http://schema.org/Painting)
    - [schema.org/Photograph](http://schema.org/Photograph)
    - [schema.org/SoftwareApplication](http://schema.org/SoftwareApplication)
    - [schema.org/Thesis](http://schema.org/Thesis)
    - [schema.org/VisualArtwork](http://schema.org/VisualArtwork)
- [schema.org/Action](http://schema.org/Action)
    - [schema.org/AssessAction](http://schema.org/AssessAction): See LCC RRM `Assertion`
        - [schema.org/ReviewAction](http://schema.org/ReviewAction)
        - [schema.org/ReactAction](http://schema.org/ReactAction)
            - [schema.org/AgreeAction](http://schema.org/AgreeAction)
            - [schema.org/DisagreeAction](http://schema.org/DisagreeAction)
    - [schema.org/TradeAction](http://schema.org/TradeAction)
        - [schema.org/BuyAction](http://schema.org/BuyAction)
        - [schema.org/SellAction](http://schema.org/SellAction)
        - [schema.org/RentAction](http://schema.org/RentAction)
    - [schema.org/TransferAction](http://schema.org/TransferAction)
- [schema.org/Place](http://schema.org/Place): See LCC RRM `Place`

-*A full list of all core schema.org schemata can be found [here](https://schema.org/docs/full.html).*


In summary:

- **What schema.org helps us with:**
    - **LCC Party:** [schema.org/Organization](http://schema.org/Organization) and
      [schema.org/Person](http://schema.org/Person)
    - **LCC Creation:** [schema.org/CreativeWork](http://schema.org/CreativeWork) and all its
      subschemata could be used
    - **LCC Place:** [schema.org/Place](http://schema.org/Place)
    - **LCC Assertion:** [schema.org/AssessAction](http://schema.org/AssessAction)
- **What schema.org *doesn't* help us with (yet?):**
    - **LCC Right**
    - **LCC RightsAssignment**
    - **LCC RightsConflict**


*A full list of all core schema.org schemata can be found [here](https://schema.org/docs/full.html).*


##### Extensibility of schema.org

Although some of the `Entity` types do not yet exist in schema.org (specifically Rights,
RightsAssignment and RightsConflict), the schema.org schemata are easily extensible. We can
create our own schemata to fit the needs of LCC. Schema.org [even encourages](http://schema.org/docs/extension.html)
others to subclass their *core* schemata into what it calls "*hosted*" and "*external*" extensions.
There are three basic types of schemata on schema.org:

- **Core:** A basic vocabulary for describing the kind of entities most common web applications
  need.
- **Hosted:** Subclassed models from Core which have their own namespace on schema.org (e.g.
  http://health-lifesci.schema.org/) and are reviewed by the schema.org community. Hosted schemata
  should be application-agnostic.
- **External:** Subclassed models from Core or Hosted which have an application-specific namespace (e.g.
  http://schema.coala.global). External schemata may be application-specific.


Applied to the context of the COALA IP specification, application-agnostic schemata (including all
of LCC RRM) would ideally become a *hosted* extension. Application-specific schemata, data models
that are specific for a specific application or service, would become *external* schemata.
Fortunately, leveraging schema.org in this way maintains compliance with rules
five and six of the LCC's "Ten Targets":

- Rule 5: Links between identifiers are system agnostic and need to be authorized by participating
  consortiums.
- Rule 6: Metadata is system agnostic and its schema has to be authorized by participating parties
  or consortiums.


**Sources:**

- [Schema.org](http://schema.org), May 2016
- [Schema.org: Full Hierarchy](http://schema.org/docs/full.html), May 2016
- [Schema.org: Schema.org Extension](http://schema.org/docs/extension.html), May 2016


### Interplanetary Linked Data

This section describes the functionality of [Interplanetary Linked Data](https://github.com/ipld/specs/tree/master/ipld)
(IPLD) and its use when working with immutable data stores and [Linked Data](#linked-data). IPLD is
an attempt to put Linked Data on distributed ledgers by using hashes as content-addressed links, a
technique referred to as "Merkle Links." Merkle links provide a number of interesting properties,
foremost of which is the ability to cryptographically check the data referred to by a link.

#### Motivation for IPLD

If we go back to our Andy Warhol example:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```

Let's add a set of data describing one of his works:


```javascript
{
    "name":"32 Campbell's Soup Cans",
    "dateCreated": "01-01-1962",
    "exampleOfWork": "https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpg"
}
```

Note that neither object contains a link to the other. There is no way for someone to tell that Andy
Warhol is the creator of "32 Campbell's Soup Cans" from just the data alone. We could solve this by
using [JSON-LD](#json-linked-data): we could make both of the objects resolvable on the internet,
add `@id`s to the objects' bodies, and add an `author` property to the creation object that points
to the person object's location.

However, this result runs into the problem of implicitly trusting the hosts that make these objects
resolvable. Hosts might return the correct objects at first, but that could change. Even worse,
resolving actors have no way of checking the integrity of an object they're requesting; a host could
return arbitrary, or even wrong, data that will stay undetected by the resolver. IPLD solves these
problems by using a hash-based, content-addressed linking format.


#### IPLD by Example

For these examples, we use [py-ipld](https://github.com/bigchaindb/py-ipld), an existing Python
implementation of IPLD, to handle IPLD specifics and data transformations. Other implementations,
such as [js-ipld](https://github.com/ipld/js-ipld) also exist.


##### Creation of Linked Objects

We can use IPLD to link the person and creation objects discussed earlier with the following steps:

1. Serialize the person object to its canonical [Concise Binary Object Representation](http://cbor.io/)
   (CBOR) form:


    ```python
    In [1]: import ipld

    In [2]: person = {
    ...:     "givenName": "Andy",
    ...:     "familyName": "Warhol",
    ...:     "birthDate": "1928-08-06"
    ...: }

    In [3]: serialized_person = ipld.marshal(person)
    Out[3]: b'\xa3ibirthDatej1928-08-06jfamilyNamefWarholigivenNamedAndy'
    ```

    `ipld.marshal` serializes the `person` object to a CBOR byte array, using the [CBOR reference
    implementation](https://bitbucket.org/bodhisnarkva/cbor).


1. Hash the serialized byte array using [multihash](https://github.com/jbenet/multihash), encoding
   the hash to base58:


    ```python
    In [4]: ipld.multihash(serialized_person)
    Out[4]: 'QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisC'
    ```


1. Now that we've derived an IPLD hash from the person object, we can use it to define an author for
   the creation:


    ```python
    In [5]: creation = {
        "name":"32 Campbell's Soup Cans",
        "dateCreated": "01-01-1962",
        "exampleOfWork": "https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpg",
        "author": { "/": "QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisC" }
    }
    ```


    We've now connected the creation to its author by using a person's hash value for the `author`
    property, creating our first "Merkle Link." Generally, merkle links can be schematized as:


    ```javascript
    Property = {
        ...
        [<String>]: <MerkleLink>
    }

    MerkleLink = {
        "/": <String: multihash value>
    }
    ```


1. Finally, to obtain a resolvable hash for the creation, we repeat the first two steps. First
   serialize the creation object its canonical CBOR form:


    ```python
    In [6]: serialized_creation = ipld.marshal(creation)
    Out[6]: b"\xa4fauthor\xd9\x01\x02x.QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisCkdateCreatedj01-01-1962mexampleOfWorkx]https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpgdnamew32 Campbell's Soup Cans"
    ```


    And then hash the resulting serialized byte array using multihash and a base58 encoding:


    ```python
    In [7]: ipld.multihash(serialized_creation)
    Out[7]: 'QmfMLNLyJZgvSPkNMvsJspRby2oqP6hWZ8Nd2PvKLhudmK'
    ```


    *Note: The creation's CBOR form replaced the original merkle link contained in `author` with an
    [unassigned CBOR tag (258)](https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml) to make
    the link more easily retrievable on deserialization.*


##### Retrieval of Linked Objects

To further explore IPLD, let's assume we've put these objects into a data store and try to retrieve
them. We'll use [IPFS](https://ipfs.io/) for the data store as its identifiers are compatible with
our previously created hashes.

We can use paths of merkle links ("Merkle Paths") to resolve any object within IPFS from their hash
value, as well as further de-reference any nested merkle links in the dereferenced object. Given the
example above, the `author` of the creation could be found through this merkle path:


```python
In [8]: ipld.resolve('/ipfs/QmfMLNLyJZgvSPkNMvsJspRby2oqP6hWZ8Nd2PvKLhudmK/author')
Out [8]:
{"givenName": "Andy",
 "familyName": "Warhol",
 "birthDate": "1928-08-06"}
```


IPLD resolves any merkle link, in this case the creation's author, to the actual object before any
further dereferences are made, allowing the creation and traversal of merkle links to feel similar
to Unix paths or accessing properties in nested objects. To link across network addresses, we can
use [multiaddr](https://github.com/jbenet/multiaddr) to construct resource paths across protocols.
Such links would allow an IPLD object to maintain resolvable links even if those links point to
separate ledgers (e.g. [IPFS](https://ipfs.io/), [BigchainDB](https://www.bigchaindb.com/),
[Ethereum](https://www.ethereum.org/), [Bitcoin](https://bitcoin.org/en/), etc.).


#### Evaluation of IPLD

In summary, IPLD looks to be a promising new data format suited for our needs, albeit with a few
cavets:

- **Benefits:**
    - Provides cryptographic integrity checks of data using upgradable hash functions (multihash);
    - Uses content-addressed storage instead of location addressed storage (merkle links vs. [URLs](https://tools.ietf.org/html/rfc1738));
    - Enables cross-ledger/database links ([multiaddr](https://github.com/jbenet/multiaddr) and
      merkle paths);
    - Unifies object identifiers through a canonicalized hashing strategy;
    - Imposes immutability through the underlying [merkle-dag](https://github.com/ipld/specs/tree/master/ipld#what-is-a-merkle-graph-or-a-merkle-dag)
      data structure;
    - Future-proofs underlying concepts ([multi-x](https://github.com/multiformats));
    - Enables wide compatibility, even down to the UNIX file system path; and
    - Deserializes to a multitude of other data serialization formats (YAML, JSON, etc.).
- **Caveats:**
    - Non-standardized protocols ([multi-x](https://github.com/multiformats));
        - [Overlaps](https://interledger.org/five-bells-condition/spec.html#crypto-conditions-type-registry)
          with other protocols that are being standardized
        - Breaks with existing and well-established protocols (e.g. [URI](https://tools.ietf.org/html/rfc3986)
          vs. [multiaddr](https://github.com/jbenet/multiaddr))
    - Does not comply with most existing Linked Data ontologies due to immutability constraints; and
    - Uses an opinionated [CBOR](http://cbor.io/) serialization strategy.


#### Compatibility of IPLD and JSON-LD

Although the naming and concept of IPLD was inspired by [JSON-LD](#json-linked-data), the two have
different sets of functionality. In particular, while the two can be used together, IPLD imposes a
number of limitations on JSON-LD's feature set.


##### Self-identifying JSON-LD Objects

With an `@id`, JSON-LD objects are able to maintain a self-identifying link and directly express
their resolvable location to users. However, the same is impossible for IPLD objects: as IPLD
objects are designed to be retrieved only by the canonical hash of their data, this hash cannot be
included as part of the pre-hashed data (trying to do so would amount to solving a cryptographic
puzzle). To avoid this, we use an [empty `@id`](https://www.w3.org/TR/json-ld/#base-iri) on IPLD
objects to resolve these objects to their current document bases (i.e. their resolvable,
content-addressed location on IPFS, etc.).


**Sources:**

- [IPLD Specification Draft](https://github.com/ipld/specs/tree/master/ipld), June 2016
- [IPLD Python Reference Implementation](https://github.com/bigchaindb/py-ipld), June 2016
- [Multihash Specification](https://github.com/jbenet/multihash), June 2016
- [Multiaddr Specification](https://github.com/jbenet/multiaddr), June 2016
- [Concise Binary Object Representation](http://cbor.io/), June 2016
- [IANA: CBOR Tags Registry](https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml), June 2016


### The Web of Trust

- TODO: Explain briefly
- Point to initiatives going on
- https://github.com/WebOfTrustInfo/rebooting-the-web-of-trust
- http://xmlns.com/wot/0.1/
- http://www.weboftrust.info/


### Fingerprinting

Determining the originality and provenance of a creation is challenging. This is true of physical
creations, but even more so for digital creations which face the challenges of being perfectly
copyable as well as easily modifiable. Although computers are good at determining perfect copies,
they struggle if subtle modifications, such as compressing image quality or cropping an image, are
made–even if a human would have no difficulties in making a connection.

The LCC takes these problems into account. In their ["Ten Targets" document](#the-lccs-ten-targets),
they propose cross-standard identifiers that can, if needed, be *transformed* into alternative
identifiers. This section discusses a similar idea: the existence of an arbitrarily complex graph
that can be used to link all the alternative identifiers of a single work to a single identifier on
a global rights registry.

Any function that takes a digital asset as an input and yields a fixed-length value could
potentially be used as a **fingerprinting function**. This could be as simple as a hash function
that inspects the arrangement of bytes in a digital asset and returns a integer, but there are more
elaborate versions:

- [Image-match](https://github.com/ascribe/image-match): An approximate image match algorithm
  implemented in Python;
- [pHash](http://www.phash.org/): A hashing method using various features of a digital asset;
- [dejavu](https://github.com/worldveil/dejavu): An audio fingerprinting and recognition algorithm
  implemented in Python;
- And many more


While a manifestation of a digital creation may initially only have a single fingerprint generated
by an arbitrary hashing function, more elaborate fingerprinting schemes could later be used to help
automatically identify other occurrences of the creation on the internet. Paired with Linked Data,
fingerprinting schemes would allow an arbitrarily complex graph to store and track all the
information related to the use of a work: copies, remixes, mash-ups, and modified versions could all
be identified automatically as paths in the graph. A traversal up a path would reveal the original
instance of the work and possibly identify the creator as well as an opportunity for compensation.

Based on this, as rights information becomes more transparent and rights easily licensable by users,
participants in the system would be incentivized to create more elaborate fingerprinting systems to
further increase transparency.


### The Interledger Protocol

- TODO:
    - This section should briefly explain what Interledger and the Interledger Protocol is about and
      how COALA IP could potentially use it.


## COALA IP: Remodeling the LCC RRM with Linked Data

In this section we describe how the LCC RRM can be modeled using JSON-LD, IPLD, and schema.org.
We will go over each model description given in the LCC Rights Reference
Model document and discuss how the respective model can be translated into Linked Data.


### What Linked Data Gives Us Out of the Box

As a building block of RRM, the LCC first defines a generic, linkable Entity Model whose entities
can be combined to create an extendable data model for intellectual property. However, by
using an RDF-based data structure, we can skip the transformation of these basic entities as RDF
already provides us with a base data structure for linking entities.


### General Approach

The section describes how to get from a LCC RRM model to a RDF-compatible JSON-LD/IPLD
model. As discussed, the "[LCC: Entity Model](http://doi.org/10.1000/285)" defines a generic model
as a base for the Rights Reference Model. The document describes how to implement a fully
extensible data model using a multitude of linked entities. Using an RDF-based data structure
means that defining a base data structure for linking entities is not necessary—this is what RDF
is all about.

To redefine the LCC's Rights Reference Model, we propose the following:

- Identify RDF schemata that map to entities defined in the LCC RRM specification.
    - If appropriate RDF schemata are not available:
        - Compose new RDF types from multiple RDF schemata; and
        - Define new RDF schemata.
- Define how entities are identified and resolved.
- Resolve mismatches between the LCC RRM terminology and RDF schemata.


For the purposes of demonstration, we put any new schemata into `http://coalaip.schema/` and assume
that this document also contains all schema.org definitions (so we don't have to provide
`http://schema.org/` as an additional context).

A slight speed bump in the transformation process is ensuring support for links between entities.
The RRM defines the existance of links in a generic manner, as one-to-many (i.e. `0 - n`)
links. RDF and Linked Data require these links to be explicitly named so as to express specific
facts within their ontologies. For example: schema.org's schemata often include a finite
set of links that can be mapped to the RRM's links, but cannot directly support the possibly
infinite number of links required by the RRM. However, we can overcome this limitation
by extending the base JSON-LD schemata, or its underlying RDF implementation. Extensions
could be hosted by schema.org as a *hosted* extension, or by others as *extended* extensions.


### The LCC Place `Entity`

In the LCC RRM, a Place describes a localizable or virtual place. It contains the following
property:

- **PlaceType:** Defines the type of a Place; is one of:
    - `lcc:LocalizablePlace`: A Place in the physical universe that can be located by spatial
      coordinates
    - `lcc:VirtualPlace`: A non-localizable Place at which a resource may be located at


In addition, a Place can have the following outgoing links to other entities:

- Links to other Places (`0 - n`; one-to-many): *RelatedPlace*


Visualized, an RRM Place looks like:


![](media/rrmplace.png)


#### Proposed Transformation

Compared to schema.org's definition of a Place, the LCC RRM Place can describe both a physical and
a virtual Place. In this specification, we need to separate the two concepts explicitly to avoid
confusion later in the transformation process. Neither a URI nor a IPLD Merkle link is able to
represent a physical location, which is why in the context of this specification, these will be
links pointing to resources, while the LCC Place model will be used
only to refer to a physical place.

For reference:

- **LLC RRM Place or Place** will be used to describe a localizable Place, meaning a physical Place
  in the universe that can be described using spatial coordinates.
- **Universal Resource Identifier** or **IPLD Merkle link** will be used to describe a virtual
  place where a resource can be found.


With schema.org's Place, the transformation of a *localizable* Place to RDF is straight forward
(example adapted from schema.org):


```javascript
// In JSON-LD
{
    "@context": "http://schema.org/",
    "@type": "Place",
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    },
    "name": "Empire State Building"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to schema.org's context>" },
    "@type": "Place",
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    },
    "name": "Empire State Building"
}
```

To support links to other Places, one can use either of the two already-defined properties on a
schema.org Place: `containsPlace` or `containedInPlace`, or extend the schema with their own
properties.


### The LCC Party `Entity`

The LCC recommends that a Party should be able to represent any of the following classes of parties:

- Rightsholders;
- Licensors;
- Administrators;
- Users; or
- Any other participants related to rights.


RRM Parties must have the following properties:

- **PartyType:** Defines if the Party is an individual or a group of individuals;
- **DateOfBirth:** Party's date of birth; only if `PartyType == 'lcc:Individual'`; and
- **DateOfDeath:** Party's date of death; only if `PartyType == 'lcc:Individual'`.


Additionally, a Party can have the following outgoing links to other entities:

- Links to other Parties (`0 - n`; one-to-many): *RelatedParty*
- Links to Places (`0 - n`; one-to-many): *RelatedPlace*


Visualized, an RRM Party looks like:


![](media/rrmparty.png)


#### Proposed Transformation

*Note: We will describe the transformation of a RRM Party into a JSON-LD/IPLD Person and
Organization very literally, so as to provide reasoning for the steps taken in the
transformation. This will only be the case for this `Entity`, as the rationale for transforming
later `Entity` types will be similar.*

Schema.org makes both a [schema.org/Person](http://schema.org/Person) and an [schema.org/Organization](http://schema.org/Organization)
available; hence, there is no need to define either concept as a single model differentiated by
`PartyType`. To keep the transformation of the `Entity` into an RDF schema simple, let us first
transform a RRM Party with `PartyType == 'lcc:Individual'` and then apply the learnings to an RRM
Party with `PartyType == 'lcc:Organization'`.


##### Transformation of RRM Party to an RDF Person

Using the minimum number of properties described in the RRM, an RRM Party with `PartyType ==
'lcc:Individual'` could look like this as a schema.org Person:


```javascript
// In JSON-LD
{
    "@context": {
        "DateOfBirth": "http://schema.org/birthDate",
        "DateOfDeath": "http://schema.org/deathDate"
    },
    "@type": "http://schema.org/Person",
    "DateOfBirth": "1928-08-06",
    "DateOfDeath": "1987-02-22"
}

// In IPLD
{
    "@context": {
        "DateOfBirth": { "/": "<hash pointing to RDF-Schema of birthDate>" },
        "DateOfDeath": { "/": "<hash pointing to RDF-Schema of deathDate>" }
    },
    "@type": { "/": "<hash pointing to RDF-Schema of Person>" },
    "DateOfBirth": "1928-08-06",
    "DateOfDeath": "1987-02-22"
}
```


While there's nothing technically wrong with the above, you may notice on a close inspection of
schema.org/Person that the schema already contains the `birthDate` and `deathDate` properties.
Rather than reinventing the wheel and remapping `DayOfBirth` and `DayOfDeath` to these properties,
we can remove the aliasing and use the properties directly defined on schema.org/Person. This gets
us:


```javascript
// In JSON-LD
{
    "@context": "http://schema.org/",
    "@type": "Person",
    "@id": "https://en.wikipedia.org/wiki/Andy_Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to schema.org's context>" },
    "@type": "Person",
    "@id": "https://en.wikipedia.org/wiki/Andy_Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


In the example, we've used Andy Warhol's Wikipedia page as his Party identifier (`@id`). As an `@id`
value is only required to be a resolvable URI or IPLD merkle-link, a JSON-LD parser would validate
this without complaining; however, `@id` would ideally point to the location of the data itself to
show the JSON-LD parser where it could be resolved within the internet. Unfortunately, Wikipedia
doesn't support this, so that `https://en.wikipedia.org/wiki/Andy_Warhol` doesn't return the required
data, which is why we'll have to look for another solution.

To start off with, lets look at some limitations and requirements derived from the RRM and JSON-LD /
IPLD:


- **LCC's Ten Targets:**
    - A Party's identifier should be linked to the [International Standard Name Identifier](http://www.iso.org/iso/catalogue_detail?csnumber=44292)
      (ISNI) hub.
    - A Party's identifier should have an URI representation, so that it can be resolved predictably
      and persistently within the Internet.
- **LCC's Principles of identification:**
    - A Party should have at least one persistent unique public identifier that is both human- and
      machine-readable.
    - If a Party has multiple public identifiers, there should be a way to automatically
      transform one identifier to another.
    - A Party's identifier can have multiple designations (e.g. ISBN-10, ISBN-13, ISBN-A, etc.).
    - A Party's identifier should have an URI representation.
    - A Party's identifier should not have any intended meaning that could be misinterpreted by
      humans.
    - A Party's identifier should not include any information about the Party itself or its
      registration date.
    - **TODO: There are even more requirements in this document that should be listed here!**
- **JSON-LD:**
    - An `@id` value must be represented as an absolute or relative
      [Internationalized Resource Identifier](https://tools.ietf.org/html/rfc3987) (IRI).
- **IPLD:**
    - Any object must be addressable using its [multihashed](https://github.com/jbenet/multihash)
      value (encoded in base58).
        - Multihash allows different hash functions to interoperate and stay upgradeable.
- And finally, our own requirements to allow for any linked entity data to be put on public ledgers
  (e.g. blockchains or registries):
    - Elements of the Party's identifier can represent the public part of an asymmetric cryptographic
      key-pair.
        - If so, the public key should be represented by a unified encoding method. See
        [Bitcoin's public key addressing](https://en.bitcoin.it/wiki/Technical_background_of_version_1_Bitcoin_addresses).
    - A Party can only be created when at least one valid cryptographic key-pair is provided.

Currently there is no system that is able to fulfill all of these requirements
and become a registry for RRM Party data. Let's pretend, for the sake of completeness, that we have
access to such an identity service in the following examples—preferably a decentralized
not-for-profit service! It will let users:

- Issue an identity that can be resolved using JSON-LD ([Content Negotiation](https://www.w3.org/Protocols/rfc2616/rfc2616-sec12.html))
  or IPLD; and
- Attach the public part of their key pairs to their identity.


Services that could be extended to support our use case include:

- https://pgp.mit.edu/
- https://keybase.io/
- https://ipfs.io/
- https://ipdb.foundation/


Equipped with this identity service, we can go back to the example's JSON-LD representation and
replace its `@id` value with an URI pointing to the dataset (the dataset itself living on the
identity service):


```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "Identity",
    "@id": "<URI pointing to this object>",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


On IPLD, we remove the `@id` property. Not only are we restricted from using self-referencing
links in IPLD, but such links are unnecessary, as any object is able to
identify itself by its own hash. Thus, we get:


```javascript
// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Identity",
    "@id": "",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


Finally, to complete the transformation, we need to include support for the possible outgoing links
of an RRM Party: links to other Parties (*RelatedParty*) and links to Places (*RelatedPlace*). To
give some context, a few potential use cases for these links include:

- Multiple Parties sharing a relationship (e.g. Party A and Party B created Creation C);
- Parties providing Places as part of their metadata (e.g. home location, contact place, or billing
  address); or
- Multiple Parties being bundled together as an Organization.


A few linking possibilities are already covered by schema.org, such as a Person's home address
(schema.org/Person's `homeLocation`; specifying a Place) or parents (schema.org/Person's `parent`;
specifying a Party). If we wanted to define relations that schema.org hadn't already provided, we
could also extend schema.org/Person with our own RDF schema.


##### Transformation of RRM Party to an RDF Organization

An RRM Party with `PartyType == lcc:Organization` describes a single entity representing a group of
individuals. Using the minimum number of properties listed in the RRM, the `Entity` type could look
like this as an schema.org Organization.


```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "Organization",
    "@id": "<URI pointing to this object>",
    "name": "World Wide Web Consortium",
    "founder": {
        "@type": "Identity",
        "@id": "<URI pointing to the founder Party>"
    },
    "member": [
        {
            "@type": "Identity",
            "@id": "<URI pointing to a member Party"
        },
        {
            "@type": "Identity",
            "@id": "<URI pointing to a member Party"
        },
        {
            "@type": "Identity",
            "@id": "<URI pointing to a member Party"
        }
    ]
}

// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Organization",
    "@id": "",
    "name": "World Wide Web Consortium",
    "founder": {
        "@type": "Identity",
        "@id": { "/": "<hash pointing to the founder Party" }
    },
    "member": [
        {
            "@type": "Identity",
            "@id": { "/": "<hash pointing to a member Party" }
        },
        {
            "@type": "Identity",
            "@id": { "/": "<hash pointing to a member Party" }
        },
        {
            "@type": "Identity",
            "@id": { "/": "<hash pointing to a member Party" }
        }
    ]
}
```

- TODO: This needs a lot of speccing out. How can members of an organization collectively sign
  something they're submitting? Is there a single public key address assigned to an organization or
  does the organization just bundle members that act like they were in an organization but act
  independently?


##### Allowing Parties to Sign Metadata

As we envision future identity registries built on top of public ledgers, we need to ensure
that users are able to include a cryptographic identity with any registered identities, allowing
them to sign any submitted metadata. Luckily, the [Friend of a Friend Project](http://www.foaf-project.org/)
has already thought about this and provide the [Web of Trust RDF ontology](http://xmlns.com/wot/0.1/)
for signing RDF documents with public-key cryptography. Integrating this ontology into our identity
model, we could get something like:

- TODO:
    - Give an code example how WOT could look like in an immutable ledger.
    - Make sure that the *immutability* is not violated, the WOT ontology as of now only work with
      mutability.


### The LCC Creation `Entity`

An RRM Creation model describes something directly or indirectly made by human beings. According
to the specification, it has a single required property:

- **CreationMode:** Describes the mode of creation; one of:
    - `lcc:Manifestation`: A perceivable manifestation of a `Work`; or
    - `lcc:Work`: A distinct, abstract Creation whose existence is revealed through one or more
      manifestations.


Additionally, a Creation can have the following outgoing links to other entities:

- Links to other Creations (`0 - n`; one-to-many): *RelatedCreation*
- Links to Places (`0 - n`; one-to-many): *RelatedPlace*
- Links to Parties (`0 - n`; one-to-many): *RelatedParty*


Visualized, an RRM Creation looks like:


![](media/rrmcreation.png)


#### Proposed Transformation

Schema.org's existing schemata already covers a large number of the RRM Creation's use cases.
Not only is the vocabulary of [schema.org/CreativeWork](http://schema.org/CreativeWork)
quite extensive, there are also a number of subtypes that can be used to define specifics about a
creation (e.g. [schema.org/Book](http://schema.org/Book)). However, one distinction to highlight is
how an RRM Creation has two `CreationMode`s: one for perceivable Creations (called `Manifestation`s)
and one for abstract Creations (called `Work`s). Note that in the rest of the text, we use
`Creation` to refer to the entity type which encompasses both `Work`s and `Manifestation`s.

Transforming to JSON-LD, we get:


```javascript
// A Work and its Manifestations in JSON-LD
// Note: We assume that the data will be put on an immutable ledger and so all links must point
//       "backwards"
{
    "@context": "http://coalaip.schema/",
    "@graph": [
        {
            "@id": "#creation",
            "@type": "Work",
            "name": "Lord of the Rings",
            "author": "<URI pointing to the author Party>"
        },
        {
            "@id": "#digitalManifestation",
            "@type": "Manifestation",
            "name": "The Fellowship of the Ring",
            "manifestationOfWork": "#creation",
            "digitalWork": "<URI pointing to file>",
            "fingerprints": [
                "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY",
                "<multihash/fingerprint value>"
            ],
            "locationCreated": "<URI pointing to a Place>"
        },
        {
            "@id": "#physicalManifestation",
            "@type": "Manifestation",
            "name": "The Fellowship of the Ring",
            "manifestationOfWork": "#creation",
            "datePublished": "29-07-1954",
            "locationCreated": "<URI pointing to a Place>"
        }
    ]
}
```


A similar result can be achieved for IPLD, although split into multiple different schemata
linked with hashes:


```javascript
// A Work object in IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Work",
    "name": "Lord of the Rings",
    "author": { "/": "<hash pointing to the author Party>" }
}

// A digital Manifestation of the Work in IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Manifestation",
    "name": "The Fellowship of the Ring",
    "manifestationOfWork": { "/": "<hash pointing to the Work>" },
    "digitalWork": { "/": "<hash pointing to a file on e.g. IPFS>" },
    "fingerprints": [
        "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY",
        "<multihash/fingerprint value>"
    ],
    "locationCreated": { "/": "<hash pointing to a Place>" }
}

// A physical Manifestation of the Work in IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Manifestation",
    "name": "The Fellowship of the Ring",
    "manifestationOfWork": { "/": "<hash pointing to the Work>" },
    "datePublished": "29-07-1954",
    "locationCreated": { "/": "<hash pointing to a Place>" }
}
```


Note that a distinction has been made between works (typed as `Work`s)
and manifestations (typed as `Manifestation`s). Both physical and digital manifestations can be
represented, with digital manifestations including a set of fingerprints as well as a link pointing
to an example of the work. In the future, we plan to subtype both a `Work` and a
`Manifestation` type from schema.org's `CreativeWork` to allow their properties to be easily
customizable.


### The LCC Right `Entity`

In comparison to all other RRM `Entity` types, the Right is by far the most interconnected. A
minimal set of required properties include:

- **RightType:** Defines the type of Right (e.g. all uses, license, copy, play, stream,
  administration, lcc:RightSet, etc)
- **ToolType:** Defines the type of medium that must be employed when exercising the Right (e.g.
  only watch on mobile phone or only use a brush to produce manifestations). `ToolType`s are not
  consumed as part of exercising the Right.
- **MaterialType:** Defines the type of material that may be employed when exercising the right
  (e.g. only use watercolour paint to produce manifestations). `MaterialType`s are consumed as part
  of exercising the Right and become part of the result.
- **ValidContextType:** Defines the type of context in which the Right may be exercised (e.g. in
  flight, public, commercial use, academic research, etc.).
- **IsExclusive:** Indicates whether the Right is exclusive to the Rightsholder (e.g. `true` or
  `false).
- **PercentageShare:** Defines the percentage share of the Right controlled (e.g. 51%, 100%, etc.).
- **NumberOfUses:** Defines the number of uses permitted by the Right (e.g. 3, 5, unlimited uses,
  etc.).
- **ValidPeriod:** Defines the period during which the Right is valid. (e.g. 2015-2016).
- **Territory:** Defines the Place where the Right is may be exercised (e.g. North America).


In addition, a Right can have the following outgoing links to other entities:

- Links to other Rights (`0 - n`; one-to-many): *RelatedRight*.
- Links to Parties (`0 - n`; one-to-many): *RelatedParty*.
- Links to Creations (`0 - n`; one-to-many): *RelatedCreations*.
- Links to Places (`0 - n`; one-to-many): *RelatedPlace*.
- Links to RightAssignments (`0 - n`; one-to-many): *RelatedContext*.
- Links to Assertions (`0 - n`; one-to-many): *RelatedContext*.
- Links to RightsConflicts (`0 - n`; one-to-many): *RelatedContext*.


Visualized, an RRM Right looks like:


![](media/rrmright.png)


##### Additional Types of Rights

The RRM specifies three special types of Rights intended for specific use
cases:

- **SourceRight:** A Right from which another Right is derived
- **SuperSededRight:** A Right to invalidate a referenced Right
- **RightSet:** A collection of Rights bundled as a single Right


We utilize `lcc:SourceRight`s in our notion of [`Copyright`s](#copyright-semantics) as well as our
`Right` transformation, but, for now, we leave the other types out of the specification:

- `lcc:SupersededRight`: Although representable, reversible rights complicate the ownership logic of
  an immutable ledger; and
- `lcc:RightSet`: In the context of putting `Right`s onto a global distributed ledger, bundling
  multiple rights together presents a specific problem: most decentralized ledgers cannot guarantee,
  and especially synchronize, the concurrent transfer of multiple assets (in the future, this may be
  possible with cryptoconditions).


#### Proposed Transformation

Transforming the RRM Right `Entity` poses some challenges. According to the RRM specification, an RRM
Right can:

- Represent both copyright as well as licensing information; and
- Be a SourceRight, SuperSeededRight or RightSet.


For the purposes of storing `Right`s on decentralized ledgers, we ignore the requirements of the
`lcc:RightSet` and model `Right`s as atomically transferrable containers of licensing information.
To make a distinction between derived licensing information and a full copyright, we separately
[explore the semantics of copyright later](#copyright-semantics). To the best of our knowledge,
there are no existing RDF schemata for creating such containers, so we propose the following to
satisfy the consolidated requirements of:

- [LCC: Rights Reference Model](http://doi.org/10.1000/284);
- [W3C: Open Digital Rights Language](https://www.w3.org/TR/odrl/); and
- [W3C: Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/).


```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "Right",
    "@id": "<URI pointing to this object>",
    "usages": "all|copy|play|stream|...",
    "territory": "<URI pointing to a Place>",
    "context": "inflight|inpublic|commercialuse...",
    "exclusive": true|false,
    "numberOfUses": "1, 2, 3, ...",
    "share": "1, 2, 3, ..., 100",
    "validFrom": {
        "@type": "Date",
        "@value": "2016-01-01"
    },
    "validTo": {
        "@type": "Date",
        "@value": "2017-01-01"
    },
    "source": "<URI pointing to a Copyright>",
    "license": "<URI pointing to a license on an immutable ledger>"
}
```


RRM Rights are just links between `Manifestation`s and licenses. Licenses are usually intended to be
read by humans, so the license should be stored on a technology that prevents changes to the
license. This could be an immutable ledger or blockchain, or be linked by Content-Addressing. With
this in mind, an implementation in IPLD/IPFS is favored:


```javascript
// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Right",
    "usages": "all|copy|play|stream|...",
    "territory": { "/": "<hash pointing to a Place>" },
    "context": "inflight|inpublic|commercialuse...",
    "exclusive": true|false,
    "numberOfUses": "1, 2, 3, ...",
    "share": "1, 2, 3, ..., 100",
    "validFrom": {
        "@type": "Date",
        "@value": "2016-01-01"
    },
    "validTo": {
        "@type": "Date",
        "@value": "2017-01-01"
    },
    "source": { "/": "<hash pointing to a Copyright>" },
    "license": { "/": "<hash pointing to a license>" }
}
```

In our transformation, it is important to highlight that every `Right` must include a `source` (or
equivalent) property that links it to an enabling `Copyright` or parent `Right`. With use on an
immutable ledger in mind, the `source` property implements support for `lcc:SourceRight`s–albeit in
a "backwards" relation in comparison to the RRM's definition–a `Right` containing a `source`
property is the derivation while the pointed-to entity is the `lcc:SourceRight`.

#### Copyright Semantics

Although RRM `Right`s are capable of representing both full copyrights as well as derived licenses
to `Creation`s, we split these two concepts into different entities to better represent them within
distributed ledgers. We base the structure of the `Copyright` entity on the `Right` entity's, but as
only a subset of the `Right`'s properties pertain to `Copyright`s (e.g. "territory", "validFrom",
etc.), we do not require implementations to subtype `Copyright`s from `Right`s. However,
semantically, and for the purposes of discussion, we treat `Copyright`s as a subtype of `Right`s.
Similarly to `Right`s, we propose that `Copyright`s be stored on decentralized ledgers for
maintaining ownership and provenance.

We propose:

```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "Copyright",
    "rightsOf": "<URI pointing to a Creation (usually a Manifestation)>",
    "territory": "<URI pointing to a Place>",
    "validFrom": {
        "@type": "Date",
        "@value": "2016-01-01"
    },
    "validTo": {
        "@type": "Date",
        "@value": "2017-01-01"
    }
}

// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Copyright",
    "rightsOf": { "/": "<hash pointing to a Creation (usually a Manifestation)>" },
    "territory": { "/": "<hash pointing to a Place>" },
    "validFrom": {
        "@type": "Date",
        "@value": "2016-01-01"
    },
    "validTo": {
        "@type": "Date",
        "@value": "2017-01-01"
    }
}
```

For implementations, we recommend that `Copyright`s be automatically registered with their
`Manifestation`s so as to immediately state the `Manifestation`'s copyright holder and allow other
`Right`s to be derived from the `Copyright`. Given that multiple `Copyright`s may be needed, e.g.
for multiple regions, there is no limit to the number of `Copyright`s that can be attached to a
given `Manifestation` (for potential conflicts, see [`Assertion`s](#the-rrm-assertion-entity) and
[`RightsConflict`s](#the-rrm-rightsconflict-entity).

#### The Notion of Ownership

Given that `Right`s and `Copyright`s are designed to be stored on decentralized ledgers, we propose
to link these entities with their related rightsholding `Party`s by cryptographic ownership.
Assuming the ledger natively supports cryptographic ownership of assets, this results in only the
owners of a `Right` or `Copyright` on the ledger to be maintained as the rightsholders. Moreover,
this means that only these owners are able to repurpose the `Right` by, for example, initiating a
[`RightsAssignment`](#the-rrm-rightsassignment) to a another `Party`. Ledgers should be chosen so
that all forms (e.g. transfers, loans, consignments, etc.) of these transactions (i.e. RRM
`RightsAssignment`s) can be stored in an ordered fashion to maintain each right's chain of
provenance.

With RRM `Right`s modelled in such a fashion, any digital creator that wants to register and
distribute the `Right`s of a `Manifestation` to interested `Party`s must:

1. Register their `Party` identifier on a global registry;
1. Register their `Creation` as a `Work` on a global registry and link it to their `Party`
   identifier;
1. Register `Manifestation`s to the `Work` on a global registry;
1. Register a `Copyright` for the `Manifestation` on a global registry;
1. Derive any number of `Right`s tailored to interested `Party`s from the `Copyright` and register
   them on a global registry; and
1. Register `RightAssignment`s to assign these `Right`s to interested `Party`s.


The above steps highlight how a `Right` is not only limited to registration; with the use of a
ledger, `Right`s also contain properties of ownership and can be transferred from one `Party` to
another via `RightsAssignment`s. The owner of a `Right` or `Copyright` on the ledger is maintained
to be the rightsholder.

However, there are a few edge cases to consider when licensing information is stored this way:

- Specific licenses can imply an agreement between the issuer of the `Right` and the commons; to
  handle this intention to grant `Right`s to literally everyone, a special `Party` symbolizing the
  commons could be created to receive and hold such `Right`s. Following the assignment of this
  `Right`, other, arbitrary, transfers of `Right`s of the license to specific `Party`s must be
  disallowed. Finally, `Party`s must also be disallowed from attaching new `Right`s with licenses
  that conflict with the "commons license" to the `Manifestation`.
- TODO: Maybe there are more edge cases like this. If so, enumerate and discuss/propose solutions.


It is important to note that with these ownership semantics for copyrights and licenses, the
ownership of a `Work` or `Manifestation` is essentially meaningless: these entities simply contain
information about a creative work and are used as pointers for `Copyright`s and `Right`s. As such,
storing `Work`s and `Manifestation`s in ledgers is unnecessary and an immutable data store, e.g.
IPFS, can be used instead if cross-protocol links are supported (i.e. multiaddr).


### The LCC RightsAssignment `Entity`

According to the RRM, a `RightsAssignment` describes an event that results in the existence or
non-existence of a `Right` (or `Copyright`). Depending on the type, a `RightsAssignment` may
be linked from an assigning `Party` ("Assigner") to a receiving `Party` ("Assignee"). From the RRM,
a `RightsAssignment` can have the following properties:

- **RightsAssignmentType**: Defines the type of `RightsAssignment`; one of:
    - **RightsLaw:** Represents the creation of a `Right` by law (e.g. the US Copyright Act of 1976);
    - **RightsPolicy:** Represents the assignment of a `Right` from an authorized `Party` to another
      `Party` without requiring the latter's agreement (e.g. security level for user access of a
      computer system); or
    - **RightsAgreement:** Represents an agreement between two Parties regarding a Right (e.g. a
      license, publishing agreement, etc.).
- **RightsAssignmentStatus**: Defines the status of the RightsAssignment. It can be one of:
    - `lcc:Offer`: An open RightsAssignment proposed by a prospective Assigner;
    - `lcc:Request`: An open RightsAssignment proposed by a prospective Assignee; or
    - `lcc:Executed`: An executed assignment of Rights.


The RRM RightsAssignment has the following outgoing references:

- Links to RRM Parties (`0 - n`; one-to-many): *RelatedParty*; and
- Links to RRM Rights (`0 - n`; one-to-many): *RelatedRight*.


Visualized, an RRM RightsAssignment looks like:


![](media/rrmrightsassignment.png)


#### Proposed Transformation

Existing schemata for transferring assets could provide a source for transformations, but because of
our unique requirements we do not use those schemata. We expect RightsAssignments to be registered
on immutable ledgers that can already handle asset transfers. COALA IP has chosen to use both IPLD
and the Interledger protocol in the hopes of establishing a metadata and licensing ontology that can
span multiple ledgers and immutable data stores. We assume the following requirements will be met
by every COALA IP-compatible ledger:

- Assets must only be transferrable if cryptographic key-pair signatures are used on the transaction
  level;
- Transactions must be able to define a JSON-serializable payload;
- Assets' provenance chains must be easily comprehensible by any user;
- The Divisibility of Assets must be defined during registration;
- Transactions must support IPLD as well as ILP's [Crypto-Conditions
  specification](https://interledger.org/five-bells-condition/spec.html);
- Transfer transactions must support different modes, including:
    - Transfers from a group of individuals to a single individual (and vice-versa);
    - Transfers that are only claimable during a certain time span (timelock conditions); and
    - Transfers that are only claimable by an individual or group that knows a certain secret key
      (hashlock conditions);
    - TODO: Are there more modes?
- TODO: Are there more requirements COALA IP asks from a ledger?


With these assumptions, we can model a minimally transformed RRM `RightsAssignment` on top of
[schema.org/TransferAction](http://schema.org/TransferAction) and include it as the payload of a
ledger's transfer-transaction. Piggybacking on a transfer-transaction allows the rights transfer to
automatically be included with information such as the current and new rightsholders, time of
execution, and status of execution (valid or rejected by the ledger).


```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "RightsAssignment",
    "contract": "<URI pointing to a contract on a ledger>"
}
```


and in IPLD:

```javascript
// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "RightsAssignment",
    "contract": { "/": "<hash pointing to a contract>" }
}
```


Although not required, we include the `contract` property in this schema to frame the rights that
are available to be transferred. A Party is not able to transfer more Rights than they have
themselves, so a transfer of Rights can only contain permissions that are a subset of the Rights
contained in the original creation of the Right or previous transfer-transaction.


### The LCC Assertion `Entity`

Assets registered under the COALA IP ontology are registered by independent users, not by a trusted
central authority (like a rightsholder) or a decentralized network (like the Bitcoin network) that
can guarantee the validity of the data. We must assume that some records based on the COALA IP
ontology will contain inaccurate or even fraudulent statements made by users. To counteract this,
the LCC RRM recommends the implementation of an RRM Assertion `Entity` that evaluates claims made
about the truth or falsehood of statements from participating Parties within the ontology.

The LCC's minimum set of required properties includes:

- **TruthValue**: Indicates the level of trust or "truthiness" of the claim; and
- **ValidPeriod**: Defines the time period during which the claim is maintained (e.g from 01.01.2011
  to 01.01.2015)


Additionally, an RRM Assertion can have the following outgoing references:

- Links to Parties (`0 - n`; one-to-many): *Asserter*;
- Links to RightsAssignments (`0 - n`; one-to-many): *SubjectOfAssertion*;
- Links to Assertions (`0 - n`; one-to-many): *SubjectOfAssertion*; and
- Links to RightsConflicts (`0 - n`; one-to-many): *SubjectOfAssertion*.


Visualized, an RRM Assertion `Entity` looks like:


![](media/rrmassertion.png)


#### Proposed Transformation

Since the ontology will potentially be exposed to an open internet and its users, an Assertion model
in the ontology is necessary. It provides a healing mechanism interpreters of the data can use to
retrieve a statistical truth. COALA IP proposes that assertions should not be made on actual models,
but rather on a model's attributes and links.


Think about the following scenario:

> Andy Warhol decides to use the COALA IP protocol to register his work on a blockchain. He's
  registering one of his works called "32 Campbell's Soup Cans" as a Work and attaches a poster
  of the work as a Manifestation of it. He also creates a Right defining the licensing terms of
  buying the poster and attaches it to the Manifestation. Since Andy is not really good with
  computers—they were never really his type of medium—he accidentally registers a Work
  of Edvard Munch's "The Scream" under his name.

Visually, this is what we'd end up with:


![](media/lccrrmassertionexample1.png)


This creates an awkward situation. We've stored our ontology on a blockchain supporting IPLD and
content-addressed storage. In contrast to a traditional SQL database, it is impossible to revert
the transactions. We can only append to a blockchain. The solution is to append an Assertion
validating specific statements that are true.

`Assertion`s are applied towards entire entities and evaluate whether an asserting `Party`
("Asserter") agrees or disagrees with the claim made by the entity. Schema.org's [ReviewAction](http://schema.org/ReviewAction)
provides a good base to work off of, albeit with less-than-ideal property names that don't map well
to the RRM's definitions. We assume that "coala.schema" will alias some of these properties and get:

```javascript
// In JSON-LD
{
    "@context": "http://coalaip.schema/",
    "@type": "Assertion",
    "asserter": "<URI pointing to a Party>",
    "assertionTruth": false,
    "assertionSubject": "<URI pointing to Work: The Scream>",
    "error": "author"
}

// and

{
    "@context": "http://coalaip.schema/",
    "@type": "Assertion",
    "asserter": "<URI pointing to a Party>",
    "assertionTruth": true,
    "assertionSubject": "<URI pointing to Work: 32 Campbell's Soup Cans>"
}

// In IPLD
{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Assertion",
    "asserter": { "/": "<hash pointing to a Party" },
    "assertionTruth": false,
    "assertionSubject": { "/": "<hash pointing to Work: The Scream>" },
    "error": "author"
}

// and

{
    "@context": { "/": "<hash pointing to coalaip.schema's context>" },
    "@type": "Assertion",
    "asserter": { "/": "<hash pointing to a Party" },
    "assertionTruth": true,
    "assertionSubject": { "/": "<hash pointing to Work: 32 Campbell's Soup Cans>" }
}
```

*Note: On IPLD, you have the option of applying additional granularity to the `Assertion` by
directly referring to an entity's property as the "assertionSubject". For example, if you wanted to
assert that "The Scream"'s "author" property is incorrect, you could do so with*

```javascript
{
    ...
    "assertionTruth": "false",
    "assertionSubject": { "/": "/ipdb/<hash of work>/author" },
    "error": "...",
    ...
}
```

*Although this doesn't directly apply the `Assertion` against the entire `Work` entity, we still
have a link to the `Work` in the IPLD hash and can associate this `Assertion` to it.*

We end up with the following:

![](media/lccrrmassertionexample2.png)


As a recommendation, we add that using IPLD with `Assertion`s is ideal, as it enforces the
immutability of an asserted object (as well as the assertion itself); with IPLD, objects cannot be
silently changed after-the-fact as any changes will cause their IPLD hashes to also change.


### The RRM `RightsConflict` Entity

TODO:

- See other introductory sections of LCC entities. Use same structure to describe the entity


#### Proposed Transformation

TODO:

- See other introductory sections of LCC entities. Use same structure to do the transformation


### Generating Verifiable Claims

As the COALA IP ontology may be potentially exposed to the public, users must have a mechanism of
proving their actions to others. To provide this with cryptographic signatures is a two part
challenge: we not only need to ensure that user identities can be associated with cryptographic
identities (see [requirements of `Party`s](#transformation-of-rrm-party-to-an-rdf-person), but also
that any submitted claim can be signed by those cryptographic identities. Although no optimal
solution currently exists for associating identities, a number of pre-existing schemata are
available for signing RDF-compatible data:

- The [Web of Trust RDF ontology](http://xmlns.com/wot/0.1/) by the [Friend of a Friend Project](http://www.foaf-project.org/),
  although this assumes the ontology is mutable; and
- The [Linked Data Signatures schema](https://web-payments.org/specs/source/ld-signatures/) by the
  [Web Payments Community Group](https://www.w3.org/community/webpayments/)

Building on top of these two facets is the [Verifiable Claims Architecture](http://w3c.github.io/webpayments-ig/VCTF/architecture/)
and its associated [data model](http://opencreds.org/specs/source/claims-data-model/) (currently
using the Linked Data Signatures schema) that is being standardized by the [Verifiable Claims Task
Force](http://w3c.github.io/vctf/). In the future, data from COALA IP could be used to generate, or
even be created, in the format proposed.


### User Extensions

TODO:

- Explain how users could extend the given entities with their own properties


## Future

This document has outlined general technologies and guidelines on using the LCC Framework as the
basis of an RDF ontology for managing digital rights with immutable data stores. As the goal is to
implement an open standard for rights management, a number of efforts are to follow:

- Define a production-ready RDF-compatible schema based on the proposed transformations (see
  [`schema/`](schema/))
- Complete a reference implementation using the RDF-compatible schema (see
  [implementations](IMPLEMENTATIONS.md))
- Include or build open source communities around COALA IP
- Identify a standards committee to work with
- Reformat the proposed transformations and their resulting schemata to that of a standard proposal


Thank you for reading!

# Contributors

Contributors to this document, in alphabetical order:

- Tim Daubenschuetz: tim.daubenschuetz@gmail.com, tim@ascribe.io
- Greg McMullen: greg@ipdb.foundation, gmcmullen@gmail.com
- Brett Sun: qisheng.brett.sun@gmail.com, brett@bigchaindb.com
