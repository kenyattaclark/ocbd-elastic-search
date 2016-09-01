## Install Docker

## Optional change host file to have Docker IP

## Start Elasticsearch running in Docker
`docker run -d -p 9200:9200 -p 9300:9300 elasticsearch`

## Show that it is running
`curl -XGET 'http://elasticsearch:9200?pretty=true'`

## Create a record in the index twitter in the type user
`curl -XPUT 'http://elasticsearch:9200/twitter/user/kenyattaclark?pretty' -d '{ "name" : "Kenyatta Clark" }'`

## Create another record in the index twitter in the type tweet
`
curl -XPUT 'http://elasticsearch:9200/twitter/tweet/1?pretty' -d '
{
    "user": "kenyattaclark",
    "post_date": "2016-09-01T13:12:00",
    "message": "Trying out Elasticsearch, so far so good?"
}'
`
## Create another record in the index twitter in the type tweet
`
curl -XPUT 'http://elasticsearch:9200/twitter/tweet/2?pretty' -d '
{
    "user": "kenyattaclark",
    "post_date": "2016-09-01T13:12:00",
    "message": "Another tweet, will it be indexed?"
}'
`
## Get the user record by id
`
curl -XGET 'http://elasticsearch:9200/twitter/user/kenyattaclark?pretty=true'
`
## Get the tweet record by id
`
curl -XGET 'http://elasticsearch:9200/twitter/tweet/1?pretty=true?pretty=true'
`

## Get the tweet record by id
`
curl -XGET 'http://elasticsearch:9200/twitter/tweet/2?pretty=true'
`
## Search across all indices
`
curl -XGET 'http://elasticsearch:9200/_search?pretty=true'
`
## Search across the twitter index
`
curl -XGET 'http://elasticsearch:9200/twitter/_search?pretty=true'
`
## Search across the user type in the twitter index
`
curl -XGET 'http://elasticsearch:9200/twitter/user/_search?pretty=true'
`
## Delete an index
`
curl -XDELETE 'http://elasticsearch:9200/twitter?pretty=true'
`
## Search using a querystring query
`
curl -XGET 'http://elasticsearch:9200/twitter/tweet/_search?q=user:kenyattaclark&pretty=true'
`
`
curl -XGET 'http://elasticsearch:9200/twitter/tweet/_search?pretty=true' -d '
{
    "query" : {
        "match" : { "user": "kenyattaclark" }
    }
}'
`
## Create mapping
`
curl -XPUT "http://elasticsearch:9200/sports/" -d'
{
   "mappings": {
      "athlete": {
         "properties": {
            "birthdate": {
               "type": "date",
               "format": "dateOptionalTime"
            },
            "location": {
               "type": "geo_point"
            },
            "name": {
               "type": "string"
            },
            "rating": {
               "type": "integer"
            },
            "sport": {
               "type": "string"
            }
         }
      }
   }
}'
`
## Add data in bulk
`
curl -XPOST "http://elasticsearch:9200/sports/_bulk" -d'
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Michael", "birthdate":"1989-10-1", "sport":"Baseball", "rating": ["5", "4"],  "location":"46.22,-68.45"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bob", "birthdate":"1989-11-2", "sport":"Baseball", "rating": ["3", "4"],  "location":"45.21,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Jim", "birthdate":"1988-10-3", "sport":"Baseball", "rating": ["3", "2"],  "location":"45.16,-63.58" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Joe", "birthdate":"1992-5-20", "sport":"Baseball", "rating": ["4", "3"],  "location":"45.22,-68.53"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Tim", "birthdate":"1992-2-28", "sport":"Baseball", "rating": ["3", "3"],  "location":"46.22,-68.85"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Alfred", "birthdate":"1990-9-9", "sport":"Baseball", "rating": ["2", "2"],  "location":"45.12,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Jeff", "birthdate":"1990-4-1", "sport":"Baseball", "rating": ["2", "3"], "location":"46.12,-68.55"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Will", "birthdate":"1988-3-1", "sport":"Baseball", "rating": ["4", "4"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Mick", "birthdate":"1989-10-1", "sport":"Baseball", "rating": ["3", "4"],  "location":"46.22,-68.45"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Pong", "birthdate":"1989-11-2", "sport":"Baseball", "rating": ["1", "3"],  "location":"45.21,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Ray", "birthdate":"1988-10-3", "sport":"Baseball", "rating": ["2", "2"],  "location":"45.16,-63.58" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Ping", "birthdate":"1992-5-20", "sport":"Baseball", "rating": ["4", "3"],  "location":"45.22,-68.53"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Duke", "birthdate":"1992-2-28", "sport":"Baseball", "rating": ["5", "2"],  "location":"46.22,-68.85"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Hal", "birthdate":"1990-9-9", "sport":"Baseball", "rating": ["4", "2"],  "location":"45.12,-68.35"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Charge", "birthdate":"1990-4-1", "sport":"Baseball", "rating": ["3", "2"], "location":"46.12,-68.55"}
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Barry", "birthdate":"1988-3-1", "sport":"Baseball", "rating": ["5", "2"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bank", "birthdate":"1988-3-1", "sport":"Golf", "rating": ["6", "4"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Bingo", "birthdate":"1988-3-1", "sport":"Golf", "rating": ["10", "7"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"James", "birthdate":"1988-3-1", "sport":"Basketball", "rating": ["10", "8"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Wayne", "birthdate":"1988-3-1", "sport":"Hockey", "rating": ["10", "10"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Brady", "birthdate":"1988-3-1", "sport":"Football", "rating": ["10", "10"], "location":"46.25,-68.55" }
{"index":{"_index":"sports","_type":"athlete"}}
{"name":"Lewis", "birthdate":"1988-3-1", "sport":"Football", "rating": ["10", "10"], "location":"46.25,-68.55" }
'
`
## Calculate average rating
`
curl -XPOST "http://elasticsearch:9200/sports/athlete/_search" -d'
{
   "size": 0,
   "aggregations": {
      "the_name": {
         "terms": {
            "field": "name",
            "order": {
               "rating_avg": "desc"
            }
         },
         "aggregations": {
            "rating_avg": {
               "avg": {
                  "field": "rating"
               }
            }
         }
      }
   }
}'
`

## Counting a value
`
curl -XPOST "http://elasticsearch:9200/sports/athlete/_search" -d'
{
   "size": 0,
   "aggs": {
      "sport_count": {
         "value_count": {
            "field": "sport"
         }
      }
   }
}'
`
## Doing a bucket aggregation grouping by sport
`
curl -XPOST "http://elasticsearch:9200/sports/athlete/_search" -d'
{
   "size": 0,
   "aggregations": {
      "sport": {
         "terms": {
            "field": "sport"
         }
      }
   }
}'
`
# Doing an aggregration to find the distance between each player
`
curl -XPOST "http://elasticsearch:9200/sports/athlete/_search" -d'
{
   "size": 0,
   "aggregations": {
      "baseball_player_ring": {
         "geo_distance": {
            "field": "location",
            "origin": "46.12,-68.55",
            "unit": "mi",
            "ranges": [
               {
                  "from": 0,
                  "to": 20
               }
            ]
         }
      }
   }
}'
`
