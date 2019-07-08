
# B2B Mortage Origination Demo using IBM Blockchain Platform

The objective is to develop a Decentralized Application that explores potential business benefits of using blockchain like

-   Improved process efficiency
-   Economy of scale
-   Privacy when you need it
-   Transparency and trust
-   Decentralized data with a logically centralized view
-   Recording of payments onto a blockchain

The use case we picked up to meet our objectives is a Loan Origination and Payment Application. The actual implementation of the use case doesn’t cover all intricacies and regulatory aspects of the loan origination and payments process but will be a good indicator of the design aspects to consider while building a DApp on the blockchain
# The DEMO Video

[![](http://img.youtube.com/vi/w2GT9wH-S3o/0.jpg)](http://www.youtube.com/watch?v=w2GT9wH-S3o "")

# The Blockchain N/W Architecture of the Demo

![](https://cdn-images-1.medium.com/max/1600/1*B9s7br7Ltqsi88lMS6DWGw.png)

## Application Architecture

The design of the Blockchain network is just one piece of the puzzle. Application design and architecture is equally important. Consider the following as you design your application:

-   Although IBM Blockchain Platform with Hyperledger Fabric at its heart scales well when compared to the public blockchain platforms, it is not a transactional database, so don’t expect throughputs of 
-   Use the blockchain for what it has to offer — trust, transparency, and immutability. Decide what you store in your blockchain based on that.
-   Embrace asynchronous event-driven programming — the blockchain nodes are only eventually consistent. The consensus process is not instantaneous.
-   The data in the blockchain is secured by design through encryption and various governing principles of the network, but the security stops at the blockchain network level. You should still address the application security with your design.
-   Think about GDPR-like regulations before you store personally identifiable information onto the blockchain.

## Design

We followed a modular microservices architecture. The complexity of interaction with the blockchain itself is abstracted out to a generic component that knows how to invoke chaincode to read/write data from/to blockchain. This helps to keep the other services lightweight and flexible.

![](https://cdn-images-1.medium.com/max/1600/1*6BWxsORbJPI1WrevJXAi_Q.png)

## Data Flow

-   The microservices expose REST APIs for the presentation layer to consume.
-   The services delegate calls to the Hyperledger Gateway for blockchain invocations.
-   The blockchain calls are asynchronous. Once the transactions are added to the blockchain the Event Hub pushes the signal to all interested subscribers through a callback endpoint.
-   We use WebSockets for the presentation layer to react to the events from the Event hub.

![](https://cdn-images-1.medium.com/max/1600/1*bZ7jNja2DaqcgYX6ELZc9A.gif)

## more documentation 
https://www.objectfrontier.com/blog/ibm-blockchain-platform-2-0-beta-run-your-blockchain-n-w-in-10-simple-steps/

https://www.objectfrontier.com/blog/demystifying-the-fabric-chaincode-with-an-example/

https://www.objectfrontier.com/blog/invoking-fabric-chaincode-from-your-application/

## Installtion Instructions

Following are the steps involved in deploying application and running the application localhost as well as in a cloud   (say in AWS EC2 instance)

### Prerequisites

* Intel Quad Core processor
* 8 GB RAM
* 160 GB internal hard drive
* Ubuntu 16.04
* Java
* MySQL
* Tomcat 8.5
* IBM - Blockchain Platform


### Step 1: Clone and Run the Microservices Code Repositories

1. Hyperledger Gateway Services https://github.com/objectfrontiergit/IBP-Gateway 

* mvn clean install (It'll fetch the dependencies)
* mvn spring-boot:run & (To run start the application)

The Gateway service APIs shall be used to integrate with the IBM Blockchain Platform(Hyperledger Fabric).

2. Applicant Services
https://github.com/objectfrontiergit/IBP-ApplicantAPI

Notes: 
* Run *src/main/resources/ibp_applicant.sql* this sql file in your database. update the DB connection details in *src/main/resources/application.properties* before you build and deploy
* replace *src/main/resources/config/connectionProfile.json* with the JSON from your network

Lender Services

https://github.com/objectfrontiergit/IBP-BankAPI

Notes: 
* Run * src/main/resources/ibp_lender.sql* this sql file in your database. update the DB connection details in *src/main/resources/application.properties* before you build and deploy
* replace *src/main/resources/config/connectionProfile.json* with the JSON from your network

https://github.com/objectfrontiergit/IBP-Lender2API

Notes: 
* Run * src/main/resources/ibp_lender2.sql* this sql file in your database. update the DB connection details in *src/main/resources/application.properties* before you build and deploy
* replace *src/main/resources/config/connectionProfile.json* with the JSON from your network

Partner Services
https://github.com/objectfrontiergit/IBP-MortgageAPI

The Web Application
https://github.com/objectfrontiergit/IBP-LoanMgtUI

Make sure you keep  *src/main/resources/application.properties* file upto date matching your network details

### Running the application in AWS

**_1._** **_Java & Tomcat Installation:_**

**Install OpenJDK:**

    $ sudo apt install default-jdk 

(OpenJDK, the open source implementation of the Java Platform is the default Java development and runtime in Ubuntu 16.04.)

**Create Tomcat user:**

    $ sudo useradd -m -U -d /opt/tomcat -s /bin/false tomcat


**Download & Install Tomcat:**

    $ sudo apt install unzip wget
    $ cd /tmp
    $ wget http://www-us.apache.org/dist/tomcat/tomcat-8/v8.5.37/bin/apache-tomcat-8.5.37.zip
    $ unzip apache-tomcat-*.zip
    $ sudo mkdir -p /opt/tomcat
    $ sudo mv apache-tomcat-8.5.37 /opt/tomcat/
    $ sudo ln -s /opt/tomcat/apache-tomcat-8.5.37 /opt/tomcat/latest
    $ sudo chown -R tomcat: /opt/tomcat
    $ sudo sh -c 'chmod +x /opt/tomcat/latest/bin/*.sh'


**Create a systemd unit file:**

Create a new tomcat.service unit file in the /etc/systemd/system/ directory with the following contents:

$ nano /etc/systemd/system/tomcat.service

    [Unit]
    Description=Tomcat 8.5 servlet container
    After=network.target
    [Service]
    Type=forking
    User=tomcat
    Group=tomcat
    Environment="JAVA_HOME=/usr/lib/jvm/default-java"
    Environment="JAVA_OPTS=-Djava.security.egd=file:///dev/urandom"
    Environment="CATALINA_BASE=/opt/tomcat/latest"
    Environment="CATALINA_HOME=/opt/tomcat/latest"
    Environment="CATALINA_PID=/opt/tomcat/latest/temp/tomcat.pid"
    Environment="CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC"
    ExecStart=/opt/tomcat/latest/bin/startup.sh
    ExecStop=/opt/tomcat/latest/bin/shutdown.sh
    [Install]
    WantedBy=multi-user.target
    
 you then use the following commands

    $ sudo systemctl daemon-reload
    $ sudo systemctl enable tomcat
    $ sudo systemctl start tomcat
    $ sudo systemctl stop tomcat
    $ sudo systemctl status tomcat
    $ sudo systemctl restart tomcat

To access tomcat  you will need to open port 8080

**Test the Installation**

Open your browser and type: http://<your_domain_or_IP_address>:8080

**_2._** **_MySQL Installation:_**

**Installing MySQL:**

To install MySQL simply update the package index on your server and install the default package with apt-get.

    $ sudo apt-get update
    $ sudo apt-get install mysql-server
    $ mysql_secure_installation
**Testing MySQL:**

To test mysql, check its status from below command.

$ systemctl status mysql.service

**Database Configurations :**

Use MySQL Command-Line Tool to update database configurations. Login to the tool with root user as below.

ec2-user@host.ip.:~$ mysql -h localhost -u root -p

Enter password: ******

**Create Database:**

CREATE DATABASE IF NOT EXISTS ibp_applicant CHARACTER SET utf8 COLLATE utf8_general_ci;

**Create User:**

CREATE USER 'ibp_user'@'localhost' IDENTIFIED BY 'password';

**Grant Permission:**

GRANT ALL ON ibp_applicant.* TO 'ibp_user'@'localhost' IDENTIFIED BY 'password';

Find SQL files from repo (src/main/resources) and execute here.

**_3._** **_Create & Deploy WAR File:_**


Create war file for each API services (Any changes in application.properties, update it before creating war file) .

i. Create WAR file for Gateway (To create WAR file run this command in CLI: mvn clean install)

ii. The war will be created in 'target' folder

iii. Move the war file to server tomcat wepapps path (example: /opt/tomcat/wepapps/) in your EC2 instance

iv. War will be unpacked as ibp-gateway. Then check gateway URL in browser(eg: http://<<yourIP>>:8080/ibp-gateway). Use this URL for remaining API services gateway URL.

Repeate the same process for all the API Services.

**_2._** **_UI Application Setup:_**


To update the services URL open this file in UI repo(src/app/http.service.ts) and change the API services URL.

i. Clone UI repo URL.

ii. open cmd(cli) and change directory to your repo path

iii. npm install (it'll fetch angular dependencies)

iv. npm run start (to start the App)

v. ng build (to create build file, the build folder will be generated in dist folder. Deploy this folder for UI.

## OFS support
Please reachout to ofstechsupport@objectfrontier.com for any questions about the demo and codebase.
