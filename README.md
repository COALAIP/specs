COALA Intellectual Property Specification
============


Contributors to this document:

- Tim Daubenschuetz: <tim.daubenschuetz@gmail.com, tim@ascribe.io>


## PLEASE READ THIS SECTION BEFORE READING THE DOCUMENT

This document is a work in progress! Some sections contain the key word "TODO" + a description on what is expected to be
written there. To everyone reading this: Feel free to take the time to conduct research in this area to write the
section yourself.

If your section contains information from external documents, please make sure to link to the
appropriate sources.

If your section contains lots of knowledge about a field that wasn't mentioned yet, please take the time to define the
field briefly to give the readers of this document a chance to on-board easily.

This document has a soft characters-per-line limit of 120 characters (Note that by including links, only the descriptor
counts). Please respect while writing.

As human beings are capable to recognize patterns easily, make sure to follow the other (here not explicitly mentioned)
text formating patterns as close as possible.


Thank you very much!


## Abstract


## Introduction

This section provides context to the following COALA Intellectual Property Specification. It explains technological
concepts that can be used to model a generic, extensible protocol to manage digital rights.
They are described briefly, to give the reader a comprehensive overview of the field. To understand them in their full
spectrum, we advise the reader to study them further on their own. For each section sources on the topic are given.


### The LCC Framework

