# Curl
```
-i - Include headers
-X - HTTP Request type



# GET
curl -H "GATECH_ID: 904160213" -iX <GET> <Target>/<API>

# POST
curl -H "GATECH_ID: 904160213" -iX <POST> <Target>/<API> -H "Content-Type: application/json" -d "{\"<Field>\":\"<Value>\"}"
```
