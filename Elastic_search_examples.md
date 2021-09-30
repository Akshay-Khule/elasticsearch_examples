GET _search
{
  "query": {
    "match_all": {}
  }
}

GET _alias

POST user/_create/1
{
  "name": "Akshay",
  "job": "software developer"
}

POST user/_create/2
{
  "name": "Neha",
  "job": "software Testing"
}

GET user/_doc/4

GET user/_count

DELETE employee{

PUT user/_doc/2
{
  "name": "Neha",
  "job": "software Testing 2"
}

PUT user/_doc/3
{
  "name": "Neha Jr",
  "job": "software Testing 3"
}

PUT user/_doc/4
{
  "name": "Akshay Jr",
  "job": "software dev"
}

POST user/_doc/4
{
  "name": "Akshay Jr",

  "job": "software dev"
}

GET _search?q=Neha

GET _search?q=name:Neha

POST employee/_doc/1
{
  "Name": "AK",
  "DoB": "2020-12-10",
  "Job": "Developer"
}
POST employee/_doc/2
{
  "Name": "AS",
  "DoB": "2020-12-10",
  "Job": "Test"
}

PUT employee
{
  "mappings" :{
    "properties" : {
      "DoB": {
        "type" : "date"
      },
      "Job": {
        "type" : "text",
        "index":false
      }
    }
  }
}

GET _search?q=Job:Test
GET employee/_doc/1
DELETE employee/_doc/1

DELETE employee

PUT employee_analyzer
{
  "mappings" :{
    "properties" : {
      "DoB": {
        "type" : "date"
      },
      "Job": {
        "type" : "text",
        "analyzer":"standard"
      }
    }
  }
}

POST employee_analyzer/_doc/1
{
  "name": "Ajit",
  "DoB" : "2020-12-10",
  "Job" : "My job is good !! hmm"
}

GET employee_analyzer/_doc/1

PUT products
{
  "settings": {
    "number_of_shards": 2,
    "number_of_replicas": 2
  }
 
}

POST products/_doc/1
{
  "name": "cofee",
  "price" : 10,
  "instock" : 5
}

GET products/_doc/1

#how to update above document?
PUT /products/_update/1
{
  "docs" :{
    "price": 12,
    "instock": 3
  }
}

POST /products/_update/1
{
  "doc": {
    "in_stock": 6
  }
}

POST /products/_update/1
{
  "doc":{
    "tags" :["electronics"]
  }
}

POST products/_update/1
{
  "script": {
    "source": "ctx._source.in_stock = 10"
  }
}

POST products/_update/1
{
  "script": {
    "source": "ctx._source.remove(\"instock\")"
  }
}

POST products/_update/1
{
  "script": {
    "source": """
    if(ctx._source.in_stock >0){
       ctx._source.in_stock--;
    }
    """
  }
}


##=====upsert
POST products/_update/2
{
  "script": {
    "source": "ctx._source.in_stock++"
  },
   "upsert": {
    "name": "cofee",
    "price" : 10,
    "in_stock" : 5
  }
}



GET products/
DELETE products/_doc/2
##--updatebyquery

POST products/_update_by_query
{
  "script": {
    "source": "ctx._source.in_stock--"
  },
   "query":{
    "match_all": {}
  }
 
}

POST products/_delete_by_query
{
  "query":{
    "match_all": {}
  }
}
#============






POST /_bulk
{ "index": { "_index": "products", "_id": 200 } }
{ "name": "Espresso Machine", "price": 199, "in_stock": 5 }
{ "index": { "_index": "products", "_id": 201 } }
{ "name": "Espresso Machine2", "price": 200, "in_stock": 6 }

GET /products/_search
{
  "query":{
    "match_all":{}
  }
}

POST /products/_bulk
{ "update": { "_id": 200 } }
{ "doc": { "price": 129 } }
{ "delete": { "_id": 200 } }


POST /my-index-000001/_doc/1
{
  "region":             "US",
  "manager.age":        30,
  "manager.name.first": "John",
  "manager.name.last":  "Smith"
}
#===================
PUT my-index-000002
{
  "mappings": {
    "properties": {
      "user": {
        "type": "nested"
      }
    }
  }
}
#The nested type is a specialised version of the object data type that allows arrays of objects to be indexed in a way that they can be queried independently of each other.

PUT my-index-000001/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}

