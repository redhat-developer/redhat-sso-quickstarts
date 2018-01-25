app-authz-rest-springboot: SpringBoot REST Service Protected Using Red Hat SSO Authorization Services
===================================================

Level: Beginner
Technologies: SpringBoot
Summary: SpringBoot REST Service Protected Using Red Hat SSO Authorization Services
Target Product: Red Hat SSO
Source: <https://github.com/redhat-developer/redhat-sso-quickstarts>


What is it?
-----------

The `app-authz-rest-springboot` quickstart demonstrates how to protect a SpringBoot REST service using Red Hat SSO Authorization Services.

This application tries to focus on the authorization features provided by Red Hat SSO Authorization Services, where resources are
protected by a set of permissions and policies defined in Red Hat SSO itself and access to these resources are enforced by a policy enforcer
that intercepts every single request to the application.

In this application, there are three paths protected by specific permissions in Red Hat SSO:

* **/api/{resource}**, where access to this page is based on the evaluation of permissions associated with a resource **Default Resource** in Red Hat SSO. Basically,
any user with a role *user* is allowed to access this page. Examples of resource that match this path pattern are: "/api/resourcea" and "/api/resourceb".

* **/api/premium**, where access to this page is based on the evaluation of permissions associated with a resource **Premium Resource** in Red Hat SSO. Basically,
only users with a role *user-premium* is allowed to access this page.

You can use two distinct users to access this application:

|Username|Password|Roles|
|---|---|---|
|alice|alice|user|
|jdoe|jdoe|user, user-premium|

System Requirements
-------------------

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.1.1 or later.


Configuration in Red Hat SSO
-----------------------

Prior to running the quickstart you need to create a `realm` in Red Hat SSO with all the necessary configuration to deploy and run the quickstart.

The following steps show how to create the realm required for this quickstart:

* Open the Red Hat SSO admin console
* In the top left corner dropdown menu that is titled `Master`, click `Add Realm`. If you are logged in to the master realm this dropdown menu lists all the realms created.
* For this quickstart we are not going to manually create the realm, but import all configuration from a JSON file. Click on `Select File` and import the [config/realm-import.json](config/realm-import.json).
* Click `Create`

The steps above will result on a new `spring-boot-quickstart` realm.

Build and Run the Quickstart
-------------------------------

Make sure your Red Hat SSO server is running on <http://localhost:8180/>. For that, you can start the server using the command below:

   ````
   cd {KEYCLOAK_HOME}/bin
   ./standalone.sh -Djboss.socket.binding.port-offset=100
   
   ````

If your server is up and running, perform the following steps to start the application:

1. Open a terminal and navigate to the root directory of this quickstart.

2. The following shows the command to deploy the quickstart:

   ````
   mvn spring-boot:run

   ````

Access the Quickstart
---------------------

In order to understand better what is happening behind the scenes, we describe some steps using Curl to exemplify 
how clients can get access to the RESTful resources provided by this application.

First thing, your client needs to obtain an OAuth2 access token from a Red Hat SSO server.

```bash
curl -X POST \
  http://localhost:8180/auth/realms/spring-boot-quickstart/protocol/openid-connect/token \
  -H 'Authorization: Basic YXBwLWF1dGh6LXJlc3Qtc3ByaW5nYm9vdDpzZWNyZXQ=' \
  -H 'content-type: application/x-www-form-urlencoded' \
  -d 'username=alice&password=alice&grant_type=password'
```

After executing the command above, you should get a response similar to the following:

