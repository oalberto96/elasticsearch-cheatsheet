### Import date from a json file
```bash
curl -u elastic -H 'Content-Type: application/x-ndjson' -X POST 'http:<elasticsearch_ip>:9200/bank/_bulk?pretty' --data-binary @<json_file_path> > <output.json>
```