GET my-index-000001
#===========

PUT my-index-003
{
  "mappings": {
    "properties": {
      "name" :{
        "type": "text"
      }
     
    }
  }
}

PUT my-index-003/_doc/1
{
  "name": "My name is Akshay"
}

GET my-index-003/_search
{
  "query": {
    "match": {
      "name": "Akshay"
    }
  }
}
#===============


PUT my-index-004
{
  "mappings": {
    "properties": {
      "name":{
        "type": "keyword"
      }
    }
  }
}



PUT my-index-004/_doc/1
{
  "name": "My name is Akshay"
}


GET my-index-004/_search
{
  "query": {
    "match": {
      "name": "My name is Akshay"
    }
  }
}
#==============
GET my-index-000001/_mapping



PUT my-index-01/_doc/1
{
  "employee.name":"akshay",
  "employee.age":"31",
  "employee.dept.unit":"comp"
}

GET my-index-01/_mapping

#Add extra mapping to current index

PUT my-index-01/_mapping
{
    "properties": {
      "employee.doj":{
        "type": "date"
      }
    }
}
#This way we can add additional element into current index
GET my-index-01/_mapping

PUT my-index-01/_doc/2
{
  "employee.name":"Akshay",
  "employee.age":"31",
  "employee.dept.unit":"comp",
  "employee.salary":"2000"
}

PUT my-index-01/_doc/3
{
  "employee.name":"Amit",
  "employee.age":"31",
  "employee.dept.unit":"comp",
  "employee.salary":"2000",
  "employee.doj": "2020-04-04"
}
#even if u not give mapping for all documents then also u can add new field at any time as above

GET my-index-01/_search
{
  "query": {
    "match": {
      "employee.name": "Akshay"
    }
  }
}
#===============

PUT my-index-01/_mapping
{
    "properties": {
      "employee.doj":{
        "type": "date",
        "format":"dd-MM-YYYY"
      }
    }
}

GET my-index-01

#with this way you can define format of perticular field.
#=========================================================
#You can not change/update existing mapping as below, from text to keyword now allow
PUT my-index-005
{
  "mappings": {
    "properties": {
      "name":{
        "type": "text"
      }
    }
  }
}

PUT my-index-005
{
  "mappings": {
    "properties": {
      "name":{
        "type": "keyword"
      }
    }
  }
}
#===

PUT new-my-index-000001
{
  "mappings" : {
      "properties" : {
        "group" : {
          "type" : "text",
          "fields" : {
            "keyword" : {
              "type" : "keyword",
              "ignore_above" : 256
            }
          }
        },
        "manager" : {
          "properties" : {
            "age" : {
              "type" : "long"
            },
            "name" : {
              "properties" : {
                "first" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                },
                "last" : {
                  "type" : "text",
                  "fields" : {
                    "keyword" : {
                      "type" : "keyword",
                      "ignore_above" : 256
                    }
                  }
                }
              }
            }
          }
        },
        "region" : {
          "type" : "text"

        },
        "user" : {
          "properties" : {
            "first" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            },
            "last" : {
              "type" : "text",
              "fields" : {
                "keyword" : {
                  "type" : "keyword",
                  "ignore_above" : 256
                }
              }
            }
          }
        }
      }
    }
}


POST /_reindex
{
  "source": {
    "index": "my-index-000001"
  },
  "dest": {
    "index": "new-my-index-000001"
  },
  "script": {
    "source": """
      if (ctx._source.product_id != null) {
        ctx._source.product_id = ctx._source.product_id.toString();
      }
    """
  }
}
#with the help of ctx , while migrating data from one index to other we can modify

