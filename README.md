# COALA Intellectual Property Guideline


Contributors to this document, in alphabetical order:

- Tim Daubenschuetz: tim.daubenschuetz@gmail.com, tim@ascribe.io
- Greg McMullen: greg@ipdb.foundation, gmcmullen@gmail.com
- Brett Sun: brett@bigchaindb.com


## THIS DOCUMENT IS WAY TOO LONG! I DON'T HAVE TIME FOR ALL OF THIS!

We've got you covered with presentations:
you.

- [COALA IP (extended)](presentations/COALA%20IP%20-%20long.pdf)
- [COALA IP](presentations/COALA%20IP%20-%20short.pdf)

Please be vocal about any questions, suggestions or feedback. Make sure to tell us!

Thank you!


## PLEASE REVIEW THIS SECTION FIRST BEFORE CONTINUING FURTHER

This document is a work in progress. Some sections contain the keyword "TODO" and a description on
what should be written. Feel free to take the time to conduct research in an unfinished area and
write the section yourself.

If your contribution contains information from external documents, please make sure to link to any
appropriate sources.

If your contribution relies on a field that wasn't previously explained, please provide an overview
of the field to give other readers the necessary background.

This document has a soft characters-per-line limit of 100 characters (note that by including links,
only the descriptor counts). Please respect this limit while writing.

As human beings are capable of recognizing patterns easily, make sure to follow the other, hereto
not explicitly mentioned, text formating patterns as close as possible.

As an overall guideline for contributions to this document document:

- Think practical.
- Think in technologies.
- Don't reinvent the wheel. Use what's already out there and give proper attribution.
- Write as if you had to implement next week.


## General TODOs:

- Improve consistency of use of defined terms. Synonyms for technical terms should be replaced
  with the standard term throughout.
- Before releasing this document to the public, write an introductory section explaining what COALA
  is, what its general goals are, why this document matters, what is left to do, and so on...
- Sometimes this document references internally to other sections ("as seen in the above section",
  ...). Using relative links, we should point to the sections we're writing about


## Table of Contents

- TODO:
    - link to the individual headlines in a tree structure


## Abstract

Content creators on the internet are getting a raw deal. They get a fraction of the revenue earned
by hosting and distribution platforms, even though their work is what keeps these sites filled
with traffic-driving content. It's hard for a creative to make a living. Licensing is hard: the
user experience is so bad, lawyers and middlemen extract the most value. In many areas, more money
flows into the pockets of the distributors than to the creators. Even though many consumers would
be happy to pay the people who made the content they love, the users are not given the chance to
pay. Instead, they are surveilled and served ads based on their data profiles.

The Coalition Of Automated Legal Applications — Intellectual Property Group (COALA IP) group was
formed to address these problems. COALA IP is working to design and implement a free and open
specification for representing and licensing intellectual property. COALA IP's goal is to establish
open, free, and easy-to-use ways to record attribution and metadata about works, assign or license
rights, mediate disputes, and authenticate claims by others. The group believes there should be a
global standard at the data level, without the need for centralized control.

This document details COALA IP's approach to representing intellectual property on distributed
ledgers. It's an effort to transform the implementation-agnostic Rights Reference Model (RRM) of
the Linked Content Coalition (LCC) into a free and open standard. It outlines technologies that
could be leveraged for the implementation and structure of a specification for all involved
parties: creators, rightsholders, distributors, consumers, developers, and so on. The protocol
will be technology-opinionated, but ledger-agnostic.

This document is part of COALA IP's effort to develop an approach for the registration and
licensing of intellectual property on distributed ledgers. It's an effort to transform the
implementation-agnostic Rights Reference Model (RRM) of the Linked Content Coalition (LCC) into
a free and open guideline. It outlines technologies that could be leveraged for the implementation
and structure of a specification for all involved parties: creators, rights holders, consumers,
developers, etc. The protocol is to be technology-opinionated, but ledger-agnostic.

The implementation of COALA IP's vision could be distinguished into three major efforts:

1. Building a community to define a minimally-viable set of data for the description of
   intellectual property rights and licensing agreements;
2. Authoring this guide providing an overview of the field and discussing the need for a technical
   specification to represent intellectual property rights on disributed ledgers; and
3. Defining a free and open messaging and communication protocol for intellectual property rights
   and licensing transactions.


## Introduction

This section provides context for the COALA Intellectual Property (COALA IP) specification. It
briefly describes and explains the technological concepts used to model a generic, extensible
protocol for managing digital rights. These explainations are only meant to give the reader a
comprehensive overview of the concepts; to understand them in their full spectrum, we encourage the
reader to further study them on their own. Sources are given for each topic.


### The LCC Framework

The [LCC Framework](http://www.linkedcontentcoalition.org/index.php/rights-data-network/lcc-framework)
is a set of documents published by the [Linked Content Coalition](http://linkedcontentcoalition.org/)
(LCC) to "unify digital rights data management". The key documents are:

- [The LCC Manifesto and Ten Targets for the Rights Data Network](http://doi.org/10.1000/290)
- [The LCC Entity Model](http://doi.org/10.1000/285)
- [The LCC Rights Reference Model](http://doi.org/10.1000/284)
- [The LCC Principles of identification](http://doi.org/10.1000/287)


#### The LCC's Ten Targets

The LCC's goal is to enable the widest possible access to accurate rights information and the
ability to automate the licensing and assignment of rights, whether for commercial or free use.
The LCC released the "[LCC Ten Targets for the Rights Data Network](http://doi.org/10.1000/290),"
as a general guide toward achieving the following goals:

1. Every Party has a unique global identifier;
2. Every Creation has a unique global identifier;
3. Every Right has a unique global identifier;
4. All identifiers have a [URI](https://www.w3.org/Addressing/URL/uri-spec.html) to persistently resolve them within the internet;
5. Links between identifiers are platform agnostic and non-proprietary
6. Metadata is platform agnostic or interoperable, and schema should have authorized mappings to
   translate between schema;
7. The provenance of Rights has to be made explicit;
8. Any participant has the ability to make standardized, machine-readable statements about
   rightholdings in creations;
9. Conflicts in rights declarations should be automatically identifiable; and
10. Registered Creations have links to corresponding digital "fingerprints" or "watermarks".


For more in-depth information about the goals of the LCC, see the "[LCC Ten Targets for the Rights Data Network](http://doi.org/10.1000/290)".


**Sources:**

- [LCC: Manifesto and Ten Targets for the Rights Data Network](http://doi.org/10.1000/290), May 2016


#### The LCC Entity Model

*Note: Knowledge of the LCC Entity Model is not vital to understand this specification. The Entity
Model is a meta-model used by the LCC to model their ontology, the LCC Rights Reference Model.*

The [LCC Entity Model](http://doi.org/10.1000/285) (LCC EM) is a generic data model the LCC uses to
define more specific data models, like the [LCC Rights Reference Model](http://doi.org/10.1000/284)
(LCC RRM). The LCC EM specification defines an `Entity` model composed of five attribute types:

- **Category:** A broad category the Entity belongs to(e.g. Language=iso3166-1a2:EN ("English"))
- **Descriptor:** The name of the Entity (e.g. Name="Andy Warhol")
- **Quantity:** A numeric value related to the Entity (e.g. Height=20cm)
- **Time:** A time or date related to the Entity (e.g. DateOfCreation=1999)
- **Link:** Links the Entity to another Entity (e.g. "Andy Warhol" ---isCreator---> "32 Campbell's
  Soup Cans")


These attributes are each represented as models in the specification and, together with
unidirectional links, make up the actual `Entity` model. An `Entity` can be linked to
other `Entities` bidirectionally, as the attached figure shows:


![](media/f1.png)


The attributes of the `Entity` model are designed to be generic so more complex data models like
the LCC RRM can be built on top.


**Sources:**

- [LCC: Entity Model](http://doi.org/10.1000/285), May 2016


#### The LCC Rights Reference Model

The [LCC Rights Reference Model](http://doi.org/10.1000/284) (LCC RRM) is a formal framework for
the representation of intellectual property rights. The LCC RRM specification describes an abstract
logical data model built on top of the LCC Entity Model, and is composed of the following `Entity`
types:

- **Party:** A person or an organization (e.g. "Richard Prince", "American Apparel", or "Sky
  Ferreira")
- **Creation:** Something created by a Party (e.g. "Untitled Instagram Portrait")
- **Place:** A virtual or physical location (e.g. "New York City" or "http://www.newyorkcity.com")
- **Right:** A set of permissions that entitle a Party to do something with a Creation (e.g.
  production and sale of t-shirts bearing the Creation)
- **RightsAssignment:** A decision by a Party resulting in the existence of a right (e.g. "Richard
  Prince grants American Apparel the right to produce and sell t-shirts bearing Untitled Instagram
  Portrait in North America")
- **Assertion:** A claim made about the substance of a Right (e.g. "Richard Prince says he has copyright in Untitled Instagram Portrait", or "Sky Ferreira says she has copyright in Untitled Instagram Portrait")
- **RightsConflict:** A statement of disagreement over a Right (e.g. "Sky Ferreira and Richard Prince both claim copyright in Untitled Instagram Portrait")


*Note: For the sake of simplicity, the `Context` type was left out of the above list. It is defined only as a parent/categorizing class of Right, RightsAgreement, Assertion, and RightsConflict. It holds no significant value on its own.*

These seven `Entity` types are the building blocks of a global digital rights ontology. They can
be linked to each other through specific, unidirectional relationships. The figure below specifies
the total possible relationships between `Entities`:


![](media/f2.png)


**Sources:**

- [LCC: Rights Reference Model](http://doi.org/10.1000/284), May 2016


#### The LCC Principles of Identification

- TODO:
    - Summarize briefly (as done in the other sections) what the document talks about, without going
      into too much detail


**Sources:**

- [LCC: Principles of identification](http://doi.org/10.1000/287), May 2016


### The Semantic Web

The world wide web (web)is a universal place for sharing information globally. In most cases, this
is done by human beings publishing information in a human-readable format, like a webpage. While
machines are theoretically capable of understanding information of this kind, in practice it is
challenging and inefficient. For example, a HTML page containing tabular data about the population
of a country is easy for a human to understand, but even if the first column is "the name of a
country", it is difficult for a machine to use this information to deduce new information from
another table, and often impossible for a machine to reason about such information if the necessary
context is not provided.

However, if we use data schemata that hold semantic meaning for both humans and machines, humans
would still be able to publish information but in a way that machines could more easily understand.
Doing so would likely prove to be of great value in advancing the web; one such effort is the
creation of the semantic web. One of it's main building blocks, the Resource Description Framework,
is briefly described in the following section.


**Sources:**

- A. Granzotto (2009): Exploiting spatio–temporal linked data to improve backlinks retrieval


#### The Resource Description Framework (RDF)

[Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/) (RDF) is a framework for
describing entities on the web. It uses the Universal Resource Identifier (URI), a generalization
of the Universal Resource Location (URL), to address resources. This makes RDF exceptionally
interoperable and extensible.

RDF's core data structure is a graph-based data model that uses sets of triplets, each consisting
of a **subject**, **predicate**, and an **object**, to construct subsets of the graph. In its
smallest form, a set containing a single triplet is already an RDF graph. Visualized, a simple
RDF graph could look like this:


![](media/rdfgraph.png)


RDF is primarily used on the world wide web. Each node of the graph is usually represented as a
URI pointing to the resource that gives further details about its relationship in the graph.
The following example was used in the [Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/) (ccREL)
W3C submission:


![](media/rdfexample.png)


This example RDF manifestation states licensing information about Lawrence Lessig's blog in a
machine-readable way. Lessig's blog (the *subject*) is licensed (the *predicate*)
under a Creative Commons Attribution 3.0 license (the *object*). ccREL is a schema provided by
Creative Commons that allows defining context for machines on how to interpret licensing information
for documents on the web. ccREL itself is implemented using RDF Schema, which is basically a
vocabulary for describing properties and classes of RDF resources. By using the [Web
Ontology Language](https://www.w3.org/TR/owl-features/) (OWL), concepts from other RDF
schemata can be included in a domain-specific RDF schema, creating a global ontology of semantically
structured data.

ccREL is just one example of a context-providing schema built on top of RDF. Since RDF itself is
just a concept, its implementations vary, especially in terms of used data structures. Embedded
RDF in HTML pages and RDF/XML syntax are two of the more popular implementations of RDF, albeit with
relatively heavy syntax and learning curves. In 2014, with RDF 1.1 coming up, a new RDF-compatible,
JSON-based data structure was accepted by the W3C: JSON-LD. It fits the semantic web concept of
linked data into [Javascript Object Notation](https://tools.ietf.org/html/rfc7159) (JSON), which
makes it much more approachable than comparable implementations. We plan to port the LCC RRM
specification to RDF using JSON-LD, so we will explore its main features in the next section.


**Sources:**

- [Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/), May 2016
- [W3C: Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/), May 2016


#### Linked Data

- TODO:
    - A brief section describing the concept of Linked Data
    - Suggested Resources:
        - https://www.w3.org/standards/semanticweb/data


#### JSON Linked Data

[JSON-Linked Data](https://www.w3.org/TR/json-ld/) (JSON-LD) is a data structure merging the
concepts of the [Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/) with
[JSON](https://tools.ietf.org/html/rfc7159). Using the concept of a `context`, it allows users to
link a JSON object's property to the corresponding RDF schemata in an ontology.

Let's assume we have the following set of data:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```


For a human it's obvious that this is about a person named Andy Warhol who was born on August
6<sup>th</sup>, 1928. However, for a machine that lacks the intuition and *context* of a human,
resolving this representation into the same conclusion is difficult.

JSON-LD solves this problem by introducing `context` into JSON documents. On a high level, this
allows data to be linked to already defined schemata. Adding a special `@context` key to the
document provides a reference to the schema of the underlying data. Transforming our previous
example to use JSON-LD would result in:


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

Right now, each application or service has their own model for representing users. For example, one
site might use `birthday` as the key for the user's birthday, while others use `day_of_birth` or
`birthDay`. They may even use different formats for the value of the key, with some using `YYYY-MM-DD`
and another using `DD-MM-YYYY`. Custom logic would have to be written to handle mapping the
different keys to each other, but also to convert their values to a standard format when in essence
they all have the same semantic meaning. They describe when a user was born.

JSON-LD solves these problems by introducing a `context` to each data model allowing for:

1. A unified mapping of keys by complying to base schemata
1. Value-validation on a [primitive data types level](https://www.w3.org/TR/rdf11-concepts/#section-Datatypes)


Going back to the Andy Warhol example, we need to explain how JSON-LD magically maps our
self-defined keys (`givenName`, `familyName` and `birthDate`) to the properties of schema.org's
`Person`. If you look at schema.org's `Person` definition, you'll see we didn't choose random key
names. Names were already part of the definition. In this case, a JSON-LD parser can automatically
map and execute validation against these properties by using the schema definition.

For more clarity, let's see how a JSON-LD parser would look at this example:

1. Notice `@context` contains `http://schema.org/Person`
2. `GET http://schema.org/Person`
3. For each of the user-defined keys, check if they map to any keys provided in the schema
    1. If this is the case, traverse the schema until a leaf node (the JSON-LD specification calls
       this an `identifying blank node`) is found
    2. "Expand" the data, replacing keys' names with URIs to their more granular schema definitions


Continuing with this example, here is what our previously defined set of data would look like after
expansion:


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


We end up with a much more verbose form of our set of data. In the JSON-LD specification it's called
*expanded* form, as the original object's been expanded with a `@context`. The original object's
form, still with an `@context`, is defined by the specification as *compacted* form.

The JSON-LD parser assumes we've defined the correctly named keys for a `Person` and uses
`http://schema.org/Person` to individually replace each of our properties with their more
detailed schema definition URIs. The result is an automatically mapped set of data that uses an
already available schema. As every key of a given value now points to a left node on a schema
ontology, and as leaf nodes are only allowed to define the most basic types, such as string,
boolean, integer, etc, the parser can now easily traverse the document and validate each occurrence
of `@value`.


##### Final Thoughts

This example is just the tip of the iceberg. JSON-LD has tremendous power. It can be used for
aliasing, self-referencing, built-in types, indexing, and more.

The rest of this document will rely heavily on JSON-LD, so we encourage you to learn more about it
by reviewing the **Sources** section below.

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
RightsAssignment and RightsConflict), the schema.org schemata are easily extensible and we can
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

This section describes the functionality of Interplanetary Linked Data (IPLD) and its use in working
with immutable data stores and Linked Data.

#### Motivation for IPLD

IPLD is an attempt to put Linked Data on distributed ledgers using hashes for linking, a technique
referred to as "Merkle links".

Let's go back to our Warhol example:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```

In addition, we have a set of data describing one of his works:


```javascript
{
    "name":"32 Campbell's Soup Cans",
    "dateCreated": "01-01-1962",
    "exampleOfWork": "https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpg"
}
```

Note that neither object contains a link to the other. There is no way to tell that Andy Warhol
is the creator of "32 Campbell's Soup Cans." We could use JSON-LD to create a link between the
objects by making both of the objects resolvable on the internet. We could do this by adding `@id`s
to the objects' bodies, and an `author` property to the creation that points to a resolvable
location of the person object.

The problem is we have to trust the hosts that make these objects resolvable. Hosts might return
the correct objects at first, but that could change. Since there is no way for resolving actors to
check the integrity of the object they're requesting, a host could return arbitrary data and the
actor wouldn't know. In addition, internal linking within objects or internal linking from URIs
is challenging using linked data protocols like JSON-LD.

The rest of this section explores the features of IPLD that promise to solve these problems.


#### IPLD by Example

The following sections give a brief overview of IPLD's functionality. More information is available
in the [IPLD's specification draft](https://github.com/ipfs/specs/tree/master/ipld).


##### Creation of Linked Objects

We can us IPLD to link the person and creation objects discussed earlier with the following steps:

1. Serialize the person's object to a canonical form of [Concise Binary Object Representation](http://cbor.io/)
   (CBOR):


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

    For this demonstration, we use [py-ipld](https://github.com/bigchaindb/py-ipld), an
    existing Python library, to handle IPLD specifics and data transformations. In this
    case, `ipld.marshal` is used to serialize the `person` object using a [CBOR
    reference implementation](https://bitbucket.org/bodhisnarkva/cbor). The result is a byte array.


1. Hash the serialized byte array using [multihash](https://github.com/jbenet/multihash) and encode
   the hash to base58


    ```python
    In [4]: ipld.multihash(serialized_person)
    Out[4]: 'QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisC'
    ```


    [Multihash](https://github.com/jbenet/multihash) is a protocol for differentiating outputs from
    various well-established cryptographic hash functions. It adds a
    [hexadecimal prefix](https://github.com/jbenet/multihash#table-for-multihash-v100-rc-semver)
    to each hash generated indicate the hash function used. This lets users identify the hash
    functions in the future in case of upgrades, and allows for multiple hash functions to coexist
    within and across applications.


3. Now that we have derived an IPLD from the person object, we can link it to the creation as its
   author:


    ```python
    In [5]: creation = {
        "name":"32 Campbell's Soup Cans",
        "dateCreated": "01-01-1962",
        "exampleOfWork": "https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpg",
        "author": { "/": "QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisC" }
    }
    ```


    We've now connected the creation to a person by using the person's hash value, thereby creating
    a "Merkle link". Generally, Merkle links can be schematized like this:


    ```javascript
    Property = {
        ...
        [<String>]: <MerkleLink>
    }

    MerkleLink = {
        "/": <String: multihash value>
    }
    ```


4. Finally, to make this creation object resolvable, we repeat the first two steps. We serialize the creation
   object to a canonical form of CBOR:


    ```python
    In [6]: serialized_creation = ipld.marshal(creation)
    Out[6]: b"\xa4fauthor\xd9\x01\x02x.QmRinxtytQFizqBbcRfJ3i1ts617W8AA8xt53DsPGTfisCkdateCreatedj01-01-1962mexampleOfWorkx]https://en.wikipedia.org/wiki/Campbell%27s_Soup_Cans#/media/File:Campbells_Soup_Cans_MOMA.jpgdnamew32 Campbell's Soup Cans"
    ```


    Note this is a special case, with the Merkle link contained in `creation` replaced by
    an [unassigned CBOR tag (258)](https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml) to
    make the link more easily retrievable on deserialization.


5. Hash the resulting serialized byte array using [multihash](https://github.com/jbenet/multihash)
   and encode the hash to base58:


    ```python
    In [7]: ipld.multihash(serialized_creation)
    Out[7]: 'QmfMLNLyJZgvSPkNMvsJspRby2oqP6hWZ8Nd2PvKLhudmK'
    ```


##### Retrieval of Linked Objects

To further explore IPLD, let's assume we've put these objects into a data store. We'll use IPFS
since its using compatible identifiers to the hashes we created previously.

We can use paths of Merkle links (Merkle paths) to resolve any object within IPFS using its hash
value, and also further de-reference any of the object's connecting edges by following its Merkle
links. Given the example above, the `author` of the creation could be found through this Merkle
path:


```python
In [8]: ipld.resolve('/ipfs/QmfMLNLyJZgvSPkNMvsJspRby2oqP6hWZ8Nd2PvKLhudmK/author')
Out [8]:
{"givenName": "Andy",
 "familyName": "Warhol",
 "birthDate": "1928-08-06"}
```


IPLD resolves any Merkle link, in this case the creation's author, with the actual object,
making traversing a Merkle link feel similar to regular property access in objects. To link
across network addresses, a format called [multiaddr](https://github.com/jbenet/multiaddr) can be
used. Multiaddr allows the construction of resource paths to reach across protocols. Doing so
would allow an IPLD object to maintain resolvable links even if its Merkle links point to
separate ledgers (e.g. IPFS, BigchainDB, Bitcoin, etc).


#### Evaluation of IPLD

In summary, IPLD is a promising new technology, with a few cavets:

- **Benefits:**
    - Provides cryptographic integrity checks of data using upgradable hash functions (multihash).
    - Content addressed storage instead of location addressed storage (compare to URL)
    - Enables cross-ledger/database resolvability of data (multiaddr and merkle-paths).
    - Unifies object identifiers through a canonicalized hashing strategy.
    - Imposes immutability by using a merkle-dag data structure.
    - Future-proofs underlying concepts (multi-x).
    - Enables wide compatibility, even down to the UNIX file system path.
    - Lightweight protocol drafts and implementations.
    - Deserializes to a multitude of other data serialization formats (YAML, JSON, XML, etc).
- **Caveats:**
    - Non-standardized protocols (multi-x).
        - [Overlaps](https://interledger.org/five-bells-condition/spec.html#crypto-conditions-type-registry)
          with other protocols that are being standardized.
        - Breaks with exisiting and well-established protocols (e.g. URI vs. multiaddr).
    - Does not comply with existing linked data ontologies due to immutability.
    - Opinionated CBOR serialization.


#### Compatibility of IPLD and JSON-LD

Although the naming and concept of IPLD was inspired by JSON-LD, the two have different sets of
functionality. This section highlights the limitations imposed on JSON-LD by IPLD.


#### Self-identifying JSON-LD Objects

JSON-LD objects can maintain a self-identifying link using the `@id` property. This allows the
object to directly express its location. The same is impossible for IPLD objects. Since they are
supposed to only be retrieved by the corresponding hash of the their data, the hash itself cannot be
part of the object as it would be a very difficult to solve cryptographic puzzle.  The amount of
processing required would be incredible, rendering the identification of objects incredibly
inefficient. Instead, we prevent JSON-LD objects with IPLD links from self-identifying themselves
using an `@id` property. This is usually not a problem as objects can also identify themselves
through content addressing.


**Sources:**

- [IPLD Specification Draft](https://github.com/ipfs/specs/tree/master/ipld), June 2016
- [IPLD Python Reference Implementation](https://github.com/bigchaindb/py-ipld), June 2016
- [Multihash Specification](https://github.com/jbenet/multihash), June 2016
- [Multiaddr Specification](https://github.com/jbenet/multiaddr), June 2016
- [Interledger: Crypto-Conditions](https://interledger.org/five-bells-condition/spec.html), June
  2016
- [Concise Binary Object Representation](http://cbor.io/), June 2016
- [IANA: CBOR Tags Registry](https://www.iana.org/assignments/cbor-tags/cbor-tags.xhtml), June 2016


### The Web of Trust

- TODO: Explain briefly
- Point to initiatives going on
- https://github.com/WebOfTrustInfo/rebooting-the-web-of-trust
- http://xmlns.com/wot/0.1/
- http://www.weboftrust.info/


### Fingerprinting

Determining the originality and provenance of a physical object is challenging, but there are
almost always subtle differences between an original physical object and copies. Digital files
can be copied perfectly, down to the bit. Furthermore, it is difficult for a computer to determine
that two files actually represent the same photograph or song if the files are different sizes or
other subtle modifications have been made, while a human would have no difficulty making the
connection.

The LCC takes these problems into account. In their "Ten Targets" document, they propose
cross-standard identifiers that can, if needed, be *transformed* into alternative identifiers. This
section discusses a similar idea: the existence of an arbitrarily complex vector that can be used
to link all the alternative identifiers of a single work to a single identifier on a global
rights registry.

Any function that takes a digital asset as an input and yields a fixed-length value could
potentially be used as a **fingerprinting function**. This could be as simple as a hash function
that inspects the arrangement of bytes in a digital asset and returns a integer, but there are more
elaborate versions:

- [Image-match](https://github.com/ascribe/image-match): An approximate image match algorithm
  implemented in Python.
- [pHash](http://www.phash.org/): A hashing method using various features of a digital asset.
- [dejavu](https://github.com/worldveil/dejavu): An audio fingerprinting and recognition algorithm
  implemented in Python.
- TODO: List more libraries
    - Find popular ones that do fingerprinting for all kinds of media types


While a manifestation of a digital creation may initially only have a single fingerprint generated
by an arbitrary hashing function, more elaborate fingerprinting schemes could later be used to help
automatically identify other occurrences of the creation on the internet. Paired with linked data,
fingerprinting schemes could allow the storage and tracking of all information about the use of a
work in an arbitrarily complex graph. Copies, remixes, mash-ups, and modified versions of the work
could be identified automatically as paths in the graph. Traversing these paths would reveal the
original instance of the work, possibly identifying the creator and providing opportunities for
compensation.

As rights information becomes more transparent and users can easily license the rights they want
to obtain, participants in the system would be incentivized to create more elaborate fingerprinting
systems to further increase transparency.


### The Interledger Protocol

- TODO:
    - This section should briefly explain what Interledger and the Interledger Protocol is about and
      how COALA IP could potentially use it.
    - Same formalities as in all the sections before apply.


## COALA IP: Remodeling the LCC RRM with Linked Data

In this section we describe how the LCC RRM can be modeled using JSON-LD, IPLD, and schema.org.
We will go over each model description given in the LCC Rights Reference
Model document and discuss how the respective model can be translated into Linked Data.


### What Linked Data Gives Us Out of the Box

As a building block of RRM, the LCC first defines a generic, linkable Entity Model whose entities
can be composed together to create an extendable data model for intellectual property. However, by
using an RDF-based data structure, we can skip the transformation of these basic entities as RDF
already provides us with a base data structure for linking entities.


### General Approach

The section describes how to get from a LCC RRM model to a RDF-compatible JSON-LD/IPLD
model. As discussed, the "[LCC: Entity Model](http://doi.org/10.1000/285)" defines a generic model
as a base for the Rights Reference Model. The document describes how to implement a fully
extensible data model using a multitude of linked entities. Using an RDF-based data structure
means that defining a base data structure for linking entities is not necessary—this is what RDF
is all about.

To redefine the LCC's Rights Reference Model, we did the following:

- Identify RDF schemata that map to entities defined in the LCC RRM specification.
    - If appropriate RDF schemata are not available:
        - Compose new RDF types from multiple RDF schemata; and
        - Define new RDF schemata.
- Define how entities are identified and resolved.
- Resolve mismatches between the LCC RRM terminology and RDF schemata.


A slight speed bump in the transformation process is to ensure support for links between entities;
while the RRM defines the existance of links in a generic manner, e.g. as one-to-many (i.e. `0 - n`)
links, RDF and Linked Data require these links to be explicitly named so as to express specific
facts within their ontologies. A case in point is how schema.org's schemata often include a finite
set of links that can be mapped to the RRM's links but can not directly support the possibly
infinite number of such links required by the RRM. However, we can overcome this issue with a bit of
effort by extending the base JSON-LD schemata, or its underlying RDF implementation; such extensions
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


Visualized, the RRM Place looks like:


![](media/lccrrmplace.png)


#### Proposed Transformation

Compared to schema.org's definition of a Place, the LCC RRM Place both describes a physical as well
as a virtual Place. In this specification, we need to separate the two concepts explicitly to avoid
confusion later in the transformation process. Neither a URI nor a IPLD Merkle link is able to
represent a physical location, which is why in the context of this specification, these will be
links pointing to resources, while the LCC Place model will be used
only to refer to a physical place.

For reference:

- **LLC RRM Place or Place** will be used to describe a localizable Place, meaning a Place in the
  universe that can be described using spatial coordinates.
- **Universal Resource Identifier** or **IPLD Merkle link** will be used to describe a virtual
  place where a resource can be found.


With schema.org's Place, the transformation of a *localizable* Place to RDF is straight forward
(example adapted from schema.org):


```javascript
// In JSON-LD
{
    "@type": "http://schema.org/Place",
    "geo": {
        "@type": "http://schema.org/GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    },
    "name": "Empire State Building"
}

// In IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Place>" },
    "geo": {
        "@type": { "/": "<hash pointing to RDF-Schema of GeoCoordinates>" },
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

- Rightsholders
- Licensors
- Administrators
- Users
- Or any other participants related to rights


RRM Parties must have the following properties:

- **PartyType:** Defines if the Party is an individual or a group of individuals
- **DateOfBirth:** Party's date of birth; only if `PartyType == 'lcc:Individual'`
- **DateOfDeath:** Party's date of death; only if `PartyType == 'lcc:Individual'`


Additionally, a Party can have the following outgoing links to other entities:

- Links to other Parties (`0 - n`; one-to-many): *RelatedParty*
- Links to Places (`0 - n`; one-to-many): *RelatedPlace*


Visualized, the RRM Party looks like:


![](media/lccrrmparty.png)


#### Proposed Transformation

*Note: We will describe the transformation of a RRM Party into a JSON-LD/IPLD Person and
Organization very literally, so as to provide reasoning for the individual steps taken in the
transformation. This will only be the case for this `Entity`, as the rationale for transforming
later `Entity` types will be fairly similar.*

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
Rather than reinvent the wheel and remap `DayOfBirth` and `DayOfDeath` to these properties, we can
remove the aliasing and instead, use the properties directly defined on schema.org/Person. This gets
us:


```javascript
// In JSON-LD
{
    "@type": "http://schema.org/Person",
    "@id": "https://en.wikipedia.org/wiki/Andy_Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}

// In IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Person>" },
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
      (ISNI) hub
    - A Party's identifier should have an URI representation, so that it can be resolved predictably
      and persistently within the Internet
- **LCC's Principles of identification:**
    - A Party should have at least one persistent unique public identifier that is both human- and
      machine-readable
    - If a Party has multiple public identifiers, then there should be a way of automatically
      transforming one identifier to another
    - A Party's identifier can have multiple designations (e.g. ISBN-10, ISBN-13, ISBN-A, etc)
    - A Party's identifier should have an URI representation
    - A Party's identifier should not have any intended meaning that could be misinterpreted by
      humans
    - A Party's identifier should not include any information about the Party itself or its
      registration date
    - **TODO: There are even more requirements in this document that should be listed here!**
- **JSON-LD:**
    - An `@id` value must be represented as an absolute or relative
      [Internationalized Resource Identifier](https://tools.ietf.org/html/rfc3987) (IRI)
- **IPLD:**
    - Any object must be addressable using its [multihashed](https://github.com/jbenet/multihash)
      value (encoded in base58)
        - Multihash allows different hash functions to interoperate and stay upgradeable
- And finally, our own requirements to allow for any linked entity data to be put on public ledgers
  (e.g. blockchains or registries):
    - Elements of the Party's identifier can represent the public part of an asymmetric cryptographic
      key-pair
        - If so, the public key should be represented by a unified encoding method; as inspiration,
          see [Bitcoin's public key addressing](https://en.bitcoin.it/wiki/Technical_background_of_version_1_Bitcoin_addresses)
    - A Party can only be created when at least one valid cryptographic key-pair is provided

Although there is no system currently available that is able to fulfill all of these requirements
and become a registry for RRM Party data, let's pretend, for the sake of completeness, that we have
access to such an identity service in the following examples (preferably, in the future, a
decentralized not-for-profit service is chosen). It lets users:

- Issue an identity that can be resolved using JSON-LD ([Content Negotiation](https://www.w3.org/Protocols/rfc2616/rfc2616-sec12.html))
  or IPLD, and
- Attach the public part of their key pairs to their identity.


Notable, currently-existing services that could be extended to support our use case include
(preferably, a decentralized, non-profit service is chosen):

- https://pgp.mit.edu/
- https://keybase.io/
- https://ipfs.io/
- https://ipdb.foundation/


Now equipped with this identity service, we can go back to the example's JSON-LD representation and
replace its `@id` value with an URI pointing to the dataset (the dataset itself living on the
identity service):


```javascript
// In JSON-LD
{
    "@type": "http://coalaip.schema/Identity",
    "@id": "<URI pointing to this object>",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


On IPLD, however, we remove the `@id` property; not only are we restricted from using
self-referencing links in IPLD, but such links are actually unnecessary as any object is able to
identify itself by its own hash. Thus, we get:


```javascript
// In IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Identity>" },
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


Finally, to complete the transformation, we need to include support for the possible outgoing links
of an RRM Party: links to other Parties (*RelatedParty*) and links to Places (*RelatedPlace*). To
give some context, a few potential use cases for these links include:

- Multiple Parties sharing a relationship (e.g. Party A and Party B created Creation C)
- Parties providing Places as part of their metadata (e.g. home location, contact place, or billing
  address)
- Multiple Parties being bundled together as an Organization


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
    "@type": "http://schema.org/Organization",
    "@id": "http://identityservice.com/organizations/w3c",
    "name": "World Wide Web Consortium",
    "founder": {
        "@type": "http://linkedcontentcoalition.com/Identity",
        "@id": "https://identityservice.com/identities/12bS2BTF4j8kkmNqoyQRwzKy76EXQWRVWJ",
    },
    "member": [
        {
            "@type": "http://linkedcontentcoalition.com/Identity",
            "@id": "https://identityservice.com/identities/152xhUAJBGEht9Jwerv1omFV82xaRcyzHH",
        },
        {
            "@type": "http://linkedcontentcoalition.com/Identity",
            "@id": "https://identityservice.com/identities/1Je5tHHkHcs1ioLZmiBFSuyiXdwd76xf4D",
        },
        {
            "@type": "http://linkedcontentcoalition.com/Identity",
            "@id": "https://identityservice.com/identities/1QDtDjNgBx6242VakTTkFMn5HmkibzrsR8",
        }
    ]
}
```

- TODO: Also define how the object would look like in IPLD
- TODO: This needs a lot of specing out. How can members of an organization collectively sign
  something they're submitting? Is there a single public key address assigned to an organization or
does the organization just bundle members that act like they were in an organization but act
independently?


##### Allowing Parties to Sign Metadata

As we envision future identity registries to be built on top of public ledgers, we need to ensure
that users are able to include a cryptographic identity with any registered identities, allowing
them to sign any submitted metadata. Luckily, the [Friend of a Friend Project](http://www.foaf-project.org/)
has already thought about this and provide the [Web of Trust RDF ontology](http://xmlns.com/wot/0.1/)
for signing RDF documents with public-key cryptography. Integrating this ontology into our identity
model, we could get something like:

- TODO:
    - Give an code example how WOT could look like in an immutable ledger
    - Make sure that the *immutability* is not violated, the WOT ontology as of now only work with
      mutability


### The LCC Creation `Entity`

A RRM Creation model describes something directly or indirectly made by human beings. According
to the specification, it has only a single required property:

- **CreationMode:** Can take the values `lcc:Manifestation` or `lcc:Work`
    - `lcc:Manifestation`: A perceivable Creation
    - `lcc:Work`: A distinct, abstract Creation whose existence is revealed through one or more
      manifestations


Additionally, a Creation can have the following outgoing references to respective other entities:

- a self referencing link (one-to-many)
- a link to a Place (one-to-many)
- a link to a Party (one-to-many)


Visualized, the LCC RRM Creation model looks like this:


![](media/lccrrmcreation.png)


#### Proposed Transformation

As mentioned in an earlier section already, schema.org's RDF schemata cover the use case of the LCC
RRM Creation model quite closely already. Not only is the vocabulary of
[schema.org/CreativeWork](http://schema.org/CreativeWork) quite extensive already, there is also a
multitude of subtypes which are useful for defining specifics about a creation (e.g.
[schema.org/Book](http://schema.org/Book)).  One destinction we need to draw though is that a LCC
RRM Creation can have two `CreationMode`s, one for a perceivable Creation (called a "Manifestation")
and one for an abstract distinct Creation, a so called "Work". Using IPLD, what we end up with is
two different schemata that are linked using hashes:


```javascript
// A Creation object in IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Creation (can be all subtypes of CreativeWork)>" },
    "name": "Lord of the Rings",
    "author": { "/": "<hash pointing to the Author>" }
}

// A a digital Manifestation of the Creation in IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Manifestation>" },
    "name": "The Fellowship of the Ring",
    "creation": { "/": "<hash pointing to the Creation>" },
    "digital_work": { "/": "<hash pointing to a file on e.g. IPFS>" },
    "fingerprints": [
        "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY",
        "<multihash/multifingerprint value>"
    ],
    "locationCreated": "<URI pointing to a Place object>"
}

// A a physical Manifestation of the Creation in IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Manifestation>" },
    "name": "The Fellowship of the Ring",
    "creation": { "/": "<hash pointing to the Creation>" },
    "datePublished": "29-07-1954",
    "locationCreated": { "/": "<URI pointing to a Place object>" }
}
```


Similarly, the same result can be achieved using JSON-LD:


```javascript
// A Creation object and its Manifestations in JSON-LD
// NOTE: All links have to point "backwards" since we're on an immutable ledger
{
    "@graph": [
        {
            "@id": "#creation",
            "@type": "http://coalaip.schema/Creation",
            "name": "Lord of the Rings",
            "author": "<URI pointing to Author object>"
        },
        {
            "@id": "#digitalManifestation",
            "@type": "http://coalaip.schema/Manifestation",
            "name": "The Fellowship of the Ring",
            "creation": "#creation",
            "digital_work": "<URI pointing to file>",
            "fingerprints": [
                "Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY",
                "<multihash/multifingerprint value>"
            ],
            "locationCreated": "<URI pointing to a Place object>"
        },
        {
            "@id": "#physicalManifestation",
            "@type": "http://coalaip.schema/Manifestation",
            "name": "The Fellowship of the Ring",
            "creation": "#creation",
            "datePublished": "29-07-1954",
            "locationCreated": "<URI pointing to a Place object>"
        }
    ]
}
```

A special feature of this modeling is, as already mentioned, the distinction in "Works" (here typed
as a `Creation`) and "Manifestations" (typed as `Manifestation`). Both physical as well digital
Manifestations could be represented, with the difference that a digital Manifestation should be
registered with a set of fingerprints as well as a link pointing to (at least) an example of the
work. We plan to subtype both `Creation` as well as `Manifestation` from schema.org's CreativeWork
so that extra properties can easily be added on case by case basis.

In the next chapter, we'll go into more detail on how to transform a LCC RRM Right into Linked Data.


### The LCC Right Model

In the LCC RRM document, the Right model is by far the most interconnected. A minimum set of
required properties includes:

- **RightType:** The type of the Right (e.g. all uses, copy, play, stream, license, copyright,
  administration, lcc:RightSet - a container grouping Rights)
- **ToolType:** The type of medium that must be employed in exercising the Right (e.g. only watch on
  mobile phone)
- **MaterialType:** The type of material that may be employed in execising the right (e.g. a type of
  cover material for a book - as a rule of thumb: brush:tool as paint:material)
- **ValidContextType:** A type of context in which the Right must be exercised (e.g. in flight, in
  public, commercial use, academic research)
- **IsExclusive:** Indicating whether the Right is exclusive to the Rightsholder (e.g. true, false)
- **PercentageShare:** The percentage share of the Rights controlled (e.g. 100%, 51%)
- **NumberOfUses:** The number of uses permitted by the Right (e.g. 3 uses, 5 uses, unlimited uses?)
- **ValidPeriod:** The period during which the Right is valid. (e.g. 2015-2016)
- **Territory:** A place where a Right is allowed to be exercised (e.g. North America)


TODO: Note that the LCC RRM specification features a couple more properties which were not included
for the following reasons:

- **HostCreationType:** Unclear what it is and if it's necessary in the world of RDF
- **OutputCreationType:** Unclear what it is
- **ToolType:** The type of medium that must be employed in exercising the Right (e.g. only watch on
  mobile phone) - not sure if this is necessary
- **MaterialType:** The type of material that may be employed in execising the right (e.g. a type of
  cover material for
  - not sure if this is necessary


In addition, a LCC RRM Right can have the following outgoing references to respective other
entities:

- a self-referencing link (one-to-many)
- a link to a Party (one-to-many)
- a link to a Creation (one-to-many)
- a link to a Place (one-to-many)
- a link to a RightsAssignment (one-to-many)
- a link to a Assertion (one-to-many)
- a link to a RightsConflict (one-to-many)


Visualized, the LCC RRM Rights model looks like this:


![](media/lccrrmright.png)


##### Additional Types of Rights

Additionally, the LCC Rights Reference Model specifies three types of LCC RRM Rights that each
implement special functionality:

- **SourceRight:** A Right from which another Right is derived
- **SuperSededRight:** A Right to render a referenced Right invalid
- **RightSet:** A collection of Rights all bundled under a single Right


For putting Rights onto a global registry, handling a RightSet poses a problem. Essentially, the
concurrent transfer of multiple assets on a decentralized ledger cannot be guaranteed and especially
not synchronized.

- TODO: Tim: I talked to Dimi about this, and we were trying to figure this out with
  cryptoconditions, but it seems that at this stage it is not possible.


Both a SourceRight, as well as a SuperSeededRight could easily be represented using an ontology, but
would still complicate ownership logic on an immutable ledger greatly, which is why we've decided to
leave them out of this specification for now.


##### The Notion of Ownership

As can be drawn from the LCC RRM specification, a LCC RRM Right is LCC RRM Party-specific. In a
sense that, a Right is always specific to the Party it's provided for. Assuming that digital
creators would like to distribute their rights to a multitude of interested LCC RRM Parties, the
following steps would be required:

1. Register their Party identifier on a global registry as described in previous sections
2. Register their Creation on a global registry as described in a previous section and link it to
   their Party identifier
3. Register a number of Rights on a global registry, tailored to the interested Party
4. Register a RightAssignment to assign the Rights to the interested Parties


What this implies is that while the actions to perform on Parties and Creations (there is one
exception: a Copyright transfer) are strictly limited to the act of registration, a LCC RRM Right
actually has properties of ownership, allowing it to be transferred (via a LCC RRM RightsAssignment)
from one LCC RRM Party to another.

Furthermore, a LCC RRM Creation is not limited to a single LCC RRM Right. In fact, there can be as
many LCC RRM Rights attached to a LCC RRM Creation as a LCC RRM Party wants. Since licensing
information is stored in a LCC RRM Right though, some edge cases must be considered:

1. Specific software licenseses imply an agreement between the issuer of the Right and the commons,
   meaning that an arbitrary transfer (also called RightsAssignment) to an Individual or
   Organization must not take place. To handle the edge case of giving permissions to literally
   everyone, it is planed to have special Identities symbolizing the commons a Right can be
   transferred to in this case. Additionally, once a Creation has been licensed under such license,
   Rights with licenses that conflict with a "commons license" must not be issued.

- TODO: Maybe there are more edge cases like this. If so, enumerate and discuss/propose solutions.


#### Proposed Transformation

Transforming the LCC RRM Rights model poses some challenges. According to the LCC RRM specification:

- A LCC RRM Right object can both represent copyright as well as licensing information
- A LCC RRM Right can be a SourceRight, SuperSeededRight as well as a RightSet.


In order to allow for Rights to be atomicly transferrable units, we decided to ignore the latter
requirement for now and purely focus on the Right being a transferrable container for specific
licensing information.

Since we weren't able to find an appropriate RDF schema to model the LCC RRM Right, we're proposing
a schema that consolidates the requirements given in:

- [LCC: Rights Reference Model](http://doi.org/10.1000/284)
- [W3C: Open Digital Rights Language](https://www.w3.org/TR/odrl/)
- [W3C: Creative Commons Rights Expression Language](https://www.w3.org/Submission/ccREL/)


```javascript
// A Right object in JSON-LD
{
    "@type": "http://coalaip.schema/Right",
    "@id": "<URI pointing to this object>",
    "usages": "all|copy|play|stream|...",
    "territory": "<URI pointing to a Place>",
    "context": "inflight|inpublic|commercialuse...",
    "exclusive": true|false,
    "numberOfUses: "1, 2, 3, ...",
    "share": "1, 2, 3, ..., 100",
    "validFrom": { "@type": "http://schema.org/Date" },
    "validTo": { "@type": "http://schema.org/Date" },
    "manifestation": "<URI pointing to the Manifestation object>",
    "license": "<URI pointing to a license on an immutable ledger>"
}
```


As can be seen, a LCC RRM Right is basically just a link between a Manifestation and a license.
Since licenses are usually documents with pages of text intended for humans to read and interpret,
pointing to a license must be done by using technology that doesn't allow the content behind a link
to be altered (e.g. an immutable ledger or by using links that implement Content-Addressing). Hence,
an implementation in IPLD is favored:


```javascript
// A Right object in IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Right>" },
    "usages": "all|copy|play|stream|...",
    "territory": { "/": "<hash pointing to a Place>" },
    "context": "inflight|inpublic|commercialuse...",
    "exclusive": true|false,
    "numberOfUses: "1, 2, 3, ...",
    "share": "1, 2, 3, ..., 100",
    "validFrom": { "/": "<hash pointing to RDF-Schema of Date" },
    "validTo": { "/": "<hash pointing to RDF-Schema of Date" },
    "manifestation": { "/": "<hash pointing to the a Manifestation>" },
    "license": { "/": "<hash pointing to the license>" },
}
```


As mentioned in a [previous section](#the-notion-of-ownership) section already, a Right object needs
to be ownable by a participant of the protocol. Being ownable means that only an individual or a
group of individuals owning a specific private key to a corresponding public key can alter the data
that has occurred in certain past transactions. In addition, Ownership transactions (in the RRM
called "RightsAssignment"s, see next section for more detail) of every form
- be it transfers, loans or consignments - must be stored in an orderly fashion, to guarantee an
  always valid chain of provenance of a Manifestation's Rights. The next section will explore this
  functionality further.


### The LCC RightsAssignment Model

According to the LCC RRM specification, a LCC RRM RightsAssignment describes an event. A LCC RRM
RightsAssignment can have the following types (property name: `RightsAssignment`):

- **RightsLaw:** Representing a law by which rights come into existence (e.g. the US Copyright Act
  of 1976)
- **RightsPolicy:** Representing an event by which a Party assigns rights to another Party without
  requiring an agreement of the later (e.g. security level for user access of a computer system)
- **RightsAgreement:** Representing an agreement between to Parties over a right (e.g. a license, a
  publishing agreement, ...)


In addition, the LCC recommends the RightsAssignment to have three statuses (property name:
`RightsAssignmentStatus`):

- `lcc:Offer`: An open RightAssignment the Assignee may accept or reject
- `lcc:Request`: A request of an Assigner to an Assignee to register/create a specific
  RightsAssignment
- `lcc:Executed`: A actual transfer of a LCC RRM Right from a Party to another Party


The LCC RRM RightsAssignment has the following outgoing references:

- a link to a LCC RRM Party (one-to-many)
- a link to a LCC RRM Right (one-to-many)


Visualized, the LCC RRM RightsAssignment model looks like this:


![](media/lccrrmrightsassignment.png)


#### Proposed Transformation

An appropriate transformation for the LCC RRM RightsAssignment model could potentially be found by
looking at schemata for asset transfers. As this specification's scope is to make digital rights
manageable on an immutable ledger though, we're not interested in defining this type of schema, as
we think an immutable ledger must have this operation already built in from the start. This
specification's goal is to be able to run on as many ledgers as possible. Both IPLD and the
Interledger Protocol were chosen consciously, to establish a metadata and licensing ontology that
can potentially overspan many ledgers and immutable data stores. General requirements for a ledger's
transactions are:

- Specific assets must only be transferrable using crypto-key-pair signatures on a transaction
  level.
- Transactions must allow to define a JSON-serializable payload
- An asset's chain of provenance must easily be comprehensible by any user of the protocol
- Asset's must allow to be devided upon registration
- Transactions must either support JSON-LD or IPLD
- Transfer-transactions should support different modes, examples are:
    - A transfer from a group of individuals to a single individual (and vice-versa)
    - A transfer that is only claimable during a certain time span (timelock conditions)
    - A transfer that is only claimable by an individual or group that knows a certain secret value
      (hashlock conditions)
    - TODO: Are there more?
- TODO: Are there more requirements COALA IP asks from a ledger?


By assuming that every ledger implements the concept of asset transfer, a minimal transformed LCC
RRM RightsAssignment would look like this (this is represented in the payload of the ledger-specific
transfer-transaction):


```javascript
// In JSON-LD
{
    "@type": "http://coalaip.schema/Transfer(Payload?)",
    "contract": "<URI pointing to a contract on a ledger>"
}
```


and in IPLD:

```javascript
// In IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Transfer(Payload?)>" },
    "contract": { "/": "<hash pointing to a contract>" }
}
```


Defining the `contract` keyword in a transfer-transaction is not required. It is mentioned at this
point, as it solves lots us use cases for potential users. Legally speaking, a contract defined in
the transfer-transaction of a Right must only contain permissions derived from the original license
issued in the creation of a Right or a former contract of a transfer-transaction. Generally, this
means that only permissions, taken from the set of permissions given from the last owner, are
allowed to be included in the contract.


### The LCC Assertion Model

Since none of the assets registered in the COALA IP ontology are minted and hence under direct
control of a decentralized network only committed to guarantee for all data to be valid, the COALA
IP ontology will inevitably contain false or fraudulent licensing statements made by its users. To
counteract this problem, the LCC RRM recommends the implementation of a LCC RRM Assertion object,
which is an evaluation about the truth or falsehood of a statement made by a participating Party
within the ontology.

The LCC's minimum set of required properties includes:

- **TruthValue**: The truth claim made about the subject of assertion (e.g. `true` or `false`)
- **ValidPeriod**: A time period defining the time of validity for an assertion (e.g from 01.01.2011
  to 01.01.2015)


Additionally, an LCC RRM Assertion can have the following outgoing references:

- a link to a Party (one-to-many)
- a link to a RightsAssignment (one-to-many)
- a link to a Assertion (one-to-many)
- a link to a RightsConflict (one-to-many)


Visualized, the LCC RRM Assertion model looks like this:


![](media/lccrrmassertion.png)


#### Proposed Transformation

An Assertion model in the ontology definitely makes sense, considering that the ontology will
potentially be exposed to every user of the Internet. Hence, a healing mechanism that helps
interpreters of the data to retrieve a statistical truth is helpful for sure. We'd like to propose
though that assertions are not being made on actual models themselves, but rather on a model's
attributes and links.


Think about the following scenario:

Andy Warhol decides to use the COALA IP protocol to register his work on a Blockchain. He's
registering one of his works called "32 Campbell's Soup Cans" as a Creation and attaches a poster of
the work as a Manifestation to it. He also creates a Right defining the licensing terms of buying
the poster and attaches it to the Manifestation. Since Andy is not really good with computers - they
were never really his type of medium - his program contains a bug that leads him to also register a
Creation of Edvard Munch's The Scream under his name. Visually, this is what we'd end up with:


![](media/lccrrmassertionexample1.png)


Now obviously, we've created a awkward situation. We've stored all our ontology on a Blockchain,
supporting IPLD and content-addressed storage, meaning that in contrast to a for example SQL
database, we're by no means able to revert the transactions made. Since we can append-only to a
Blockchain, the solution left to solve the problem is actually to append an Assertion validating
specific statements.

Instead of falsifying the existence of the Creation "The Scream" though (this is what the LCC RRM
recommends), what we'd actually like to assert is the Creation's author property. By using IPLD's
merkle-path feature, we're able to achieve exactly that by defining an Assertion object like this:

```javascript
// In IPLD
{
    "@type": { "/": "<hash pointing to RDF-Schema of Assertion>" },
    "truth": "false",
    "author": { "/": "<hash pointing to a Party>" },
    "subject": {
        "/": "<IPLD hash pointing to Creation: The Scream's author property>"
        // e.g. /ipdb/<hash_of_creation>/author
    },
}

// and

{
    "@type": { "/": "<hash pointing to RDF-Schema of Assertion>" },
    "truth": "true",
    "author": { "/": "<hash pointing to a Party>" },
    "subject": {
        "/": "<IPLD hash pointing to Creation: 32 Campbell's Soup Cans's author property>"
    },
}
```


What we'd end up with is the following:

![](media/lccrrmassertionexample2.png)


As changing any of the objects values of key would provoke a change in the object's IPLD hash and
since mutating data is not possible anyways, we could also simply point the assertion to the object
itself. However then, we'd probably lose valuable metadata and it would be difficult to find out
why an object was flagged by an asserter.



### The LCC RightsConflict Model

TODO:
    - See other introductory sections of LCC models. Use same structure to describe the model


#### Proposed Transformation


## Blockchainify Linked Data

- TODO:
    - Briefly: What is a blockchain
    - By blockchaining every set of linked data of an ontology, a simple claim system is established
    - By requiring the signature of every set of linked data submitted to a blockchain, data is
      linked to the submitters identity. Since the submitter's identity has a certain trust in the
      ontology, the submitter transfers the trust of himself + the trust all people that signed his
      key gave him onto the data he has submitted

    - e.g. I claim that I am Andy Warhol and I've created 32 Campbell's Soup Cans. Well guess,
        what: nobody signed my freshly created identity. Also, Andy Warhol is not alive anymore.
        Sure I can submit this information to the blockchain and show to the world on ascribe that
        I'm the creator, but everyone will be able to tell that I'm very likely NOT Andy Warhol and
        that 32 Campbell's Soup Cans is NOT my creation
    - Super difficult question: When pointing to data that is not on a blockchain, what to do then?
      Discard? Allow it (but careful because data from outside is subject to change)
        - Imagine identities would be stored on a non blockchain. Using the given definition for a
          Creation, I'd go ahead and link it to the identity that is maybe stored on me.tim.com also
          exposing my public key. Now imagine someone gets access to tim.com and just changes my
          public key to a public key they they private key. Can they then make valid claims in my
          name?

## Future

This specification outlined how the LCC Framework can be used to define an RDF ontology on top of
immutable data stores to manage digital rights. As the goal is to make rights modeling an open
international standard and implement the outcome there is a number of events that need to follow
this specification, which are outlined here in sequential order:


- Define a working RDF schema based on this specification
- Include or build open source communities around it
- Using the RDF schema, implement this specification
- Identify a standards commite to send this to
- Take the RDF schema, take the proposed transformations here and put them in a format of a standard
  proposal

Thanks for reading!
