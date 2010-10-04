!SLIDE

## ”CouchDB is built of the Web“ ##
<p class="caption">
<a href="http://jacobian.org/writing/of-the-web/">Jacob Kaplan-Moss</a>
</p>

!SLIDE bullets incremental

# CouchDB and The Web #

* Speaks HTTP
* Uses JSON for data transport
* RESTful API

!SLIDE code smaller

# Everything is HTTP #

    GET /jaoo

    HTTP/1.1 404 Object Not Found
    Server: CouchDB/1.0.0 (Erlang OTP/R13B)
    Date: Sun, 03 Oct 2010 18:26:03 GMT
    Content-Type: text/plain;charset=utf-8
    Content-Length: 44
    Cache-Control: must-revalidate
     
    {"error":"not_found","reason":"no_db_file"}

!SLIDE code smaller

# Everything is HTTP #

    PUT /jaoo

    HTTP/1.1 201 Created
    Server: CouchDB/1.0.0 (Erlang OTP/R13B)
    Location: http://localhost:5984/jaoo
    Date: Sun, 03 Oct 2010 18:27:14 GMT
    Content-Type: text/plain;charset=utf-8
    Content-Length: 12
    Cache-Control: must-revalidate
     
    {"ok":true}

!SLIDE code smaller

# Everything is HTTP #

    GET /jaoo

    HTTP/1.1 200 OK
    ...

    {"db_name":"jaoo","doc_count":0,"doc_del_count":0,
    "update_seq":0,"purge_seq":0,"compact_running":false,
    "disk_size":79,"instance_start_time":"1286130434654709",
    "disk_format_version":5}

!SLIDE code smaller

# Everything is a Resource #

    PUT /jaoo/eb28b751a33d1bf9d7dfffd6700006d5

    HTTP/1.1 201 Created
    Server: CouchDB/1.0.0 (Erlang OTP/R13B)
    Location: http://localhost:5984/jaoo/2010
    Etag: "1-6f773089b853fd5f7867562b179a854a"
    Date: Sun, 03 Oct 2010 19:32:38 GMT
    Content-Type: text/plain;charset=utf-8
    Content-Length: 67
    Cache-Control: must-revalidate

    {"ok":true,"id":"eb28b751a33d1bf9d7dfffd6700006d5",
    "rev":"1-6f773089b853fd5f7867562b179a854a"}
    
!SLIDE

## Documents are Data ##

!SLIDE code javascript small

    @@@ javascript
    {
      "_id": "eb28b751a33d1bf9d7dfffd6700006d5",
      "_rev": "1-6f773089b853fd5f7867562b179a854a",
      "name": "JAOO 2010",
      "type": "conference",
      "starts_at": "2010/10/03 09:00:00 +0000",
      "ends_at": "2010/10/08 17:00:00 +0000",
      "tags": ["software", "agile", "cloud"]
            
    }

!SLIDE

## Documents can have Attachments ##

!SLIDE code javascript small

    @@@ javascript
    {
      "_id": "eb28b751a33d1bf9d7dfffd6700006d5",
      ...
      "_attachments": {
        "logo.gif": {
          "content_type": "image/gif",
            "revpos": 3,
            "length": 1929,
            "stub": true
        }
      }
    }

!SLIDE smaller 

## Attachments are Resources too ##

!SLIDE code smaller

    GET /jaoo/eb28b751a33d1bf9d7dfffd6700006d5/logo.gif
