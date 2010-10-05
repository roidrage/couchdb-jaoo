!SLIDE

# Goodies #

!SLIDE

# Changes feed #

## Real time updates as they occur ##

!SLIDE smaller

    GET /jaoo/_changes?feed=continuous

    {"seq":17,"id":"eb28b751a33d1bf9d7dfffd670001965",
     "changes":[{"rev":"1-967a00dff5e02add41819138abb3284d"}]}
    {"seq":27,"id":"_design/conferences",
     "changes":[{"rev":"20-764d764cb658fbe2dd63ff66ac2eb609"}]}
    {"seq":28,"id":"eb28b751a33d1bf9d7dfffd670001965",
     "changes":[{"rev":"2-7ae4eafde29bc3d75a19f1b922d08e21"}]}

!SLIDE bullets incremental

# Uses for Changes Feed #

* Enables continuous replication
* Update a UI as new data comes in
* Invalidate caches
* Update search indexes

!SLIDE bullets incremental

# Filters #

* Can filter updates from the changes feed
* Read only what you're interested in
* Read only what you're allowed to read

!SLIDE

# Filters #

    function(doc, req) {
      if(doc.type == 'conference') {
        return true;
      } else {
        return false;
      }
    }

!SLIDE small

## Filters are part of design documents. ##

    {
      "_id": "_design/conferences",
      "filters": {
        "only-conferences": "function(doc, req) {...}"
      }
    }

!SLIDE smaller

    GET /jaoo/_changes?filter=conferences/only-conferences

!SLIDE bullets incremental

# Update Handlers #

* Update documents without fetching them

!SLIDE 

# Update Handlers #

    function(doc, req) {
      var field = req.form.field;
      var value = req.form.value;
      var message = "set "+field+" to "+value;
      doc[field] = value;
      return [doc, message];
    }

!SLIDE 

# Update Handlers #

## Yep, they also go in the design document. ##

    {
      "_id": "_design/conferences",
      "updates": {
        "in-place": "function(doc, req) {...}"
      }
    }

