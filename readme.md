
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

-   Although IBM Blockchain Platform with Hyperledger Fabric at its heart scales well when compared to the public blockchain platforms, it is not a transactional database, so don’t expect throughputs of traditional relational database
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


* Docker
* IBM Blockchain Platform


### Step 1: Get Docker Images and Run the Microservices

#### 1. Hyperledger Gateway Services 
https://hub.docker.com/r/amarofs/ibp-gateway 

* docker run -v /home/fsadmin/ibp-devops/connectionProfile.json:/tmp/connectionProfile.json -v /home/fsadmin/ibp-devops/application.properties:/tmp/application.properties -p 8090:8090 -e  network.config.path=/tmp/connectionProfile.json --env JAVA\_OPTS=&quot;-Dspring.config.location=/tmp/application.properties;-Dlogging.level.org.springframework=DEBUG&quot; --name gateway amarofs/ibp-gateway

###### Notes: 

i. /home/ofsadmin/ibp-devops/connectionProfile.json -> The connection profile need to be downloaded from “IBM Network” and should append the channel details in this JSON.

ii. /home/ofsadmin/ibp-devops/application.properties -> Update custom configurations( peer name, channel details, Gateway URL) in this file and have it in host machine. 

iii. /tmp/connectionProfile.json && /tmp/application.properties -> This files will be available in Docker container. 

iv. network.config.path -> Provide the path which mentioned in the -v argument connection profile(probably /tmp/connectionProfile.json).

v. JAVA_OPTS -> The java configurations will be used while running the jar in docker images.

The Gateway service APIs shall be used to integrate with the IBM Blockchain Platform

#### 2. Applicant Services
https://hub.docker.com/r/amarofs/ibp-applicant

* docker run -v /home/ofsadmin/ibp-devops/connectionProfile.json:/tmp/connectionProfile.json -v /home/ofsadmin/ibp-devops/application.properties:/tmp/application.properties -p 8090:8090 -e  network.config.path=/tmp/connectionProfile.json --env JAVA_OPTS="-Dspring.config.location=/tmp/application.properties;-Dlogging.level.org.springframework=DEBUG" --name applicant amarofs/applicant

###### Notes: 

i. /home/ofsadmin/ibp-devops/connectionProfile.json -> The connection profile need to be downloaded from “IBM Network” and should append the channel details in this JSON.

ii. /home/ofsadmin/ibp-devops/application.properties -> Update custom configurations( peer name, channel details, Gateway URL) in this file and have it in host machine. 

iii. /tmp/connectionProfile.json && /tmp/application.properties -> This files will be available in Docker container. 

iv. network.config.path -> Provide the path which mentioned in the -v argument connection profile(probably /tmp/connectionProfile.json).

v. JAVA_OPTS -> The java configurations will be used while running the jar in docker images.

#### 3.Lender Services

https://hub.docker.com/r/amarofs/ibp-lender1


* docker run -v /home/ofsadmin/ibp-devops/connectionProfile.json:/tmp/connectionProfile.json -v /home/ofsadmin/ibp-devops/application.properties:/tmp/application.properties -p 8090:8090 -e  network.config.path=/tmp/connectionProfile.json --env JAVA_OPTS="-Dspring.config.location=/tmp/application.properties;-Dlogging.level.org.springframework=DEBUG" --name lender1 amarofs/lender1

https://hub.docker.com/r/amarofs/ibp-lender2


* docker run -v /home/ofsadmin/ibp-devops/connectionProfile.json:/tmp/connectionProfile.json -v /home/ofsadmin/ibp-devops/application.properties:/tmp/application.properties -p 8090:8090 -e  network.config.path=/tmp/connectionProfile.json --env JAVA_OPTS="-Dspring.config.location=/tmp/application.properties;-Dlogging.level.org.springframework=DEBUG" --name lender2 amarofs/lender2

###### Notes: 

i. /home/ofsadmin/ibp-devops/connectionProfile.json -> The connection profile need to be downloaded from “IBM Network” and should append the channel details in this JSON.

ii. /home/ofsadmin/ibp-devops/application.properties -> Update custom configurations( peer name, channel details, Gateway URL) in this file and have it in host machine. 

iii. /tmp/connectionProfile.json && /tmp/application.properties -> This files will be available in Docker container. 

iv. network.config.path -> Provide the path which mentioned in the -v argument connection profile(probably /tmp/connectionProfile.json).

v. JAVA_OPTS -> The java configurations will be used while running the jar in docker images.

####  4.Partner Services
https://hub.docker.com/r/amarofs/ibp-lender2

* docker run -v /home/ofsadmin/ibp-devops/connectionProfile.json:/tmp/connectionProfile.json -v /home/ofsadmin/ibp-devops/application.properties:/tmp/application.properties -p 8090:8090 -e  network.config.path=/tmp/connectionProfile.json --env JAVA_OPTS="-Dspring.config.location=/tmp/application.properties;-Dlogging.level.org.springframework=DEBUG" --name partner amarofs/partner

###### Notes: 

i. /home/ofsadmin/ibp-devops/connectionProfile.json -> The connection profile need to be downloaded from “IBM Network” and should append the channel details in this JSON.

ii. /home/ofsadmin/ibp-devops/application.properties -> Update custom configurations( peer name, channel details, Gateway URL) in this file and have it in host machine. 

iii. /tmp/connectionProfile.json && /tmp/application.properties -> This files will be available in Docker container. 

iv. network.config.path -> Provide the path which mentioned in the -v argument connection profile(probably /tmp/connectionProfile.json).

v. JAVA_OPTS -> The java configurations will be used while running the jar in docker images.

####  5.The Web Application
https://github.com/objectfrontiergit/IBP-LoanMgtUI

 Make sure you keep 
 *src/main/resources/application.properties* file upto date matching your network details

**_2._** **_UI Application Setup:_**


To update the services URL open this file in UI repo(src/app/http.service.ts) and change the API services URL.

i. Clone UI repo URL.

ii. open cmd(cli) and change directory to your repo path

iii. npm install (it'll fetch angular dependencies)

iv. npm run start (to start the App)

v. ng build (to create build file, the build folder will be generated in dist folder. Deploy this folder for UI.

## OFS support
Please reachout to ofstechsupport@objectfrontier.com for any questions about the demo and codebase.
