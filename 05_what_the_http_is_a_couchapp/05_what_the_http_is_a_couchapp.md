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

!SLIDE

## Design Documents ##

    {
      "_id": "_design/conferences"
      "views": {
        "by_name": {
          "map": "function(doc)...",
          "reduce": "function(keys...)"
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

!SLIDE smaller

## Validation functions ##

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

!SLIDE small

## Show functions ##

    function(doc, request) {
      return '<h1>' + doc.name + '</h1>';
    }

!SLIDE small

## Show functions can manipulate the response ##

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

!SLIDE smaller

## List functions ##

    function(header, request) {
      var row;
      start(headers:{'Content-Type': 'text/html'})
      send('<html><body>')
      while (row = getRow()) {
        send('<h1>' + row.value.name + '</h1>')
      }
      send('</body></html>')
    }

!SLIDE code smaller

    GET
    /jaoo/_design/conferences/_list/title/by_name\
          ?key="JAOO 2010"

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

!SLIDE smaller

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

