# Curl
```bash
-i - Include headers
-X - HTTP Request type
-H - Header value
-d - Data

# GET
curl -H "GATECH_ID: 904160213" -iX <GET> <Target>/<API>

# POST
curl -H "GATECH_ID: 904160213" -iX <POST> <Target>/<API> -H "Content-Type: application/json" -d "{\"<Field>\":\"<Value>\"}"
curl -H "GATECH_ID: 904160213" -iX <POST> <Target>/<API> --json '{"<Key>":"<Value>"}'
```
