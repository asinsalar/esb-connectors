Product: Integration tests for WSO2 ESB Wordpress connector

Pre-requisites:

 - Maven 3.x
 - Java 1.6 or above

Tested Platform: 

 - Microsoft WINDOWS V-7
 - UBUNTU 13.04
 - WSO2 ESB 4.8.1

STEPS:

 1. Make sure the ESB 4.8.1 zip file with latest patches available at "Integration_Test/products/esb/4.8.1/modules/distribution/target/"

 2. This ESB should be configured as below;
	In Axis configurations (\repository\conf\axis2\axis2.xml).

   i) Enable message formatter for "text/html" in messageFormatters tag
			<messageFormatter contentType="text/html" class="org.wso2.carbon.relay.ExpandingMessageFormatter"/>

   ii) Enable message builder for "text/html" in messageBuilders tag
			<messageBuilder contentType="text/html" class="org.wso2.carbon.relay.BinaryRelayBuilder"/>

   iii) Enable message formatter for "application/x-www-form-urlencoded" in messageFormatters tag
                       <messageFormatter contentType="application/x-www-form-urlencoded"
                          class="org.apache.axis2.transport.http.XFormURLEncodedFormatter"/>
   
   iV) Enable message builder for "application/x-www-form-urlencoded" in messageBuilders tag
			<messageBuilder contentType="application/x-www-form-urlencoded"
                        class="org.apache.synapse.commons.builders.XFormURLEncodedBuilder"/>

   V) Enable message formatter for "application/json" in messageFormatters tag
			<messageFormatter contentType="application/json" class="org.apache.synapse.commons.json.JsonStreamFormatter"/>

   Vi) Enable message builder for "application/json" in messageBuilders tag
			<messageBuilder contentType="application/json" class="org.apache.synapse.commons.json.JsonStreamBuilder"/>

   Vii) Install HTTP PATCH request enabling patch and Json patch to ESB 4.8.1
		patch0804 - http PATCH request patch
		patch0800 - Json string escape ("\") character patch


 3. Make sure "integration-base" project is placed at "Integration_Test/products/esb/4.8.1/modules/integration/"

 4. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/integration-base" and run the following command.
      $ mvn clean install

 5. Add following dependancy to the file "Integration_Test/products/esb/4.8.1/modules/integration/connectors/pom.xml"
	<dependency>

		<groupId>org.wso2.esb</groupId>

		<artifactId>org.wso2.connector.integration.test.base</artifactId>

		<version>4.8.1</version>

		<scope>system</scope>

		<systemPath>${basedir}/../integration-base/target/org.wso2.connector.integration.test.base-4.8.1.jar</systemPath>

	</dependency>

 6. Copy Wordpress connector zip file (wordpress.zip) to the location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/repository/"

 7. Make sure the wordpress test suite is enabled (as given below) and all other test suites are commented in the following file - "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/testng.xml"  
     <test name="Wordpress-Connector-Test" preserve-order="true" verbose="2">
        <packages>
            <package name="org.wso2.carbon.connector.integration.test.wordpress"/>
        </packages>
    </test>

8. Create a Wordpress.com account and to obtain the access token go through following steps
	i) Go to "https://wordpress.com/wp-login.php?" and create your wordpress account.
       ii) Using "https://developer.wordpress.com/apps/new/" create a wordpress app, once configured you will receive your CLIENT ID and 	    CLIENT SECRET to identify your app.
      iii) To obtain the access token use the authorization endpoint "https://public-api.wordpress.com/oauth2/authorize?	     		   client_id=your_client_id&redirect_uri=your_url&response_type=code"
           Server / Code Authentication The redirect to your application will include a "code" which you will need in the next step. 
       iv) Now pass the code to the token endpoint by making a POST request to the token endpoint;
	   "https://public-api.wordpress.com/oauth2/token"
           For more detailed instructions refer : "https://developer.wordpress.com/docs/oauth2/". 	

9. Copy the connector properties file at "Wordpress-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/artifacts/ESB/connector/config/wordpress.properties" to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/artifacts/ESB/connector/config/" and update the copied file as below.
	i) accessToken - use the access token you got from step 8
       ii) domain - use your wordpress site domain you used to pull the access token

10. Copy the java file "Wordpress-connector-1.0.0/org.wso2.carbon.connector/src/test/java/org/wso2/carbon/connector/integration/test/wordpress/WordpressConnectorIntegrationTest.java" to location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/java/org/wso2/carbon/connector/integration/test/wordpress/"


11. Copy proxy file "Wordpress-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/artifacts/ESB/config/proxies/wordpress/wordpress.xml" to location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/artifacts/ESB/config/proxies/wordpress/"

12. Copy rest request folder "Wordpress-connector-1.0.0/org.wso2.carbon.connector/src/test/resources/artifacts/ESB/config/restRequests/wordpress" to location "Integration_Test/products/esb/4.8.1/modules/integration/connectors/src/test/resources/artifacts/ESB/config/restRequests/"

13. Navigate to "Integration_Test/products/esb/4.8.1/modules/integration/connectors/" and run the following command.
      $ mvn clean install

 NOTE : Following Wordpress account, can be used for run the integration tests.
    Username :wso2wordpress@gmail.com	
    password : connector1234