```bash
{
    "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJGSjg2R2NGM2pUYk5MT2NvNE52WmtVQ0lVbWZZQ3FvcXRPUWVNZmJoTmxFIn0.eyJqdGkiOiI3OWY4NmFjZS01Zjk4LTQ0MTctYWJmZC0xMjcyOGQ2OGJkNDEiLCJleHAiOjE1MDQxOTE5MzYsIm5iZiI6MCwiaWF0IjoxNTA0MTkxNjM2LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgxODAvYXV0aC9yZWFsbXMvc3ByaW5nLWJvb3QtcXVpY2tzdGFydCIsImF1ZCI6ImFwcC1hdXRoei1yZXN0LXNwcmluZ2Jvb3QiLCJzdWIiOiJlNmE3NzcyYS1kZmZlLTRiNDItYTFiMS0zZDZmOTM0OWE0NmIiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJhcHAtYXV0aHotcmVzdC1zcHJpbmdib290IiwiYXV0aF90aW1lIjowLCJzZXNzaW9uX3N0YXRlIjoiNzE4MmU0YzEtNzY5ZS00MTNlLWI2MWItM2FlZTFjYWZmY2JmIiwiYWNyIjoiMSIsImFsbG93ZWQtb3JpZ2lucyI6WyJodHRwOi8vbG9jYWxob3N0OjgwODAiXSwicmVhbG1fYWNjZXNzIjp7InJvbGVzIjpbInVzZXIiXX0sInJlc291cmNlX2FjY2VzcyI6e30sInByZWZlcnJlZF91c2VybmFtZSI6ImFsaWNlIn0.bpKwmY2CqEm1TLYZoC_6jG0V1XcLKC2dStTAnUJgUMQfTBn3kZHsrWZeahKq7IdVocn7bWoBU0mP8i0rf89GcoZS1j-oju32XArTtE2e-tWVeWaRa1vJHNjhsIAuvZ4CmRh6QOTa-0qowbi1oEZxL3aQ6jPL4OSjBOAJgS51tn4",
    "expires_in":300,
    "refresh_expires_in":1800,
    "refresh_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJGSjg2R2NGM2pUYk5MT2NvNE52WmtVQ0lVbWZZQ3FvcXRPUWVNZmJoTmxFIn0.eyJqdGkiOiI2Mzk3MDRhOS1jYTg1LTQxOWYtODA5Yi03MDkzOGQyNzQwNTQiLCJleHAiOjE1MDQxOTM0MzYsIm5iZiI6MCwiaWF0IjoxNTA0MTkxNjM2LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgxODAvYXV0aC9yZWFsbXMvc3ByaW5nLWJvb3QtcXVpY2tzdGFydCIsImF1ZCI6ImFwcC1hdXRoei1yZXN0LXNwcmluZ2Jvb3QiLCJzdWIiOiJlNmE3NzcyYS1kZmZlLTRiNDItYTFiMS0zZDZmOTM0OWE0NmIiLCJ0eXAiOiJSZWZyZXNoIiwiYXpwIjoiYXBwLWF1dGh6LXJlc3Qtc3ByaW5nYm9vdCIsImF1dGhfdGltZSI6MCwic2Vzc2lvbl9zdGF0ZSI6IjcxODJlNGMxLTc2OWUtNDEzZS1iNjFiLTNhZWUxY2FmZmNiZiIsInJlYWxtX2FjY2VzcyI6eyJyb2xlcyI6WyJ1c2VyIl19LCJyZXNvdXJjZV9hY2Nlc3MiOnt9fQ.CV3tZfYylhXMK7F0ozf-u4AZx_edm0FhdIDYiRDbhM3trRFzmpaRtuwML2KethVqj01PhD0VYYjt2yK0GWgkFswW1tc-Rqq2TMjabeGTcLPwvb8NZ7FcZnwglZUU46mfuV8-m1Idzqgs5DhmpkBALkAXjeaVPedAMNsPFQSPJE4",
    "token_type":"bearer",
    "not-before-policy":0,
    "session_state":"7182e4c1-769e-413e-b61b-3aee1caffcbf"
}
```
Now replace the ``${access_token}`` variable below with the value of the ``access_token`` claim from the response. 

```bash
curl -X POST \
  http://localhost:8180/auth/realms/spring-boot-quickstart/authz/entitlement/app-authz-rest-springboot \
  -H "Authorization: Bearer ${access_token}" \
  -H 'Content-type: application/json' \
  -d '{
	"permissions": [
		{
			"resource_set_name": "Default Resource"
		}	
	]
}'
```

As an alternative, you can also obtain permissions for any resource protected by your application. For that, execute the command below:

```bash
curl -X GET \
    http://localhost:8180/auth/realms/spring-boot-quickstart/authz/entitlement/app-authz-rest-springboot \
    -H "Authorization: Bearer ${access_token}"
```

