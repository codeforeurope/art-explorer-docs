---
layout: page
---
### Introduction

Welcome to the Manchester Art Gallery Open Data API (beta). This API provides access to Manchester Art Gallery's collection of works database in computer-readable [JSON] format. It is intended for software developers who wish to build tools and apps which xx, yy and zz.

API access is provided on a "fair use" basis, and does not require an access key for its use. All data is copyright Manchester Art Gallery 2014 unless stated otherwise, and is made available under a permissive [FOO] license.

Please let us know if you are building anything exciting with it, and if you have any helpful comments or suggestions then get in touch at [bob@example.com](mailto:bob@example.com).

### API Overview

Data is returned in JSON format, either raw or in a "padded JSON" ([JSON-P]) wrapper for ease of development within a browser environment. To make a call with JSONP, supply a callback attribute on the querystring e.g. `GET http://data.manchestergalleries.asacalow.me/i/1918.370?callback=foo`. JSONP is also supported by front-end javascript libraries such as [JQuery].

#### End points

The API provides two endpoints for retrieving works data:

  - __Search:__ Is a fully-featured data search – offering keyword search, sorting, filtering and an option to return additional supporting keywords which can be used to further filter results.
  - __Collection Item:__ Returns data on a single item in the collection, based on its accession record.

### API Documentation

#### Collection Search

A call to `GET /search` will return a paginated list of resources in JSON format, given the following parameters:

*Parameter*|*Info*
q (required) | A valid search query e.g. ford maddox brown
p | An optional page number (default is 0)
pp | An optional page size (default is 10)

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

#### Get an individual record

A call to `GET /i/:id`, replacing ':id' with a valid accession number, will return details of a specific work.

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

[JSON]: http://json.org
[JSONP]: http://json-p.org/
[JQuery]: https://jquery.org/
[FOO]: http://example.com
