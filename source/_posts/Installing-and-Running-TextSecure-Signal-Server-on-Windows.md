---
title: Installing and Running TextSecure (Signal) Server on Windows
date: 2017-02-01 05:43:06
tags: 
- Security
- Encryption
- end to end 
- Data Security
category: Data Security
---

# Installing and Running TextSecure (Signal) Server on Windows

While working at [Plexus](http://plexus365.com/), I had to implement Signal Server's end to end encryption on certain project. Though Open Whisper System had the code in [github](https://github.com/WhisperSystems/Signal-Server), the instructions were quite unclear on installing the server on local machine. I stumbled upon few Github posts and got this amazing guide. So this is my take on installing TextSecure (Signal) Server on a local Windows 10 machine. 

<!-- more -->

## Meta
- Author: Abhishek Deb(vikz91.deb@gmail.com, abhishek@plexus365.com)  
- Date: 1st Feb 2017
- Source:  [WhisperSystems](https://github.com/WhisperSystems/Signal-Server) [Lucaconte](https://github.com/lucaconte/BeatTheMeddler).  

## About
If you are using Whatsapp then you must have seen "end-to-end" encryption thingy! Well, After Apple's beef with FBI, Whatsapp just [turned on encryption for a billion people](https://www.wired.com/2016/04/forget-apple-vs-fbi-whatsapp-just-switched-encryption-billion-people/).

![Whatsapp End-to-end security](https://www.securebeam.com/images/whatsapp-end-to-end/whatsapp_end_to_end.jpg)  
*(Image Courtesy : [SecureBeam](https://www.securebeam.com/whatsapp-end-to-end))*

## Prerequisites

- Java 1.7.
- Windows 10
- Redis (for caching purposes)
- PostgreSQL instance
- Twillio account for SMS registration process confirmation
- Amazon AWS S3 bucket service for messages' attachments
- A GCM account from google developer console: a senderId add a ApiKey. Create a project, senderId is the projectNumber. For ApiKey, go to API management -> credentials -> create credentials -> API Keys -> Server Key and save this key
- APN account for Apple Push Notifications (two pem certificates) [ask]


## Building and Compiling 

1. Clone TextSecure (Signal) Server   
`` git clone https://github.com/lucaconte/Signal-Server.git``
2. Clone Push Server  
`` git clone https://github.com/lucaconte/PushServer.git``
3. Clone WebSocket Resources  
`` git clone https://github.com/lucaconte/WebSocket-Resources.git ``
5. In Push Server pom.xml, change ``capsule.maven.plugin.version`` version to ``1.0.1``
4. Build Push Server  (from inside this folder)  
`` mvn clean install ``
5. Download & install gpg4win gpg tool for signing)  
`` https://www.gpg4win.org/download.html : https://files.gpg4win.org/gpg4win-vanilla-2.3.3.exe``
6. Make sure Environment Variables are set for gpg  
`` GNUPGHOME : c:\Users\<username>\AppData\Roaming\gnupg //or whatever path it is ``  
7. Create default ``secring`` and ``pubring`` by opening gpg2.exe file from installed location
8. Exit the opened terminal which says "type message"
9. Build Push Websocket-Resource (from inside thsi folder)    
`` mvn clean install  ``
10. The build will fail at javadoc signing but its okay as we have got the main binary in target folder (websocket-resources-0.4.1)
11. Link this file in maven repo ( make sure the versions are same in the file and in teh command)  
`` mvn install:install-file -Dfile=./library/target/websocket-resources-0.4.1.jar -DgroupId=org.whispersystems -DartifactId=websocket-resources -Dversion=0.4.1 -Dpackaging=jar
  ``
12. Now build the Signal Server  (from inside the Signal Server folder)  
`` mvn clean install -DskipTests``
13. We skip the tests because they will fail for not finding a key which we will cover later


## Creating Config files
### pushserver.yml

```yml
redis:
  url: redis://localhost:6379/2

authentication:
  servers:
    -
      name: 123
      password: 123
gcm:
  xmpp: false
  apiKey: <INPUT HERE>
  senderId: <INPUT HERE>
  redphoneApiKey: AIddSyAsviyMy8jKe8chCEfr8NbeqGghy7oOCi4 #fake


apn:
  feedback: false
  pushKey: /path/to/your/apnpushcertificates/apns-dev-key-noenc.pem #fake
  voipKey: /path/to/your/apnpushcertificates/apns-dev-key-noenc.pem #fake
  voipCertificate: /path/to/your/apnpushcertificates/apns-dev-cert.pem #fake
  pushCertificate: /path/to/your/apnpushcertificates/apns-dev-cert.pem #fake

server:
    applicationConnectors:
    - type: http
      port: 9090
    adminConnectors:
    - type: http
      port: 9091
    gzip:
        enabled: true

logging:
  level: INFO
  appenders:
    - type: file
      currentLogFilename: /tmp/pushserver.log
      archivedLogFilenamePattern: /tmp/pushserver-%d.log.gz
      archivedFileCount: 5
    - type: console
```

### textsecure.yml

```yml
# This is the sample config/textsecure.yml file for the TextSecure Server
# Pay attention! To start TextSecur server you will need to install and start PushServer

twilio:
  accountId: <INPUT HERE>
  accountToken: <INPUT HERE>
  numbers:
    -
      +33756796138 #fake
  localDomain: foo.org

push:
  host: localhost
  port: 9090
  username: 123
  password: 123

s3:
   accessKey: ABCDEFGCUFYDHVM2LXXX #fake
   accessSecret: W0UfGDddfAbqYyCTIIbSQlDtreTGokOs0OTpL0SE #fake
   attachmentsBucket: thenameofyouts3buket #fake

directory:
  url: "redis://localhost:6379/0"

cache:
  url: "redis://localhost:6379/1"

server:
  applicationConnectors:
    - type: http
      port: 8080
      #keyStorePath: config/example.keystore
      #keyStorePassword: example
      #validateCerts: true
  adminConnectors:
    - type: http
      port: 8081
      #keyStorePath: config/example.keystore
      #keyStorePassword: example
      #validateCerts: true


websocket:
  enabled: true

messageStore: # Postgres database configuration for message store
  driverClass: org.postgresql.Driver
  user: "postgres"
  password: "postgres"
  url: "jdbc:postgresql://localhost:5432/messagedb"

database:
  driverClass: org.postgresql.Driver
  user: "postgres"
  password: "postgres"
  url: "jdbc:postgresql://localhost:5432/accountsdb"
  properties:
    charSet: UTF-8

#federation: # is disabled

logging:
  level: INFO
  appenders:
    - type: file
      currentLogFilename: /tmp/textsecureshserver.log
      archivedLogFilenamePattern: /temp/textsecureserver-%d.log.gz
      archivedFileCount: 5
    - type: console


redphone:
  authKey: 1234567890 #fake

```
Place pushserver.yml in Push Server Folder.  
Place textsecure.yml in Text Sexure /config folder.  

## Disabling iOS Features ( Temporarily)
useful part if you plan to test Android only as first

1. Remove @NotEmpty annotation above "private ApnConfiguration apn;" field definition into ``org.whispersystems.pushserver.PushServerConfiguration`` class, so into yml PushServers's file the APN related params become optional.

2. Into ``org.whispersystems.pushserver.PushServer``'s "run" method comment every row with apnSender var and pass null in the jersey Controller registration of the PushController:

``environment.jersey().register(new PushController(null, gcmSender));``  
instead of  
``environment.jersey().register(new PushController(apnSender, gcmSender));``  
Lines : 55, 58 and 62



## Configuring & Running Database

### Postgresql
1. Download and Install Postgres  
`` https://www.postgresql.org/download/windows/ ``
2. Create Databases (Exit all postgres cli instances first)  
`` createdb -U postgres accountsdb  ``  
`` createdb -U postgres messagedb  ``
3. Import Table Schemas: (from inside Signal-Server folder)  
`` java -jar target/TextSecureServer-<VERSION>.jar accountdb migrate config/textsecure.yml ``  
`` java -jar target/TextSecureServer-<VERSION>.jar messagedb migrate config/textsecure.yml ``

### Redis
1. Download and Install Redis  
`` https://github.com/rgl/redis/downloads ``




## Running Everything Together
1. Run Redis
`` redis-server ``
2. Run Push Server  
  `` java -jar Push-Server-<VERSION>-capsule-fat.jar server pushserver.yml ``
3. Run Text Secure ( Signal) Server   
``  java -jar target/TextSecureServer-<VERSION>.jar server config/textsecure.yml  ``


## Testing with Android App
Coming soon ...