After executing the command above, you should get a response similar to the following:

```bash
{
    "rpt": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJGSjg2R2NGM2pUYk5MT2NvNE52WmtVQ0lVbWZZQ3FvcXRPUWVNZmJoTmxFIn0.eyJqdGkiOiIzYzI2NmZhOS01NmZlLTQ0MjItODg4Ni0xMmRjYTRlZTYyMTYiLCJleHAiOjE1MDQxOTIwNDQsIm5iZiI6MCwiaWF0IjoxNTA0MTkxNzQ0LCJpc3MiOiJodHRwOi8vbG9jYWxob3N0OjgxODAvYXV0aC9yZWFsbXMvc3ByaW5nLWJvb3QtcXVpY2tzdGFydCIsImF1ZCI6ImFwcC1hdXRoei1yZXN0LXNwcmluZ2Jvb3QiLCJzdWIiOiJlNmE3NzcyYS1kZmZlLTRiNDItYTFiMS0zZDZmOTM0OWE0NmIiLCJ0eXAiOiJCZWFyZXIiLCJhenAiOiJhcHAtYXV0aHotcmVzdC1zcHJpbmdib290IiwiYXV0aF90aW1lIjowLCJzZXNzaW9uX3N0YXRlIjoiMGQ2NTQ0YzMtZmY2Yi00ZGU4LTk5YzEtNDM2ZDg2YzkyNzIxIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiYWxpY2UiLCJhY3IiOiIxIiwiYWxsb3dlZC1vcmlnaW5zIjpbImh0dHA6Ly9sb2NhbGhvc3Q6ODA4MCJdLCJyZWFsbV9hY2Nlc3MiOnsicm9sZXMiOlsidXNlciJdfSwicmVzb3VyY2VfYWNjZXNzIjp7fSwiYXV0aG9yaXphdGlvbiI6eyJwZXJtaXNzaW9ucyI6W3sicmVzb3VyY2Vfc2V0X2lkIjoiNDIwZGMxNzktZTY5OC00ZmYzLTkyMTgtMWE0YmQ5N2I2ZjU5IiwicmVzb3VyY2Vfc2V0X25hbWUiOiJEZWZhdWx0IFJlc291cmNlIn1dfX0.m6ePUAiAGkRlAQWvroiRq4EwqTNGxsc3TtW0YbFeF_n8WrzJA38mIwrnU-D1fFSXBvA3SxPImf4y9er5Zo31ZMtBNhns4-yl850JPEoApJoaUHeL_PMCvN9zwvIi4IN4-wl58yOjQ1MzJ50SOpiJKfp7VXLgJQkj8OodMctz0GE"
}
```

Finally, you are able to access the RESTFul resources protected by this application with a RPT (Requesting Party Token). For that, replace the ``${rpt}`` variable below with the value of the ``rpt`` claim from the response.

```bash
curl -X GET \
  http://localhost:8080/api/resourcea \
  -H 'Authorization: Bearer ${rpt}'
```

User *alice* should be able to access */api/resourcea* and you should get **Access Granted** as a response. Now, if you try to access */api/premium* as *alice* you should
get a **403** HTTP status code.

You should also get a **403** HTTP status code if you try to obtain permissions from Red Hat SSO for "Premium Resource".

```bash
curl -X POST \
  http://localhost:8180/auth/realms/spring-boot-quickstart/authz/entitlement/app-authz-rest-springboot \
  -H "Authorization: Bearer ${access_token}" \
  -H 'Content-type: application/json' \
  -d '{
	"permissions": [
		{
			"resource_set_name": "Premium Resource"
		}	
	]
}'
```

As a response you should get something similar to the following:

```bash
{"error":"not_authorized"}
```

You can follow the same steps to check behavior when accessing the same resources with user *jdoe*.

Integration test of the Quickstart
----------------------------------

1. Make sure you have an Red Hat SSO server running with an admin user in the `master` realm or use the provided docker image
2. Be sure to set the `TestHelper.keycloakBaseUrl` in the `createArchive` method (default URL is localhost:8180/auth).
3. Set accordingly the correct url for the `keycloak.auth-server-url` in the test [application.properties](src/test/resources/application.properties).
4. Run `mvn test`