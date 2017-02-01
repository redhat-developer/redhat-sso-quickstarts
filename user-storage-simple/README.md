user-storage-properties: User Storage SPI Simple Example
========================================================

Level: Beginner  
Technologies: JavaEE  
Summary: User Storage SPI Simple Example  
Target Product: RH-SSO  
Source: <https://github.com/redhat-developer/redhat-sso-quickstarts>  


What is it?
-----------

This is an example of user storage backend by a simple properties file.  This properties file only contains username/password key pairs.


System Requirements
-------------------

You need to have RH-SSO 7.1 running.

All you need to build this project is Java 8.0 (Java SDK 1.8) or later and Maven 3.1.1 or later.


Build and Deploy the Quickstart
-------------------------------

To deploy this provider you must have RH-SSO running in standalone or standalone-ha mode. Then type the follow maven command:

   ````
   mvn clean install wildfly:deploy
   ````

The `readonly-property-file` provider is hardcoded to look within the `users.properties` file embeded in the deployment jar
There is one user 'tbrady' with a password of 'superbowl'

The `writeable-property-file` provider can be configured to point to a property file on disk.  It uses federated
storage to augment the property file with any other information the user wants.

Our developer guide walks through the implementation of both of these providers.