GET new-my-index-000001/_search
{
  "query": {
    "match_all": {}
  }
}

##======================================

PUT /_template/access-logs/
{
  "index_patterns": ["access-logs*"],
 
  "mappings": {
    "properties": {
      "time":{
        "type": "date"
      },
      "logs":{
        "type": "text"
      }
     
    }
  }
}


PUT /access-logs-2020-01-01/_doc/2
{
  "time": "2020-12-23",
  "logs" : "info"
}

GET /access-logs-2020-01-01



GET /access-logs-2020-01-01/_search
{
  "query": {
    "match_all": {}
  }
}

##==========This will help to create index automatically


POST datatype/_doc
{
  "name": "Akshay",
  "id": 2,
  "date": "2020-12-12"
}

GET datatype
#=====id is mapped with long value and date with data data type

PUT /people
{
  "mappings": {
    "dynamic": "strict",
    "properties": {
      "first_name": {
        "type": "text"
      }
    }
  }
}

DELETE people

POST /people/_doc
{
  "first_name": "Bo",
  "last_name": "Andersen"
}
#with this if we get rest call with last_name as field then it will throw error

GET /people/_search
{
  "query": {
    "match_all": {}
  }
}

POST number/_doc
{
  "numberid":12
}
GET number/_mapping
#By default elastic serach create logs as data type , if we need to change this to int, then we can use dynamic_template_test

PUT /number3
{
  "mappings": {
    "dynamic_templates": [
      {
        "integers": {
          "match_mapping_type": "long",
          "mapping": {
            "type": "integer"
          }
        }
      }
    ]
  }
}

POST /number3/_doc
{
  "in_stock": 123
}
GET number3/_mapping

PUT my-index-1234
{
  "mappings": {
    "properties": {
      "status_code": {
        "type":       "keyword"
      },
      "session_id": {
        "type":       "keyword",
        "doc_values": false
      }
    }
  }
}
#If you are sure that you don’t need to sort or aggregate on a field, or access the field value from a script, you can disable doc values in order to save disk spac

#The index option controls whether field values are indexed. It accepts true or false and defaults to true. Fields that are not indexed are not queryable.

PUT my-index-23456
{
  "settings": {
    "analysis": {
      "analyzer": {
        "my_english_analyzer_new": {
          "type": "standard",
          "max_token_length": 5,
          "stopwords": "_english_"
        }
      }
    }
  }
}
#1cFirst you will create an index with some custom analyzer


POST my-index-234/_analyze
{
  "analyzer": "my_english_analyzer_new",
  "text": "The 2 QUICK Brown-Foxes jumped over the lazy dog's bone."
}
#2 Then analyze it

POST my-index-234/_doc
{
 "mappings": {
    "properties": {
      "name":{
        "type": "text",
        "analyzer": "my_english_analyzer_new"
      }
    }
 }
}
#3 Then using mapping  post data in index

POST my-index-234/_doc
{
  "name": "help meeeeeeeeeeeeeee"
}

GET my-index-234/_search
{
  "query": {
    "match_all": {}
  }
}

PUT employee1
{
  "mappings": {
    "properties": {
      "dob":{
        "type": "date"
      },
      "jobdescription":{
        "type": "text",
        "analyzer": "simple"

      }
    }
  }
}


####*****you can specify analyzer at  field, index, or query level and why we are specifying analzer.. answer is https://www.youtube.com/watch?v=9VhTnWuely4

POST my-custom/_doc
{
  "name": "hello how are you, all GOOD. YYYYYYY"
}

GET my-custom/_search
{
  "query": {
    "match": {
      "name": "GOOD"
    }
  }
}

POST my-custom/_analyze
{
  "name":"hello how are you, all GOOD. YYYYYYYAA"
}


PUT employee1/_doc/1
{
  "dob":"2020-12-12",
  "jobdescription": "job 2 pm to 10 pm"
}

GET employee1/_search
{
  "query": {
    "match": {
      "jobdescription": "job"
    }
  }
}

