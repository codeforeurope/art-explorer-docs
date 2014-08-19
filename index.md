---
layout: page
---
### Introduction

Welcome to the Manchester Art Gallery Open Data API (beta). This API provides access to [Manchester Art Gallery]'s collection of works database in computer-readable [JSON] format. It is intended for software developers who wish to build tools and apps which xx, yy and zz.

API access is provided on a "fair use" basis, and does not require an access key for its use. All data is copyright Manchester Art Gallery 2014 unless stated otherwise, and is made available under a permissive [FOO] license.

Please let us know if you are building anything exciting with it, and if you have any helpful comments or suggestions then get in touch at [bob@example.com](mailto:bob@example.com).

### API Overview

Data is returned in JSON format, either raw or in a "padded JSON" ([JSON-P]) wrapper for ease of development within a browser environment. To make a call with JSON-P, supply a callback attribute on the querystring e.g. `GET http://data.manchestergalleries.asacalow.me/i/1918.370?callback=foo`. JSON-P is also supported by front-end javascript libraries such as [JQuery].

#### End points

The API provides two endpoints:

  - __Search:__ Is a fully-featured record search – offering keyword search, sorting, filtering and an option to return additional supporting keywords which can be used to further filter results.
  - __Collection Item:__ Returns data on a single item in the collection, based on its accession record.

### API Documentation

#### Collection Search

A call to `GET /search` will return a paginated list of resources in JSON format, given the following parameters:

*Parameter* | *Allowed characters/values* | *Info*
q (required) | A-Z, 0-n, *, ", ' | A valid search query e.g. "ford madox brown" (__NB:__ can use [Elasticsearch Query Syntax](EQS))
p | 1-n | An optional page number (default is 1)
pp | 1-100 | An optional page size (default is 10)
f | _type_, _subject_ and/or _creator_ (comma-separated) | Return facets for the given field e.g. "show me the most common creators for the given search"
s | Any field name (e.g. _identifier_) | The field to sort results by (default ascending)
so | either _asc_ or _desc_ | Sort order, ascending (e.g. A-Z, 0-9) or descending (e.g. Z-A, 9-0)
type | A-Z | Filter by type of work, e.g. "oil on canvas"
subject | A-Z | Filter by subject, e.g. "british painting"
creator | A-Z | Filter by name of creator, e.g. "Goya"
earliest | 1-3000 | Return results since the given year e.g. 1880
latest | 1-3000 | Return results up to the given year e.g. 1900
i | _true_ or _false_ | Return results which have a web-accessible image only (default false)

Records with associated images have an `images` attribute, containing an array of partial JPEG image paths. These partial paths can be used to build the following fully qualified URLs:

For __thumbnail__ images (up to 320px width/height), complete the path like so:

    http://data.manchestergalleries.asacalow.me/assets/images/thumb/<path>

For __full-size__ images (up to 1200px width/height), complete the path like this:

    http://data.manchestergalleries.asacalow.me/assets/images/full/<path>

##### Example Usage

Request:

    http://data.manchestergalleries.asacalow.me/search?q=portrait

Response:

    {
      "total":5271,
      "items":[
        {
          "identifier":"LI2010.59.4",
          "title":"Studio Portrait Lighting",
        } , {
          "identifier":"1192.23",
          "title":"Family Portrait",
          "creator":"Matisse",
          "subject": [ "fine art", "reproduction" ],
        } , {
          "identifier":"PAWM090",
          "title":"Abel Heywood",
          "creator":"Joseph W. Swynnerton",
          "description":"Portrait bust",
          "subject": [ "public monuments", "monument", "portrait" ],
          "earliest":1880,
          "latest":1880,
          "type":["carved"]
        },
        …
      ]
    }

#### Retrieve an individual record

A call to `GET /i/:id` (replacing ':id' with a valid accession number) will return details of a specific work.

##### Example Usage

Request:

    http://data.manchestergalleries.asacalow.me/i/LI2010.34.10

Response:

    {
      "identifier":"LI2010.34.10",
      "title":"Sheet of Studies for 'Work'",
      "creator":"Ford M. Brown",
      "subject":["exhibition"],
      "earliest":1862,
      "latest":1862,
      "type": ["drawing"],
      "coverage": {
        "placeName": "unframed, 12.4, 17.5, cm, framed, 31, 43.7, cm"
      }
    }

[Manchester Art Gallery]: http://manchestergalleries.org
[JSON]: http://json.org
[JSON-P]: http://json-p.org/
[JQuery]: https://jquery.org/
[FOO]: http://example.com
[EQS]: http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax
