# HOW TO DO BIBJSON

It's just JSON... only, with some useful conventions.

_Contents_

* [Purpose](#the-purpose-of-bibjson)
* [Overview](#overview)
* [Objects](#which-things-are-objects)
* [Links/Identifiers](#web-links-and-identifying-things)
* [License](#licensing-things)
* [Collection](#collection)
* [Linked Data](#linked-data)
* [Parsing](#parsing-to-bibjson)
* [Feedback](#what-do-you-think)

## The purpose of BibJSON

BibJSON is a convention for representing bibliographic metadata in JSON; 
it makes it easy to **share** and **use** bibliographic metadata online.

It is a form of [JSON](http://www.json.org) - a simple, useful and common way of representing data on the web; we use it to shift information around between our apps.

When we want to share with other people, having some conventions about how to use the JSON to do so can be a very useful thing.

By sharing BibJSON in an agreed manner, we can share data online and use it directly in web applications to quickly and easily make better use of our data.

BibJSON is designed to be **simple** and **useful** above all else.
It has virtually no requirements, and you could use your own namespaces to extend it. 
Use it as best fits the purpose of your community.

```json
{
  "title": "Open Bibliography for Science, Technology and Medicine",
  "author":[
    {"name": "Richard Jones"},
    {"name": "Mark MacGillivray"},
    {"name": "Peter Murray-Rust"},
    {"name": "Jim Pitman"},
    {"name": "Peter Sefton"},
    {"name": "Ben O'Steen"},
    {"name": "William Waites"}
  ],
  "type": "article",
  "year": "2011",
  "journal": {"name": "Journal of Cheminformatics"},
  "link": [{"url":"http://www.jcheminf.com/content/3/1/47"}],
  "identifier": [{"type":"doi","id":"10.1186/1758-2946-3-47"}]
}
```

## Overview

* A BibJSON record is a JSON object
* A BibJSON collection is a JSON object containing "metadata" followed by "records"
* The "records" key in a collection points to a list of BibJSON records (JSON objects)
* The collection and the records both have the "collection" key, and their value should be the same
* Each record should have a "cid" - an identifier unique within the parent collection
* Each record should have a "type" - such as "article", "book", or even "author"
* Record type places no constraint on what can be placed in the record
* The default set of keys are based on the [bibtex keys](http://en.wikipedia.org/wiki/BibTeX)
* BibJSON keys are lowercase, no spaces, and usually singular
* The keys can point to strings, lists, or objects
* Any thing that is a simple string should remain so
* Where object complexity is required, make it an object
* Where additional keys are namespaced, include a "namespace" declaration in the collection "metadata"
* BibJSON APIs may return other metadata relevant to the parent app; developers can identify such metadata by prefixing the key with "_"; just ignore what is not useful to you

Any simple string is just a string, e.g. 
```json
{
  ...
  "year":"2011"
  ...
}
```

Anything more complex is an object, such as 
```json
{
  ...
  "journal":{
      "name": "A really great journal",
      "id": "rgjourn"
  }
  ...
}
```

Anything that could have multiples goes in a list -
```json
{ 
  ...
  "author":[
    {
        "name": "MacGillivray, Mark"
    },
    {
        "name": "Pitman, Jim"
    },
    "lists can contain string, list, and object children"
  ]
  ...
}
```

## Which things are objects

As mentioned in the overview, when something is simple it can just be a string pointed at by a suitable key; but otherwise, it needs to be an object.
So which things are objects?

* author is a list of objects
* editor is a list of objects
* license is a list of objects
* identifier is a list of objects
* link is a list of objects
* journal is an object

## Web links and identifying things

Web links are objects in a list and can contain a URL, an anchor, and more.
They should go to relevant stuff about the current record.

Identifiers should identify the thing the current record is about. 
They must have an ID and a type, and can have more.

```json
{
  "title": "An example of links",
  "link": [
    {
        "url": "http://example.com",
        "anchor": "Go to Example"
    }
  ]
  "identifier": [
    {
        "id": "10.1186/1758-2946-3-47",
        "type": "DOI",
        "url": "http://dx.doi.org/10.1186/1758-2946-3-47"
    }
  ]
}
```

## author, editor, journal

Some typical keys for these objects

```json
{
  "author": [
    {
      "name": "Erdös, Paul",
      "alternate": ["Paul Erdos"],
      "firstname": "Paul",
      "lastname": "Erdös",
      "id": "paulerdos"
    }
  ],
  "editor": [
      ...
  ],
  "journal": {
    "name": "American Journal of Mathematics",
    "shortcode": "Amer. J. Math.",
    "id": "amerjmath"
  }
}
```

You could even have these as separate records in a collection, (use the "type" field to identify "author", say) then refer to them from other objects by their ID.

## Licensing Things

License information is represented by the "license" key.
It should contain a type and a URL to an explanation of the license.
It could also contain other fields giving more info about the license, if necessary.
License is a list, just in case there are multiples...

(We have chosen to Americanise ourselves and use "license" :)

Learn more about bibliographic metadata licensing at [http://openbiblio.net/principles](http://openbiblio.net/principles)

```json
{
  "license": [
    {
      "type": "copyheart",
      "url": "http://copyheart.org/manifesto/",
      "description": "A great license",
      "jurisdiction": "universal"
    }
  ]
}
```

## Collection

An example collection
```json
{
  "metadata": {
    "collection": "my_collection",
    "label": "My collection of records",
    "description" "a great collection",
    "id": "long_complex_uuid",
    "owner": "test",
    "created": "2011-10-31T16:05:23.055882",
    "modified": "2011-10-31T16:05:23.055882",
    "source": "http://webaddress.com/collection.bib",
    "records": 1594,
    "from": 0,
    "size": 2,
  },
  "records": [
    {
      "collection": "my_collection",
      "type": "book",
      "title": "a great book",
      "id": "your_record_id",
      ...
    },
    ...
  ]
}
```

**NOTE:** You can provide your own record IDs in the "id" key.
Internal IDs allocated by BibServer (and other internal data set by BibServer or other processes) are set to keys prefixed with "_" - e.g. "_id" is used for internal BibServer IDs. 
(BibServer also exposes records via the provided as well as the internal ID.)

## Another example

```json
{
  "type": "article",
  "title": "On a family of symmetric Bernoulli convolutions",
  "author": [
    {
      "name": "Erdös, Paul"
    }
  ],
  "journal": {
    "name": "American Journal of Mathematics"
    "identifier":[
      {
        "id": "0002-9327",
        "type": "issn"
      }
    ],
    "volume": "61",
    "pages": "974--976"
  },
  "year": "1939",
  "owner": "me",
  "id": "ID_1",
  "collection": "my_collection",
  "url": "http://example.com/me/my_collection/ID_1",
  "link":[
    {
      "url": "http://okfn.org",
      "anchor": "Open Knowledge Foundation"
    }
  ]
}
```

## Linked Data

Rather than re-defining our own methods for representing linked data, we have adopted the JSON-LD linked data specification.
This enables representation of your data in a BibJSON collection whilst also taking advantage of the power of linked data where necessary, without making the basics of BibJSON overly complex for those that do not require it.
So in order to represent your data as linked data, just incorporate the [JSON-LD](http://json-ld.org) linked data specifications when creating your BibJSON collection.

### JSON-LD Example

We would like to use real world examples as far as possible - do you require linked data functionality within BibJSON?
If so, please [contact us](mailto:openbiblio-dev@lists.okfn.org") and we can work through a BibJSON / JSON-LD example with you.

## Parsing to BibJSON

We have written parsers to BibJSON from a number of popular formats such as bibtex, CSV, RIS, MARC - and we have written BibServer to be easily extensible with additional parsers written in multiple programming languages.
If we do not yet have the parser you require in our repo, get in touch and we will help you write one.

<!--
        <p>The parsers are accessible via an API call at 
        <a href="http://bibsoup.net/parse">http://bibsoup.net/parse</a>, so you can send a 
        source URL to it and get it back as BibJSON.</p>
-->

The parse functionality can be run independently from the core of BibServer, so it is possible to create and expose your own parsers if you wish.

## Schema

BibJSON is intended to be simple yet useful, and should be used as is most practical.
There is no fixed schema as yet, but as JSON-LD is supported it is possible to reference any vocabulary via a namespace declaration and use it with your key/value pairs where necessary.

Development is managed via our [mailing list](mailto:openbiblio-dev@lists.oknf.org) the Open biblio WG blog at [http://openbiblio.net](http://openbiblio.net), and the wiki at [http://wiki.okfn.org/Projects/openbibliography/bibjson](http://wiki.okfn.org/Projects/openbibliography/bibjson).


## Why use / who uses BibJSON

<!--
        <p>You can see lots of examples at <a href="http://bibsoup.net">http://bibsoup.net</a>.
        Just add .json to the URL or &format=json to 
        the parameters (or send a GET request with proper "accept" JSON headers). Try viewing in <a href="">Firefox</a> using 
        <a href="https://addons.mozilla.org/en-US/firefox/addon/jsonview/">JSONview</a>.</p>
-->

BibJSON data can be very easily displayed, searched, embedded, merged and shared on the internet via BibServer and similar tools. With that comes the ability to use your bibliographic metadata directly in online documents to manage, share and link your reference lists - not just maintaining your collection for use in static documents, but using your collections as part of the 

[Let us know](mailto:openbiblio-dev@lists.okfn.org) if you are using or considering using BibJSON for your own project.
It becomes more useful as more people use it.

 ## Anything similar? (why not use them?)

The concept of BibJSON is like that of [GeoJSON](http://geojson.org/).

There are various other ways to represent your data and share it with other people. 
We have designed BibJSON around some key requirements - to be useful on the web, to remain simple, and to present consensus on usage.

We think that the power of linked data is good where necessary, and we support that via our adoption of JSON-LD. We prefer it over others such as RDF/JSON as it enables us to maintain a simple key/value structure where appropriate.

## What do you think?

We would like to know what you think about BibJSON. We would like to find out where it actually is useful for other people.

The best place to get in touch is on our mailing list [openbiblio-dev@lists.okfn.org](mailto:openbiblio-dev@lists.okfn.org).

It would be great to hear back about any potential uses of BibJSON, or any interest in converting records or collections, or in writing parsers to do so.
Also, any follow up comments about the conventions described here, or suggestions for additions / changes, should be presented to the mailing list for consideration.

---

Written by Mark MacGillivray and Jim Pitman, [CC-BY](http://creativecommons.org/licenses/by/3.0/)

BibJSON was initially developed for the Bibliographic Knowledge Network (BKN) project directed by [Jim Pitman](http://www.stat.berkeley.edu/~pitman/), and supported by U.S. National Science Foundation Awards 0835773 and 0835463.
Any opinions, findings, and conclusions or recommendations expressed in this material are those of the authors of this document and do 
not necessarily reflect the views of the National Science Foundation.

* Contact us at [openbiblio-dev@lists.okfn.org](mailto:openbiblio-dev@lists.okfn.org)
* An [OKF](http://okfn.org) [Open Biblio Working Group](http://openbiblio.net) project
* With support from [JISC](http://jisc.ac.uk) via the [Open Biblio 2](http://openbiblio.net/p/jiscopenbib2) project
* As used in [BibServer](http://github.com/okfn/bibserver)
* Read [Open Bibliography for STM](http://www.jcheminf.com/content/3/1/47)

---

Current Website: https://okfnlabs.org/projects/bibjson/

Original Website https://web.archive.org/web/20231127153911/https://okfnlabs.org/bibjson/