PUT employee5/_doc/1
{
  "dob":"2020-12-12",
  "jobdescription": "Software Engg"
}

###############match query
GET employee5/_search
{
  "query": {
    "match": {
      "jobdescription": "engineer"
    }
  }
}

POST _bulk
{ "index" : { "_index" : "test_bulk", "_id" : "1" } }
{"name":"Akshay", "job_description":"computer engg"}
{ "index" : { "_index" : "test_bulk", "_id" : "2" } }
{"name":"Neha", "job_description":"computer test"}
{ "update" : {"_id" : "1", "_index" : "test_bulk"} }  
{ "doc" : {"name" : "Akshay 3.0"} }
{ "delete" : { "_index" : "test_bulk", "_id" : "1" } }


POST _bulk
{ "index" : { "_index" : "test_query", "_id" : "1" } }
{"name":"Akshay", "job_description":"Akshay"}
{ "index" : { "_index" : "test_query", "_id" : "2" } }
{"name":"Neha", "job_description":"computer test"}
{ "index" : { "_index" : "test_query", "_id" : "3" } }
{"name":"Anay", "job_description":"computer test"}
{ "index" : { "_index" : "test_query", "_id" : "2" } }
{"name":"Ak", "job_description":"computer test"}

GET test_query/_search
{
  "query": {
    "multi_match": {
      "query": "Akshay",
      "fields": ["name","job_description"]
    }
  }
}
#######termqueries
PUT termqueries
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "job_description":{
        "type": "keyword"
      }
    }
  }
}

POST _bulk
{ "index" : { "_index" : "termqueries", "_id" : "1" } }
{"name":"Akshay", "job_description":"Software Engg"}
{ "index" : { "_index" : "termqueries", "_id" : "2" } }
{"name":"Neha", "job_description":"Software testing"}

GET termqueries/_search
{
  "query": {
    "term": {
      "job_description": {
        "value": "Software testing"
      }
    }
  }
}

GET termqueries/_search
{
  "query": {
    "term": {
      "job_description": {
        "value": "Software testing"
      }
    }
  }
}

POST _bulk
{ "index" : { "_index" : "test_bool", "_id" : "1" } }
{"name":"Akshay", "job_description":"Software Engg", "salary": 2000}
{ "index" : { "_index" : "test_bool", "_id" : "2" } }
{"name":"Amit", "job_description":"Software test", "salary": 1000}
{ "index" : { "_index" : "test_bool", "_id" : "3" } }
{"name":"Ajit", "job_description":"Vice President", "salary": 3000}
{ "index" : { "_index" : "test_bool", "_id" : "4" } }
{"name":"sachin", "job_description":"Vice", "salary": 4000}
{ "index" : { "_index" : "test_bool", "_id" : "5" } }
{"name":"Neha", "job_description":"Teacher", "salary": 10000}
{ "index" : { "_index" : "test_bool", "_id" : "6" } }
{"name":"Any", "job_description":"Doctor", "salary": 2000000}

GET test_bool/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "range": {
            "salary": {
              "gte": 1000
            }
          }
        }
      ]
    }
  }
}

GET test_bool/_search
{
  "query": {
    "bool": {
      "should": [
          {"match": {
            "name": "Amit"
          }},
          {
            "match": {
              "salary": "2000"
            }
          }
         
     ]
    }
  }
}

#Paginatation

GET test_bool/_search
{
  "from": 0,
  "size": 3,
  "query": {
    "match_all": {}
  }
}

GET test_bool/_search?scroll=1m
{
  "size": 2,
  "query": {
    "match_all": {}
  }
 
}

GET /_search/scroll
{
  "scroll":"1m",
  "scroll_id" :"FGluY2x1ZGVfY29udGV4dF91dWlkDXF1ZXJ5QW5kRmV0Y2gBFjk1bWFqZ04tVFV1amk0NUlka1pzU0EAAAAAAAPKTBZwNm5EbTdOX1R4S0o4dVpBZDM2b3R3"
}