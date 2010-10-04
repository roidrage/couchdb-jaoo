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
* Can sync their data anywhere

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

!SLIDE javascript

## Show functions ##

    @@@ javascript
    function(doc, request) {
      return '<h1>' + doc.name + '</h1>';
    }

!SLIDE javascript

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

!SLIDE bullets incremental

## List functions ##

* Transform view results
* Filter view results
* Into e.g. CSV, XML, HTML

!SLIDE

## So seriously, what's a CouchApp? ##

!SLIDE

## Just another CouchDB document. ##

!SLIDE

## Combines ##

* Views
* HTML templates
* Show, list, validate functions
* Assets

!SLIDE

## Single design document. ##
