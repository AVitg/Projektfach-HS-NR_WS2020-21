# Suche
* url.original
```
url.original:*/?-*   vs    url.original:"*/?-*"
```
```
url.original:*whoami*
```



Um Indexe zu backupen /exportieren:


in /etc/elasticsearch/elasticsearch.yml
path.repo: /var/elastic/  //ordner muss existieren und nicht in und schreibbar sein /tmp liegen


im dev tool:
```
PUT /_snapshot/my_backup001
{
    "type": "fs",
    "settings": {
        "location": "/var/elastic/001",
        "compress": true
    }
}


PUT /_snapshot/my_backup001/snapshot_1
GET /_snapshot/my_backup001/snapshot_1

GET /_snapshot/_status

GET /_snapshot

GET /_snapshot/_all
```



```
{
  // We use Vega-Lite syntax here instead of Vega. The Vega visualization
  // supports both and we can specify which one we want to use by specifying
  // the corresponding schema here.
  $schema: "https://vega.github.io/schema/vega-lite/v2.json"
  // Use points for drawing to actually create a scatterplot
  mark: point
  // Specify where to load data from
  data: {
    // By using an object to the url parameter we will
    // construct an Elasticsearch query
    url: {
      // Context == true means filters of the dashboard will be taken into account
      %context%: true
      // Specify on which field the time picker should operate
      %timefield%: @timestamp
      // Specify the index pattern to load data from
      index: filebeat-*
      // This body will be send to Elasticsearch's _search endpoint
      // You can use everything the ES Query DSL supports here
      body: {
        // Set the size to load 10000 documents
        size: 10000
        // Just ask for the fields we actually need for visualization
        _source: ['@timestamp', 'source.ip', 'destination.port']
      }
    }
    // Tell Vega, that the array of data will be inside hits.hits of the response
   
    format: { property: "hits.hits" }
  }
  // You can do transformation and calculation of the data before drawing it
  transform: [
    // Since @timestamp is a string value, we need to convert it to a unix timestamp
    // so that Vega can work on it properly.
    {
      // Convert _source.@timestamp field to a date
      calculate: "toDate(datum._source['@timestamp'])"
      // Store the result in a field named "time" in the object
      as: "time"
    }
  ]
  // Specify what data will be drawn on which axis
  encoding: {
    y: {
      // Draw the time field on the x-axis in temporal mode (i.e. as a time axis)
      field: _source.source.ip
      type: nominal
      // Hide the axis label for the x-axis
      axis: { title: false }
    }
    x: {
    field: _source.destination.port
    type:nominal
    axis: { title: false }
    }
    size: {field: '_source.destination.port', type: "nominal", legend: null}
   color: {field: '_source.source.ip', type: "nominal", legend: null}
    
  }
}
```

```
PUT _watcher/watch/php_issue
{
  "trigger": {
    "schedule": {
      "interval": "10s"
    }
  },
  "input": {
    "search": {
      "request": {
        "indices": ["filebeat*"],
        "body": {
          "size": 0,
          "query": {
            "bool": {
              "filter": [
                {
                  "range": {
                    "@timestamp": {
                      "gte": "now-10s"
                    }
                  }
                },
                {
                  "match": {
                    "url.original": "/?-s"
                    
                  }
                }
              ]
            }
          }
        }
      }
    }
  },
  "condition": {
    "compare": {
      "ctx.payload.hits.total": {
        "gt": 0
      }
    }
  },
  "actions" :{
	"log" : {
		"logging" : {
			"text" :" Warning ?-s found NEW {{ctx.payload.hits.total}}"
		}
	}
 }
}


GET _watcher/watch/php_issue

```



filebeat -c /etc/filebeat/filebeat.yml  -e




```
PUT _watcher/watch/php_issue2
{
  "trigger": {
    "schedule": {
      "interval": "10s"
    }
  },
  "input": {
    "search": {
      "request": {
        "indices": ["filebeat*"],
        "body": {
          "size": 0,
          "query": {
            "bool": {
              "filter": [
                {
                  "range": {
                    "event.ingested": {
                      "gte": "now-200s"
                    }
                  }
                },
                {
                  "match": {
                    "url.original": "/?-s"
                    
                  }
                }
              ]
            }
          }
        }
      }
    }
  },
  "condition": {
    "compare": {
      "ctx.payload.hits.total": {
        "gt": 0
      }
    }
  },
  "actions" :{
	"log" : {
		"logging" : {
			"text" :" Warning ?-s found NEW {{ctx.payload.hits.total}}"
		}
	}
 }
}


```

```
https://www.elastic.co/guide/en/x-pack/6.2/watcher-getting-started.html


PUT _watcher/watch/php_issue3
{
  "trigger": {
    "schedule": {
      "interval": "10s"
    }
  },
  "input": {
    "search": {
      "request": {
        "indices": ["filebeat*"],
        "body": {
          "size": 100,
          "query": {
            "bool": {
              "filter": [
                {
                  "range": {
                    "event.ingested": {
                      "gte": "now-200s"
                    }
                  }
                },
                {
                  "match": {
                    "url.original": "/?-s"
                    
                  }
                }
              ]
            }
          }
        }
      }
    }
  },
  "condition": {
    "compare": {
      "ctx.payload.hits.total": {
        "gt": 0
      }
    }
  },
  "actions" :{
		"index_payload" : { 
		    "transform" : { 
      	"script" : "return [ 'source.ip': ctx.payload.hits.hits.0._source.source.ip ]"
      },
    "index" : {
      "index" : "my-index"
      
    }
	}
 }
}
```
