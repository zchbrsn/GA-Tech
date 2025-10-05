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
