---
layout: page
---
### Introduction

Welcome to the Manchester Art Gallery Open Data API (beta). This API provides access to [Manchester Art Gallery]'s collection of works database in computer-readable [JSON] format. It is intended for software developers who wish to build tools and apps which xx, yy and zz.

API access is provided on a "fair use" basis, and does not require an access key for its use. All data is copyright Manchester Art Gallery 2014 unless stated otherwise, and is made available under a permissive [FOO] license.

Please let us know if you are building anything exciting with it, and if you have any helpful comments or suggestions then get in touch at [bob@example.com](mailto:bob@example.com).

### API Overview

Data is returned in JSON format, either raw or in a "padded JSON" ([JSON-P]) wrapper for ease of development within a browser environment. To make a call with JSON-P, supply a callback attribute on the querystring e.g. `GET http://data.manchesterartgallery.org/i/1918.370?callback=foo`. JSON-P is also supported by front-end javascript libraries such as [JQuery].

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

Returned collection records have the following fields. Where there is no data available for a particular record field, the field will not be present in the returned result.

*Field name* | *Example Value* | *Info*
identifier | 1192.23 | A unique identifier for the record
description | "Portrait of a bust" | The full description of the item
earliest | 1920 | The earliest possible year on which the item will have been created
latest | 1920 | The latest possible year on which the item will have been created. __NB:__ when the earliest and latest years are the same, the creation year is generally known.
creator | "Alphonse Legros" | The name of the item's creator
title | "Portrait of a man" | The item's title
type | ["carved", "sculpture"] | A collection of descriptive types for the item
subject | ["costume", "women", "headwear", "period: 1920s"] | A set of tags associated with the item.
coverage | {"placename": "Europe, United Kingdom, England, London" } | A geographical area associated with the record – just a summary placename for now.
images | [{'path': '1/2/3.jpg', 'acknowledgement': '© Manchester City Galleries'}] | A collection of images of the item. Each image has a path (used to retrieve either a thumbnail or a full-sized image, see below) and an image rights acknowledgement. The full acknowledgement *must* be displayed alongside an image when used.
acknowledgement | "© Manchester City Galleries" | An artistic/design rights acknowledgement for the item. *Must* be displayed in full alongside any presentation of an item's detail, e.g. where the title, description etc. are shown together.
location | "Manchester City Art Gallery" | If on display, the current public display location of the item.
acquisition_credit | "Transferred from the Royal Manchester Institution" | A credit to display for how/where the item was acquired for the collection. *Must* be displayed in full alongside any presentation of an item's detail, e.g. where the title, description etc. are shown together.

Records with associated images have an `images` attribute, containing an array of partial JPEG image paths. These partial paths can be used to build the following fully qualified URLs:

For __thumbnail__ images (up to 320px width/height), use the following pattern: `http://data.manchesterartgallery.org/assets/images/thumb/<path>`.  For example, given a path of "1/2/3.jpg" the full thumbnail image URL would be:

    http://data.manchesterartgallery.org/assets/images/thumb/1/2/3.jpg

For __full-size__ images (up to 1200px width/height), use the following pattern: `http://data.manchesterartgallery.org/assets/images/full/<path>`. For example, given a path of "1/2/3.jpg", the full image URL would be:

    http://data.manchesterartgallery.org/assets/images/full/1/2/3.jpg

#### Example Search API Usage

Request:

    http://data.manchesterartgallery.org/search?q=portrait

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

    http://data.manchesterartgallery.org/i/LI2010.34.10

Response:

    {
      "identifier":"LI2010.34.10",
      "title":"Sheet of Studies for 'Work'",
      "creator":"Ford M. Brown",
      "subject":["exhibition"],
      "earliest":1862,
      "latest":1862,
      "type": ["drawing"]
    }

[Manchester Art Gallery]: http://manchestergalleries.org
[JSON]: http://json.org
[JSON-P]: http://json-p.org/
[JQuery]: https://jquery.org/
[FOO]: http://example.com
[EQS]: http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/query-dsl-query-string-query.html#query-string-syntax