The [LCC Framework](http://www.linkedcontentcoalition.org/index.php/rights-data-network/lcc-framework) is a set of documents published by the [Linked Content Coalition](http://linkedcontentcoalition.org/) (short form: LCC)
to unify digital rights data management. The Framework's key documents are:

- [Ten targets for the rights data network](http://doi.org/10.1000/290)
- [The LCC Entity Model](http://doi.org/10.1000/285)
- [The LCC Rights Reference Model](http://doi.org/10.1000/284)
- [The LCC Principles of identification](http://doi.org/10.1000/287), May 2016


In this introductory section, we'll go over each document summarizing their contents briefly to give context and
attribution for the following specification.


#### The LCC's ten targets

The LCC's goal is to enable the widest possible access to appropriate rights information and the automation to rights
trading, independent of commercial or free use. As a general guidance to fulfill their goals, they released a document,
called "[Ten targets for the rights data network](http://doi.org/10.1000/290)" that is composed of the following ten goals:

1. Every Party has a unique global identifier
2. Every Creation as a unique global identifier
3. Every Right has a unique global identifier
4. All identifiers have a URI representation to persistently and predictably resolve them within the Internet
5. Links between identifiers are system agnostic and need to be authorized by participating consortiums
6. Meta data is system agnostic and its schema has to be authorized by participating parties or consortiums
7. The provenance of rights has to be made explicit
8. Any participant is able to make standardized, machine-interpretable statements about rightholdings in creations
9. Conflicts in rights declarations should be automatically identified
10. Digital "fingerprints" or "watermarks" are linked to the registered Creation


For more in-depth information about the goals of the LCC, find the attached link in this section at the top.


**Sources:**

- [LCC: Ten targets for the rights data network](http://doi.org/10.1000/290), May 2016


#### The LCC Entity Model

*Note that knowing the definition of the LCC Entity Model is not vital for understand the contents of this
specification. The LCC Entity Model is a meta-model the LCC defined to model their actual ontology - the LCC Rights
Reference Model.*

The [LCC Entity Model](http://doi.org/10.1000/285) (short form: LCC EM) is a generic meta data model used by the LCC as a
"building block" to define more specific data models like the [LCC Rights Reference Model](http://doi.org/10.1000/284)
(short form: LCC RRM).
In a nutshell, the LCC Entity Model specification defines a model called `Entity` that is composed of five attribute types:

- **Category:** Categorizes the Entity (e.g. Language=iso3166-1a2:EN ("English"))
- **Descriptor:** Names the Entity (e.g. Name="Andy Warhol")
- **Quantity:** Quantifies the Entity (e.g. Height=20cm)
- **Time:** Times the Entity (e.g. DateOfCreation=1999)
- **Link:** Links the Entity (e.g. "Andy Warhol" --- isCreator ---> "32 Campbell's Soup Cans")


In the LCC EM specification, all five of these attributes are represented as models as well. Using unidirectional links,
they make up the actual `Entity` model. An `Entity` itself is linked to other `Entities` bidirectionally, as can be seen
in the attached figure:


![](media/f1.png)


The `Entity` model's attributes are deliberately chosen to be generic so that more complex data models like the LCC RRM can be
built on top. For in-depth information follow the link to the PDF provided in the beginning of this section.


**Sources:**

- [LCC: Entity Model](http://doi.org/10.1000/285), May 2016


#### The LCC Rights Reference Model

The [LCC Rights Reference Model](http://doi.org/10.1000/284) is a formal definition for representing intellectual property
rights digitally. The LCC RRM document is written as a specification for an abstract logical data model, that is built
on top of the LCC EM and is composed of the following seven entities:

- **Party:** A Person or an organization (e.g. "Andy Warhol")
- **Creation:** Something created by a Party (e.g. "32 Campbell's Soup Cans")
- **Place:** A virtual or physical Place (e.g. "New York City, USA")
- **Right:** A set of permissions that entitle a Party to do something with a Creation (e.g. "Andy Warhol controls all
  rights to 32 Campbell's Soup Cans")
- **RightsAssignment:** A decision as a result of which a Right comes into existence (e.g. "According to the 1976 US
  Copyright Act, Andy Warhol controls all rights to 32 Campbell's Soup Cans")
- **Assertion:** A claim made about the substance of a Right (e.g. "I, the Museum of Modern Art, New York, claim that
  Andy Warhol is the righteous creator of 32 Campbell's Soup Cans")
- **RightsConflict:** A statement of disagreement over a Right (e.g. "I, Malory, claim that Andy Warhol is NOT the
  righteous creator of 32 Campbell's Soup Cans")


*Note that for the sake of simplicity, the Entity `Context` was left out of the above described definition. In essence
though, it is just defined as a parent/categorizing class of Right, RightsAgreement, Assertion and RightsConflict and
has hence no significant value.*

As these seven entities are supposed to be building blocks of a global digital rights ontology, they are linked
unidirectional, as can be seen in the attached figure:


![](media/f2.png)


**Sources:**

- [LCC: Rights Reference Model](http://doi.org/10.1000/284), May 2016


#### The LCC Principles of identification

- TODO:
    - Summarize briefly (as done in the other sections) what the document talks about, without going into to much detail


**Sources:**

- [LCC: Principles of identification](http://doi.org/10.1000/287), May 2016


### The semantic web

- TODO:
    - For what is it useful
    - How does RDF look like
    - How does RDF schema look like
    - How does OWL look like


#### JSON Linked Data

[JSON-Linked Data](https://www.w3.org/TR/json-ld/) (short form: JSON-LD) is a data structure merging the concepts of the
[Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/) (short form: RDF) with [JSON](https://tools.ietf.org/html/rfc7159).
Using the concept of a "context", it allows to provide additional mappings by linking JSON-object properties to schemata
in an ontology.

Lets assume we have the following set of data:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```


Now, for a human it's obvious that this set of data is about a person named "Andy Warhol" who was born on the 6th
August 1928. For a machine that is lacking the intuition and _context_ of a human, resolving this representation is rather
difficult.

JSON-LD solves this problem by introducing the concept of a "context" into JSON documents. On a high level, this allows
to link data to already defined schemata.
In order to include "context" into a JSON-object a key called `@context`, needs to be included that defines or
references the schema of the underlying data. Using JSON-LD to define our previously mentioned example, it would look
like this:


```javascript
{
    "@context": "http://schema.org/Person",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06"
}
```


Using the JSON-LD-specific keyword `@context` - pointing to a resource that defines how our data should look like - a
JSON-LD parser could `GET http://schema.org/Person` the schema and validate the attached data against it.
Additionally - and this might be the greatest benefit of JSON-LD -, if some other application developer were to be
handling this kind of data for their users, they could rely on the same schema definition. In turn, this would unify data
representation across services, which is a **GREAT** improvement for the world (wide web).

Think of it like this: Twitter, Facebook, Github, Instagram - they all have the notion of a user model for example.
Some of them might name the key of the user's birthday `birthDay`, while others name it `dayOfBirth`, while again others
would name it `birth_day`. All those keys however, have the same semantic meaning for a user model, as they define when
the user was born. So if all services were to rely on a unified schema for defining their data models, applications that talk across services
would be so much more easier to implement. "Cross-Standard Links" (as the LCC calls it), would come for free actually.

Going back to the example, one question that remains though is: How does JSON-LD know how to map our self-defined key (`givenName`,
`familyName` and `birthDate`) names to the properties of schema.org's Person?
Well, turns out we didn't choose those key names randomly. They're already part of the schema.org's Person definition,
hence a JSON-LD parser is capable to map them automatically and then execute validation against it.

For more clarity, let's see how a JSON-LD parser would look at this example:

1. `GET http://schema.org/Person`
2. For each of the user-defined keys, check if they map to the keys provided in the schema
    2.1 If this is the case, traverse the schema until a leaf node (the JSON-LD specification calls this an
    `identifying blank node`) is found
    2.2 "Expand" the data, replacing a keys name with an URI to its more granular schema definition


To give a practical example, this is how our previously defined set of data would look like:


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


So essentially, what we end up with is a much more verbose form of our set of data. In fact, the JSON-LD specification
gives certain forms names. The form that is shown above is called _expanded_ form, as it was expanded using Person's
schema.org definition defined in `@context`.
The form in the example we gave earlier (where we defined `@context`) is called _compacted_ form.

So essentially, what happens is that the JSON-LD parser assumes we defined the correctly named keys for Person already, so
when expanding the compacted version of our JSON-object, it just individually looks them up at
`http://schema.org/Person` and if they're defined, replaces them with more detailed URIs to their schema definition.
What we end up with is a automatically mapped set of data by simply using what is already out there.
Since every key of a given value now points to a leaf node of a schema ontology and since leaf nodes are only allowed to
define the most basic types (like string, boolean, number), the parser can then easily traverse the document and
validate each occurrence of `@value`.


##### Final thoughts

With this example, we've just shown you the tip of the ice berg.
JSON-LD has tremendous powers (Aliasing, Self-Referencing, Built-in types, Indexing, ...) to do all kinds of crazy things.
Since this specification will make use of JSON-LD heavily, we encourage to learn more about JSON-LD before continuing
reading this document. Useful links can be found in the **Sources** section below.
*As a side note: The authors of the JSON-LD RFC have done a tremendous job writing anapprochable specification.
It features great examples and is easy to read. :+1:*


**Sources:**

- [W3C Recommendation: JSON Linked Data 1.0](https://www.w3.org/TR/json-ld/), May 2016
- [Wikipedia: JSON-LD](https://en.wikipedia.org/w/index.php?title=JSON-LD&oldid=715712992), May 2016
- [Codeship: JSON-LD: Building Meaningful Data APIs](http://blog.codeship.com/json-ld-building-meaningful-data-apis/),
  May 2016


#### schema.org

- TODOs in this section:
    - Just describing schema.org is I think way to narrow here. This section should be about linked data on the world
      wide web in general. schema.org is just a regular player when it comes to linked data and RDF. Hell, there are even
      search engines that users can lookup schemas (http://wiki.dbpedia.org/). Obviously mention schema.org as a
      prefered source though.


schema.org is a collaborative initiative with the mission to create, maintain and promote schemata for structured data on
the Internet. It's vocabulary is defined as an ontology, connecting different concepts using links. It can be
used with different encodings, including RDFa, Microdata and _JSON-LD_.


##### Available Schemas

Schema.org includes the following schemata that could be helpful in defining a digital intellectual property
specification based on LCC's EM/RRM:

- [schema.org/Person](http://schema.org/Person): See LCC RRM `Party`
- [schema.org/Organization](http://schema.org/Organization): See LCC RRM `Party` (A `Person` can be member of an
  `Organization`)
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


*A full list of all core schema.org schemata can be found [here](https://schema.org/docs/full.html)*


##### Extensibility of schema.org

As it is the goal of this specification to convert LCC's RRM to a linked data ontology, using JSON-LD and schema.org, we
need to be able to model the seven main LCC RRM entities: Party, Creation, Place, Right, RightsAssignment, Assertion,
RightsConflict using schema.org and JSON-LD.
While enumerating schema.org's schemata in the previous section, for schemata that have similarities to the LCC's
definition we already marked them. For clarity though, we're listing them in this section again to also highlight what
schema.org **doesn't** provide yet:


**What schema.org helps us with:**

- **LCC Party:** [schema.org/Organization](http://schema.org/Organization) and [schema.org/Person](http://schema.org/Person)
- **LCC Creation:** [schema.org/CreativeWork](http://schema.org/CreativeWork) and all its subschemata could be used
- **LCC Place:** [schema.org/Place](http://schema.org/Place)
- **LCC Assertion:** [schema.org/AssessAction](http://schema.org/AssessAction)


**What schema.org _doesn't_ help us with (yet?):**

- **LCC Right**
- **LCC RightsAssignment**
- **LCC RightsConflict**


So even though schema.org already helps us by defining some of the LCC models some do not even exist at all (specificatlly
Rights, RightsAssignment and RightsConflict). Although, this seems like a problem at first, it is not. schema.org's
schemata are easily extensible. schema.org [even encourages](http://schema.org/docs/extension.html) subclassing their 'core'
schemata into so called 'hosted' and 'external' extensions. In general, there are three types of schemata on schema.org:

- **Core:** A basic vocabulary for describing the kind of entities the most common web applications need
- **Hosted:** Subclassed models from Core that have their own namespace (e.g. http://health-lifesci.schema.org/) and
  were reviewed by the schema.org community. Hosted extensions should be application-agnostic
- **External:** Subclassed models from Core/Hosted that do have an application-specific namespace (e.g.
  http://schema.bigchaindb.com). External extensions may be application-specific


Applied to the contents of this specification, this would mean that application-agnostic schemata (so everything contained
in LCC RRM) would ideally become a Hosted extension, while application-specific schemata (data models that are specific for
a certain copyright society or in our case ascribe.io) would become External schemata.
Fortunately, using schema.org in this way it also complies with rule five and six of the ten LCC targets for the rights
data network, which say:

- Rule 5: Links between identifiers are system agnostic and need to be authorized by participating consortiums
- Rule 6: Meta data is system agnostic and its schema has to be authorized by participating parties or consortiums


**Sources:**

- [Schema.org](http://schema.org), May 2016
- [Schema.org: Full Hierarchy](http://schema.org/docs/full.html), May 2016
- [Schema.org: Schema.org Extension](http://schema.org/docs/extension.html), May 2016


### TBD: IPLD or only IPFS?

- TODO: Figure out if using IPLD and IPFS in this spec already makes sense.
  Probable answer is: yes, since IPFS basically a registry for blobs. IPLD, not so sure as it's basically linked data on
  ipfs, which wouldn't help to much in terms of registries


### The Web of Trust

- TODO: Explain briefly
- Point to initiatives going on
- https://github.com/WebOfTrustInfo/rebooting-the-web-of-trust
- http://xmlns.com/wot/0.1/
- http://www.weboftrust.info/


## Modeling LCC RRM using Linked Data

In this section we describe how LCC's Rights Reference Model can be modeled using JSON-LD and schema.org. In other
words, we'll go over each model description given in the LCC Rights Reference Model document and discuss how the
respective model can be translated into JSON-LD.


### General Approach

In this section, we describe abstractly how to get from a LCC RRM model to a RDF-compatible JSON-LD model.
As mentioned earlier already, with its document "[LCC: Entity Model](http://doi.org/10.1000/285)", the defined a nice
generic model to base their actual Rights Reference Model on.
What this document in essence describes, is how to implement a data model that is fully extendable using a multitude of
linked entities. Using an RDF-based data structure in turn, means that defining a base data structure for
linking entities is not necessary anymore, as this is what RDF is all about already.
What this leaves us with is that in order to successfully redefine the LCC Rights Reference Model, what we're left to do
is:

- Identify RDF schemata that map to respective entities defined in the LCC RRM specification
    - If appropriate RDF schemata are not available:
        - Compose own RDF types from multiple RDF schemata
        - Define own RDF schemata
- Define how entities are identified and resolved
- Resolve mismatches between the LCC RRM lingo and RDF schemata


### The LCC Place Model

In the LCC Framework, a Place describes a localizable or virtual place. Understandably, it has the following property:

- **PlaceType:** Defining the type of Place
    - `lcc:LocalizablePlace`: A Place in the universe that can be described using spatial coordinates
    - `lcc:VirtualPlace`: A non-localizable Place at which a resource may be located


In addition, a Place can have the following outgoing reference to respective other entities:

- a self-referencing link (one-to-many)


Visualized the LCC RRM Place looks like this:


![](media/lccrrmplace.png)


#### Proposed Transformation

Compared to schema.org's definition of a Place, the LCC RRM Place both describes a physical as well as a virtual Place.
In this specification though, we need to separate the two concepts explicitly upfront, to avoid confusions further in
the transformation process.

For further reference, a:

- **LLC RRM Place or Place** will be used to describe a localizable Place, meaning a Place in the universe that can be
  described using spatial coordinates
- **Universal Resource Identifier** will be used to describe a virtual place at which a resource may be located


This implies that a LCC RRM Place of `PlaceType == lcc:LocalizablePlace` will be transformed to a RDF Place, while a
LCC RRM Place of `PlaceType == lcc:VirtualPlace` will just be URIs in documents, linking in between data sets.

Using schema.org's Place, a transformation is straight forward (example taken from schema.org):


```javascript
{
    "@context": "http://schema.org",
    "@type": "Place",
    "geo": {
        "@type": "GeoCoordinates",
        "latitude": "40.75",
        "longitude": "73.98"
    },
    "name": "Empire State Building"
}
```

Using the special `containsPlace` property, self-referencing links to other Places are possible.


### The LCC Party Model

A recommendation of the LCC is that the Party model must be able to represent any class of party, meaning:

- a rightsholder
- a licensor
- a administrator
- a user
- or any other participant doing something related to a right.


In the LCC RRM document a party has to have the following properties:

- **PartyType:** Defines whether the party is an individual or a group of individuals
- **DateOfBirth:** Only if PartyType == 'lcc:Individual'
- **DateOfDeath:** Only if PartyType == 'lcc:Individual'


Additionally, a Party can have the following outgoing references to respective other entities:

- a self-referencing link (one-to-many relationship)
- a link to a Place (one-to-many relationship)


Visualized the LCC Party model looks like this:


![](media/lccrrmparty.png)


Another feature of the LCC RRM Party model is that it must only have a `DateOfBirth` and a `DateOfDeath` when its `PartyType`
is `lcc:Individual`. Other features are that it may have are self-referencing links as well as links to LCC RRM Places.
Note that using the property `PartyType` an LCC RRM Party can both represent an individual as well as an organization.


#### Proposed Transformation

*As a side note: In this chapter, we describe the transformation of the LCC RRM Party model to a JSON-LD Person and/or Organization
very literally, as we want to provide reasoning for individual steps of the transformation. Note, that this will just be
the case for this chapter, as in essence the rationale for transforming other models is fairly similar.*

schema.org defines already both a [schema.org/Person](http://schema.org/Person) as well as a [schema.org/Organization](http://schema.org/Organization).
Hence, there is no need to define both concepts as a single model and differentiate using `PartyType`. To value Separation of
Concerns, lets first transform the LCC RRM Party model with `PartyType == 'lcc:Individual'`, then apply the discovered results
to `PartyType == 'lcc:Organization'`.


##### Transform LCC RRM Party to RDF Person

Using the minimum number of properties the LCC RRM describes, a `PartyType == 'lcc:Individual'` LCC RRM Party in JSON-LD using
schema.org's Person could look like this:


```javascript
{
    "@context": {
        "@vocab": "http://schema.org/",
        "DateOfBirth": "birthDate",
        "DateOfDeath": "deathDate"
    },
    "@type": "Person",
    "@id": "https://en.wikipedia.org/wiki/Andy_Warhol",
    "DateOfBirth": "1928-08-06",
    "DateOfDeath": "1987-02-22",
}
```


Now, obviously mapping `birthDate` and `deathDate` of schema.org's Person to LCC's `DayOfBirth` and `DayOfDeath` doesn't
make a lot of sense. Neither do they comply with the way JSON is usually formated (e.g. first letter is lower case), nor is
it necessary to reinvent the wheel on top of schema.org (by for example coming up with new names for properties). So for
simplicity purposes, we simply get rid of the so called JSON-LD-'Aliasing', using the properties schema.org provides us with:


```javascript
{
    "@context": "http://schema.org/",
    "@type": "Person",
    "@id": "https://en.wikipedia.org/wiki/Andy_Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22",
}
```


Now, we used Andy Warhol's Wikipedia page as his Party ID (`@id`). Considering that fact that all we need to provide is
a resolvable URI, a JSON-LD parser will validate this without complaining.
Ideally though, `@id` is a reflection of the data itself, showing a JSON-LD parser where this set of data is actually
resolvable within the Internet. Since `https://en.wikipedia.org/wiki/Andy_Warhol`, does not exactly return the given data,
we'll have to do something about this.

First off, lets look at some requirements various involved parties have given:


**JSON-LD:**

- an `@id`'s value must be represented as an [Internationalized Resource Identifier](https://tools.ietf.org/html/rfc3987) (short form: IRI), either absolute or relative


**LCC's ten targets for the rights data network:**

- A Party's ID should be represented as an [International Standard Name Identifier](http://www.iso.org/iso/catalogue_detail?csnumber=44292) (short form: ISNI) linking to the [International Standard Name Hub](http://www.isni.org)
- A Party's ID should have an [Universal Resource Identifier](https://tools.ietf.org/html/rfc1630) (short form: URI) representation, so that it can be resolved predictably and persistently within the Internet


**LCC's Principles of identification:**

- A Party should have at least one persistent unique public ID that is both human- and machine-readable
- Has a Party multiple public IDs, then there should be a way that enables one identifier to be automatically 'translated' to
  another
- A Party's ID may have multiple designations (e.g. ISBN-10, ISBN-13, ISBN-A)
- A Party's ID should have an [Universal Resource Identifier](https://tools.ietf.org/html/rfc1630) (short form: URI) representation
- A Party ID's characters or elements have no intended meaning that could lead to misinterpretation by humans
- A Party ID's characters or elements include no information about the Party itself or its registration date
- **TODO: There are even more requirements in this document that should be listed here!**


As we're proposing this practical specification based on the LCC Framework and JSON-LD with the background of saving all
linked entity data on public ledgers (read: "Blockchains" or "Registries"), we'd like to add our own set of requirements:

- Elements of the Party's ID may represent the public part of an asymmetric cryptographic key pair
    - If so, the public key should be represented using a unified way of encoding (as inspiration see [Bitcoin Address public key encoding](https://en.bitcoin.it/wiki/Technical_background_of_version_1_Bitcoin_addresses)
- A Party must only allowed to be issued when providing at least one valid public part of an asynchronous cryptography key pair


As the combination of these requirements do not exist as a coherent system as of now, we'll just pretend for the sake of
completeness that there is in fact a system that fulfills them all.
Hence for all following examples, we'll use an imaginary identity service that acts as a registry for the LCC RRM Party
data. It:

- lets users issue an identity that can be resolved to JSON-LD using [Content Negotiation](https://www.w3.org/Protocols/rfc2616/rfc2616-sec12.html)
- lets users attach the public part of their key pairs to their identity.


Notable services for this type of use case could be:

- https://pgp.mit.edu/
- https://keybase.io/
- https://ipfs.io/
- a public instance of https://www.bigchaindb.com/


Preferably, a decentralized, non-profit service is chosen.
Going back to the example mentioned earlier, complying with the requirements would mean that we'd have to replace the `@id`
of the data set representing the Party of Andy Warhol ideally with an URI pointing to his identity page.


```javascript
{
    "@context": {
        "@vocab": "http://schema.org/",
        "birthDate": "birthDate",
        "deathDate": "deathDate",
        "givenName": "givenName",
        "familyName": "familyName"
    },
    "@type": "http://linkedcontentcoalition.com/Identity",
    "@id": "https://identityservice.com/identities/14XgsoWd47KLJa6Ehu62j7Mugq38zX5umt",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1928-08-06",
    "deathDate": "1987-02-22"
}
```


As lots of the users' data will be saved on public ledgers - meaning the user is required to sign the data they're submitting -
we'll need to make sure to map their cryptographical identity to their registered identity.
Luckily, the [Friend of a Friend Project](http://www.foaf-project.org/) has us covered already by providing [an RDF
schema for signing RDF documents using Public Key Cryptography](http://xmlns.com/wot/0.1/), called Web of Trust RDF.
Including this Ontology into our identity schema, it could look like this:


```javascript
{
    "@context": {
        "schema": "http://schema.org/",
        "wot": "http://xmlns.com/wot/0.1",
    },
    "@type": "http://linkedcontentcoalition.com/Identity",
    "@id": "https://identityservice.com/identities/14XgsoWd47KLJa6Ehu62j7Mugq38zX5umt",
    "schema:givenName": "Andy",
    "schema:familyName": "Warhol",
    "schema:birthDate": "1928-08-06",
    "schema:deathDate": "1987-02-22",
    "wot:hasKey": {
        "@type": "wot:PubKey",
        "pubkeyAddress": "https://ipfs.io/ipfs/Qmbs2DxMBraF3U8F7vLAarGmZaSFry3vVY5zytuN3BxwaY",
        "fingerprint": "CE3097A2A1015D28A0AA643AA3DB3EBEFFEAD7E0",
        "length": 2048,
        "identity": {
            "@type": "wot:User",
            "name": "Andy Warhol (This is just a dummy key for demonstration purposes!)"
        }
    }
}
```


Two other requirements we yet need to resolve are the links proposed in the LCC RRM Party Model. As mentioned previously,
there can be a one-to-many relationship from a LCC RRM Party to other LCC RRM Parties as well as a one-to-many relationship
between a LCC RRM Party and LCC RRM Places. Now, when studying the LCC RRM document, it becomes clear that theoretically
these requirements need to be fulfilled, as there could be use cases where:

- multiple Parties share a relationship (e.g. Party A and Party B created Creation C)
- Parties might provide Places as a meta data (think: their home location, a contact place or a billing address)
- multiple Parties may be bundled to an Organization


Hence, in this context, it makes sense to define these relationships as one-to-many relationship.
Using RDF and JSON-LD however, explicitly defining a one-to-many relationship doesn't make a lot of sense. Links need
to be named and usually express a very specific logical fact in the ontology. While it means that theoretically these
relationships would still be possible, usually in JSON-LD they're extended as needed, adjusting either the JSON-LD
object itself or its underlying RDF implementation.

To give some examples, say we want to specify a Person's home addresses. What we can do in this case is just use [schema.org/Person](http://schema.org/Person)'s
`homeLocation` and specify either [schema.org/Place](http://schema.org/Place)s or [schema.org/ContactPoint](http://schema.org/ContactPoint)s.
As another example, imagine we'd like to specify that Party A is a parent of Party B. This could be useful when trying
to express that usage rights of an creation are transferred after the death of Party B, to the heir, Party A.
Fortunately, schema.org's Person again has us covered. A [schema.org/Person](http://schema.org/Person) has a property `parent`
(accepting values of type `Person) that maps perfectly.

Now, imagine we wanted to define a relation, schema.org's Person doesn't provide us with yet. What we would need to do is
extend schema.org's Person defining our own RDF and then host it as either a "hosted" or "external" extension of schema.org (See
more about this in the previous section called: "Extensibility of schema.org").

Now that we figured out all the specialities surrounding the transformation of a LCC RRM model into JSON-LD,
defining an Organization is straight forward.


##### Transform LCC RRM Party to RDF Organization

In essence, a LCC RRM Party of `PartyType == lcc:Organization` is a group of individuals represented by a single entity.
Using the minimum number of properties the LCC RRM document describes, a LCC RRM Party of `PartyType == lcc:Organization` in
JSON-LD using schema.org's Organization could look like this:


```javascript
{
    "@context": "http://schema.org/",
    "@type": "Organization",
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

- TODO: This needs a lot of specing out. How can members of an organization collectively sign something they're
  submitting? Is there a single public key address assigned to an organization or does the organization just bundle
  members that act like they were in an organization but act independently?


### The LCC Creation Model

A LCC RRM Creation model describes something directly or indirectly made by human beings. According to the
specification, it has only a single required property:

- **CreationMode:** Can take the values `lcc:Manifestation` or `lcc:Work`
    - `lcc:Manifestation`: A perceivable Creation
    - `lcc:Work`: A distinct, abstract Creation whose existence is revealed through one or more manifestations


Additionally, a Creation can have the following outgoing references to respective other entities:

- a self referencing link (one-to-many)
- a link to a Place (one-to-many)
- a link to a Party (one-to-many)


Visualized, the LCC RRM Creation Model looks like this:


![](media/lccrrmcreation.png)


#### Proposed Transformation

As mentioned in an earlier section already, schema.org covers the use case of Creation quite closely already. Not only
is the vocabulary of [schema.org/CreativeWork](http://schema.org/CreativeWork) quite extensive already, there is a
multitude of subtypes which are useful for defining specifics about a creation (e.g. [schema.org/Book](http://schema.org/Book)).
Using schema.org to define an LCC RRM Creation model (more specifically: a book) in JSON-LD could look like this
(example taken from schema.org)


```javascript
{
    "@context": "http://schema.org",
    "@id": "#trilogy",
    "@type": "Book", <-- Is a subtype of CreativeWork
    "hasPart": [
        {
            "@id": "#book3",
            "@type": [
                "Book",
                "PublicationVolume"
            ],
            "name": "The Return of the King",
            "datePublished": "20-10-1955",
            "locationCreated": "http://dbpedia.org/page/United_Kingdom",
            "isPartOf": "#trilogy",
            "inLanguage": "en",
            "volumeNumber": "3",
            "author": "https://identityservice.com/identities/1CUd9hYE768sMp2CmNNnAZuVmeBGmAGDMS"  <-- points to Tolkien's registered identity
        },
        {
            "@id": "#book2",
            "@type": [
                "Book",
                "PublicationVolume"
            ],
            "name": "The Two Towers",
            "datePublished": "11-11-1954",
            "locationCreated": "http://dbpedia.org/page/United_Kingdom",
            "isPartOf": "#trilogy",
            "inLanguage": "en",
            "volumeNumber": "2",
            "author": "https://identityservice.com/identities/1CUd9hYE768sMp2CmNNnAZuVmeBGmAGDMS"
        },
        {
            "@id": "#book1",
            "@type": [
                "Book",
                "PublicationVolume"
            ],
            "name": "The Fellowship of the Ring",
            "datePublished": "29-07-1954",
            "locationCreated": "http://dbpedia.org/page/United_Kingdom",
            "isPartOf": "#trilogy",
            "inLanguage": "en",
            "volumeNumber": "1",
            "author": "https://identityservice.com/identities/1CUd9hYE768sMp2CmNNnAZuVmeBGmAGDMS"
        }
    ],
    "name": "Lord of the Rings",
    "inLanguage": "en",
    "genre": "fictional",
    "author": "https://identityservice.com/identities/1CUd9hYE768sMp2CmNNnAZuVmeBGmAGDMS"
}
```


In this case, the LCC RRM Creation with `CreationMode == lcc:Work` would be the outer object, abstractly grouping a set
of LCC RRM Creations with `CreationMode == lcc:Manifestation`. A special property in this case is `hasPart`, which
allows reflectively linking to other Creations. As well as `locationCreated` a predicate specific to schema.org that
accepts a [schema.org/Place](http://schema.org/Place). By looking closely at it's like to dbpedia, we can conclude that
even though dbpedia itself defines `country` as its own type but features schema.org's Place as a possible type, which
is why the link we've provided is valid.

As this example should be sufficient for showing how straight forward the transformation of an LCC RRM Creation model to
a RDF-based model is, we can continue with transforming the LCC RRM Place.


### The LCC Right Model
#### Proposed Transformation


### The LCC RightsAssignment Model
#### Proposed Transformation


### The LCC Assertion Model
#### Proposed Transformation


### The LCC RightsConflict Model
#### Proposed Transformation


## Blockchainify Linked Data

- TODO:
    - Briefly: What is a blockchain
    - By blockchaining every set of linked data of an ontology, a simple claim system is established
    - By requiring the signature of every set of linked data submitted to a blockchain, data is linked to the submitters
      identity. Since the submitter's identity has a certain trust in the ontology, the submitter transfers the trust of
      himself + the trust all people that signed his key gave him onto the data he has submitted

      - e.g. I claim that I am Andy Warhol and I've created 32 Campbell's Soup Cans
      Well guess, what - Nobody signed by freshly created identity. Also, Andy Warhol is not alive anymore. Sure I can
      submit this information to the blockchain and show to the world on ascribe that I'm the creator, but everyone will
      be able to tell that I'm very likely NOT Andy Warhol and that 32 Campbell's Soup Cans is NOT my creation
    - Super difficult question: When pointing to data that is not on a blockchain, what to do then? Discard? Allow it
      (but careful because data from outside is subject to change)
        - Imagine identities would be stored on a non blockchain. Using the given definition for a Creation, I'd go
          ahead and link it to the identity that is maybe stored on me.tim.com also exposing my public key. Now imagine
          someone gets access to tim.com and just changes my public key to a public key they they private key. Can they
          then make valid claims in my name?
