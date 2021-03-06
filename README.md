# elasticsearch-cheatsheet

### Show nodes
```
GET _cat/nodes?v
```

### Show indices
```
GET _cat/indices?v
```

### Create an index
```
PUT <index_name>
```

### Get index details
```
GET <index_name>
```

### Create a document
```
POST <index_name>/_doc
{
  <document_body>
}
```

#### Example
```
POST bank/_doc
{
  "name": "Super Bank",
  "foo": "bar"
}
```

### Get details of a document
```
GET <index_name>/_doc/<document_id>
```

### Update a document
```
POST <index_name>/_update/<document_id>
{
  "doc": {
    <document_body>
  }
}
```

### Update a document with a filter
```
POST <index_name>/_update/<document_id>
{
  "script: {
    "lang": "painless"
    "source": "<painless_script>"
  }
}
```
#### Example
```
POST bank/_update/r6AliXABm3M6SXHJ0qnB
{
  "script: {
    "lang": "painless"
    "source": "ctx._source.remove('clients')"
  }
}
```

### Delete an index
```
DELETE <index_name>
```

### Delete a document
```
DELETE <index_name>/_doc/<document_id>
```

### Search documents
```
GET <index_name>/_search
```

### Get source of a document
```
GET <index_name>/_source/<document_id>
```

## Aliases
### Create an index
```
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "<index_name>",
        "alias": "<alias_name>"
      }
    }
  ]
}
```
### Delete an index
```
POST _aliases
{
  "actions": [
    {
      "remove": {
        "index": "<index_name>",
        "alias": "<alias_name>"
      }
    }
  ]
}
```
#### Example
```
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "dogs",
        "alias": "doggos"
      }
    },
    {
      "add": {
        "index": "dogs",
        "alias": "puppies"
      }
    },
    {
      "remove": {
        "index": "dogs",
        "alias": "cats"
      }
    }
  ]
}
```

### Set an alias with a filter
```
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "<index_name>",
        "alias": "<alias_name>",
        "filter": {
          <filter>
        }
      }
    }
  ]
}
```

#### Example
```
POST _aliases
{
  "actions": [
    {
      "add": {
        "index": "dogs",
        "alias": "best-dogos",
        "filter": {
          "term": {
            "breed": "husky"
          }
        }
      }
    }
  ]
}
```

## Templates
### Define a template
```
PUT _template/logs
{
  "aliases": {
    "<alias>": {}
  },
  "mappings": {
    "properties": {
      "<field>": {
        "type": "<field_type>"
       }
    }
  },
  "settings": {
    <settings>
  },
  "index_patterns": ["<pattern>"]
}
```

#### Example
```
PUT _template/logs
{
  "aliases": {
    "logs": {}
  },
  "mappings": {
    "properties": {
      "title": {
        "type": "keyword"
       }
    }
  },
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 1
  },
  "index_patterns": ["logs-*"]
}
```
### Dynamic templates
#### Example
```
PUT _template/<template_name>
{
  "index_patterns": ["<pattern>"]
  "mappings": {
    "dynamic_templates": [
      {
        "strings_to_keywords": {
          "match_mapping_type": "string",
          "unmatch": "*_text",
          "mapping": {
            "type": "keyword"
          }
        }
      },
      {
        "longs_to_integers": {
          "match_mappings_type": "long",
          "mapping": {
            "type": "integer"
          }
        }
      },
      {
        "strings_to_text": {
          "match_mapping_type": "string",
          "match": "*_text",
          "mapping": {
            "type": "text"
          }
        }
      }
    ]
  }
}
```
