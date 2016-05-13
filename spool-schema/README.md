SPOOL-Schema
============

# Abstract


# Introduction


## The LCC's ten targets


The [Linked Content Coalition](http://linkedcontentcoalition.org/)'s goal is to enable the widest possible access to
appropriate rights information and the automation to rights trading, independent of commercial or free use.
As a general guidance to fulfill their goals, they released a document, called "[Ten targets for the rights data
network](http://doi.org/10.1000/290)" that is composed of the following ten goals:

1. Every Party has a unique global identifier
2. Every Creation as a unique global identifier
3. Every Right has a unique global identifier
4. All identifiers have a URI representation to persistently and predictably resolve them within the internet
5. Links between identifiers are system agnostic and need to be authorized by participating consortiums
6. Meta data is system agnostic and has to be authorized by participating parties or consortiums
7. The provenance of rights has to be made explicit
8. Any participant is able to make standardized, machine-interpretable statements about rightholdings in creations
9. Conflicts in rights declarations should be automatically identified
10. Digital "fingerprints" or "watermarks" are linked to the registered Creation


For more in-depth information about the goals of the LCC, find the attached link in this section at the top.


**Sources:**

- [LCC: Ten targets for the rights data network](http://doi.org/10.1000/290), May 2016


## The LCC Entity Model

*Note that knowing the definition of the LCC Entity Model is not vital for understand the contents of this
specification. The LCC Entity Model is a meta-model the LCC defined to model their actual ontology - the LCC Rights
Reference Model.*

The [LCC Entity Model](http://doi.org/10.1000/285) (short form: LCC EM) is a generic meta data model used by the LCC as a
'building block' to define more specific data models like the [LCC Rights Reference Model](http://doi.org/10.1000/284)
(short form: LCC RRM).
In a nutshell, the LCC Entity Model specification defines a model called `Entity` that is composed of five attribute types:


- **Category:** Categorizes the Entity (e.g. Language=iso3166-1a2:EN ("English")
- **Descriptor:** Names the Entity (e.g. Name="Andy Warhol")
- **Quantity:** Quantifies the Entity (e.g. Height=20cm)
- **Time:** Times the Entity (e.g. DateOfCreation=1999)
- **Link:** Links the Entity (e.g. "Andy Warhol" --- isCreator ---> "32 Campbell's Soup Cans")


As these five attributes make up the actual `Entity` model, they are linked unidirectional. `Entity`s are linked
bidirection, as can be seen in the attached figure:

![](media/f1.png)

The Entity model's attributes are deliberately chosen to be generic so that more complex data models like the LCC RRM can be
built on top. For in-depth information follow the link to the PDF provided in the beginning of this section.


**Sources:**

- [LCC: Entity Model](http://doi.org/10.1000/285), May 2016


## The LCC Rights Reference Model

The [LCC Rights Reference Model](http://doi.org/10.1000/284) is a formal definition for representing intellectual property
rights. The document is written as a specification for an abstract logical data model. The LCC Rights Reference Model is
built on top of LCC EM, and is composed of the following seven entities - all of them based (read: inheritance) on the LCC
Entity Model:


- **Party:** A Person or an organization (e.g. "Andy Warhol")
- **Creation:** Something created by a Party (e.g. "32 Campbell's Soup Cans")
- **Place:** A virtual or physical Place (e.g. "New York City, USA")
- **Right:** A set of permissions that entitle a Party to do something with a creation (e.g. "Andy Warhol controls all
  rights to 32 Campbell's Soup Cans")
- **RightsAssignment:** A decision as a result of which a Right comes into existence (e.g. "According to the 1976 US
  Copyright Act, Andy Warhol controls all rights to 32 Campbell's Soup Cans")
- **Assertion:** A claim made about the substance of a Right (e.g. "I, the Museum of Modern Art, New York, claim that
  Andy Warhol is the righteous creator of 32 Campbell's Soup Cans")
- **RightsConflict:** A statement of disagreement over a Right (e.g. "I, Malory, claim that Andy Warhol is NOT the
  righteous creator of 32 Campbell's Soup Cans")


*Note that for the sake of simplicity, the Entity `Context` was left out of the above described definition. In essence
though, it is just defined as a parent/categorizing class of Right, RightsAgreement, Assertion and RightsConflict and
has no significant value.*

As these seven entities are supposed to be building blocks of a global digital rights ontology, they are linked
unidirectional, as can be seen in the attached figure:

![](media/f2.png)


**Sources:**

- [LCC: Rights Reference Model](http://doi.org/10.1000/284), May 2016


## JSON Linked Data

[JSON-Linked Data](https://www.w3.org/TR/json-ld/) (short form: JSON-LD) is a data structure merging the concepts of the
[Resource Description Framework](https://www.w3.org/TR/rdf11-concepts/) (short form: RDF) with [JSON](https://tools.ietf.org/html/rfc7159).
Using the concept of a "context", it allows to provide additional mappings by linking JSON-object properties to schemas
in an ontology.

### Example

Lets assume we have the following set of data:


```javascript
{
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1987-02-22"
}
```


Now, for a human it's obvious that this set of data is about a person named "Andy Warhol", who was born on the 22th
February 1987. A machine that is lacking the intuition and _context_ of a human, resolving this representation is rather
difficult.

JSON-LD solves this problem by introducing the concept of a "context" into JSON documents.
In order to include "context" into a JSON-object a key called `@context`, needs to be included that defines or
references the schema of the underlying data. Using JSON-LD to define our previously mentioned example, it would look
like this:


```javascript
{
    "@context": "http://schema.org/Person",
    "givenName": "Andy",
    "familyName": "Warhol",
    "birthDate": "1987-02-22"
}
```


This is useful, since we've defined the JSON-LD-specific keyword `@context` in our data, which points to a resource defining how
our data should look like, a JSON-LD parser could lookup this schema (by `GET`'ing `http://schema.org/Person`) and
validate the attached data against it.
Additionally - and this might be the greatest benefit of JSON-LD -, if some other application developer were to also
define a data for their users, they could rely on the same schema definition. In turn, this would unify data
representation across services, which is a **GREAT** improvement for the world (wide web).

Think of it like this: Twitter, Facebook, Github, Instagram - they all have the notion of a user model.
Some of them might name the key of the user's birthday `birthDay`, while others name it `dayOfBirth`, while again others
would name it `birth_day`. However, all those keys in the user model have the same semantic meaning - they define when the user
was born. So if they'd all rely on a unified schema for defining their data models, services that talk across services
would be so much more easier to implement. "Cross-Standard Links" (as the LCC calls it), would come for free actually.


Going back to the example, one question that remains is how our self-defined keys (`givenName`, `familyName` and
`birthDate`) can simply be mapped to schema.org's definition of Person.
Well, turns out we didn't choose these key names randomly. They're already part of the schema.org's Person definition.
So essentially, what the example shows is the most abbreviated version of this JSON-object. In fact, the JSON-LD RFC
gives this form even a name. It's called _compacted_.

For more clarity, lets see how a JSON-LD compiler would look at this example.


1. `GET http://schema.org/Person`
2. For each of the user-defined keys, check if they exist in the schema provided in `@context`
    2.1 If this is the case, traverse their schema until leaf node is found and validate the data


A more verbose variant of this data (the JSON-LD RFC calls this _expanded_) would then look like this:


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
            "@value": "1987-02-22"
        }
    ],
}
```

So essentially, what happens is that the JSON-LD parser assumes we defined the correct properties for Person already, so
when expanding the compacted version of our JSON-object, it just individually looks them up at
`http://schema.org/Person` and if they're defined, sets them as keys. What we end up with is a automatically mapped set
of data by simply using what is already out there.

In essence though, with this example, we've just shown you the tip of the ice berg.
JSON-LD has tremendous powers (Aliasing, Self-Referencing, Built-in types, Indexing, ...) to do all kinds of crazy things,
that could revolutionize on how we think about data on the web. Since this specification will make use of JSON-LD heavily,
we encourage to learn more about JSON-LD before continuing reading this document. Useful links can be found in the
**Sources** section below. *As a side note: The authors of the JSON-LD RFC have done a tremendous job writing an
approchable specification. It features great examples and is easy to read. :+1:*


**Sources:**

- [W3C Recommendation: JSON Linked Data 1.0](https://www.w3.org/TR/json-ld/)
- [Wikipedia: JSON-LD](https://en.wikipedia.org/w/index.php?title=JSON-LD&oldid=715712992), May 2016
- [Codeship: JSON-LD: Building Meaningful Data APIs](http://blog.codeship.com/json-ld-building-meaningful-data-apis/),
  May 2016
