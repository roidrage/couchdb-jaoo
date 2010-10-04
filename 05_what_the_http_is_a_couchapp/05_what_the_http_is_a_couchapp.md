!SLIDE

## What the HTTP is a CouchApp? ##

!SLIDE bullets incremental

## Put all these things together ##

* HTTP
* JavaScript
* Views
* Offline replication

!SLIDE

# CouchApps #

## Web Applications served with CouchDB ##

!SLIDE bullets incremental

## CouchApps ##

* Talk to the database directly (HTTP!)
* Use JavaScript for the most part
* Can be run anywhere
* Can sync anywhere, with their data

!SLIDE bullets incremental

## CouchApps ##

* Are design documents

!SLIDE javascript

## Design Documents ##

    @@@ javascript
    {
      "_id": "_design/conferences"
      "views": {
        "by_name": {
          "map": "function(doc)...",
          "reduce": "function(keys, values, rereduce)"
        }
      }
    }

!SLIDE bullets incremental

## There's more to design documents ##
## than meets the eye. ##

!SLIDE bullets incremental

* Validation functions
* Show functions
* List functions
* Attachments: CSS, JavaScript, HTML, Images

!SLIDE bullets incremental

## Validation functions ##

* Validate documents before saving
* Can prevent a write to the database

!SLIDE javascript smaller

## Validation functions ##

    @@@ javascript
    function(newDocument, savedDocument, userCtx) {
      function require(field, message) {
        message = message || "Document must have a " + field;
        if (!newDocument[field]) throw({forbidden : message});
      };
      
      if (newDocument.type == 'conference') {
        require("name");
      }
    }

!SLIDE bullets incremental

## Show functions ##

* Transform documents
* Into anything
* E.g. HTML

!SLIDE javascript small

## Show functions ##

    @@@ javascript
    function(doc, request) {
      return '<h1>' + doc.name + '</h1>';
    }

!SLIDE javascript small

## Show functions can manipulate the response ##

    @@@ javascript
    function(doc, request) {
      return {
        body: '<h1>' + doc.name + '</h1>',
        headers: {
          'Content-Type': 'application/xml'
        }
      }
    }

!SLIDE smaller

## Accessing show views ##

    GET /jaoo/_design/conferences/_show/title/e51a33d1bf9...

!SLIDE bullets incremental

## List functions ##

* Transform view results
* Filter view results
* Into e.g. CSV, XML, HTML

!SLIDE javascript smaller

## List functions ##

    @@@ javascript
    function(header, request) {
      var row;
      send('<html><head><title>Conferences</title></head><body>')
      while (row = getRow()) {
        send('<h1>' + row.value.name + '</h1>')
      }
      send('</body></html>')
    }

!SLIDE code small

    GET /jaoo/_design/conferences/_list/title/by_name?key="JAOO 2010"

!SLIDE

## So seriously, what's a CouchApp? ##

!SLIDE

## Just another CouchDB document. ##

!SLIDE bullets incremental

## Combines ##

* Views
* HTML templates
* Show, list, validate functions
* Assets

!SLIDE

## Single design document. ##

!SLIDE javascript smaller

    @@@ javascript
    {
      "lists": {
        "title": "function(header, request) {}"
      },
      "shows": {
        "title-row", "function(doc, request) {}"
      },
      "validate_doc_update":
          "function(newDocument, savedDocument, userCtx)"
    }

!SLIDE bullets incremental

## Embed CommonJS Modules ##

# TODO #
