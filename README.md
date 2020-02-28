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
