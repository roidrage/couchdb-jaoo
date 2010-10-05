!SLIDE

## ”CouchDB is just a B-Tree and a Transformation Engine.“ ##

<p class="caption">
<a href="http://twitter.com/bitdiddle/status/19645939355">Anonymous Tweeter</a>
</p>

!SLIDE center

# The B-Tree #

![B-Tree](btree.png)

!SLIDE

## All data is stored in a B-tree. ##

!SLIDE

## Data is always appended, ##
## never updated. ##

!SLIDE

# Querying Data #

!SLIDE bullets incremental

* Custom Queries are stored as Views
* Views are written in JavaScript
* Views are part of design documents

!SLIDE bullets incremental

# Views #

* Keys pointing to a value
* Keys can be any valid JSON object
* Values can be documents
* Custom objects too

!SLIDE bullets incremental

# Map/Reduce #

* Map emits the desired lookup keys
* Reduce aggregates the results

!SLIDE 

# Map Phase #

## Find by Name ##

    function(doc) {
      emit(doc.name, doc);
    }

!SLIDE javascript

## Find by tag ##

    function(doc) {
      for (var index in doc.tags) {
        emit(doc.tags[index], 1);
      }
    }

!SLIDE javascript small

## Find by tag ##

    function(keys, values, rereduce) {
      var sum = 0;
      for (var i = 0; i < values.length; i++) {
         sum += values[i];
      }
      return sum;
    }

!SLIDE smaller

## Querying Views ##

    /jaoo/_design/conferences/_view/by_name?key="JAOO 2010"

!SLIDE bullets incremental

# Design Documents #

* Collect map/reduce functions
* Name convention: `_design/<document>`
* Regular JSON document

!SLIDE javascript small

# Design Documents #

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

# Views #

* Are only updated on read
* Are always read from disk
* Are invalidated when design documents are updated
* Can fetch ranges too
