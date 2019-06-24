# B2B Mortage Origination Demo using IBM Blockchain Platform

The objective is to develop a Decentralized Application that explores potential business benefits of using blockchain like

-   Improved process efficiency
-   Economy of scale
-   Privacy when you need it
-   Transparency and trust
-   Decentralized data with a logically centralized view
-   Recording of payments onto a blockchain

The use case we picked up to meet our objectives is a Loan Origination and Payment Application. The actual implementation of the use case doesn’t cover all intricacies and regulatory aspects of the loan origination and payments process but will be a good indicator of the design aspects to consider while building a DApp on the blockchain
# The DEMO

![](https://youtu.be/w2GT9wH-S3o)

# The Blockchain N/W Architecture of the Demo

![](https://lh6.googleusercontent.com/YVc-fNbv4L9WOcwe3IoTBOH_8I7D9SyBUuUrQZCRSalWgduXV9dd6Hf65PAqlW0nIPFjOHxQHd1KDEPMlMbrjztFRlGwyGs_A4MgLgInJd1FLLlD0iDOWoQZm-m-rulcbhs1tzv-)

## Application Architecture

The design of the Blockchain network is just one piece of the puzzle. Application design and architecture is equally important. Consider the following as you design your application:

-   Although IBM Blockchain Platform with Hyperledger Fabric at its heart scales well when compared to the public blockchain platforms, it is not a transactional database, so don’t expect throughputs of 
-   Use the blockchain for what it has to offer — trust, transparency, and immutability. Decide what you store in your blockchain based on that.
-   Embrace asynchronous event-driven programming — the blockchain nodes are only eventually consistent. The consensus process is not instantaneous.
-   The data in the blockchain is secured by design through encryption and various governing principles of the network, but the security stops at the blockchain network level. You should still address the application security with your design.
-   Think about GDPR-like regulations before you store personally identifiable information onto the blockchain.

## Design

We followed a modular microservices architecture. The complexity of interaction with the blockchain itself is abstracted out to a generic component that knows how to invoke chaincode to read/write data from/to blockchain. This helps to keep the other services lightweight and flexible.

![](https://lh3.googleusercontent.com/gsV3mqDKp5GPZ-07hC4se-RUp8wOq4Ezm6RNdsamDLZlHkfolXzZ39bJWhrsxU1MIkLgdmQpOsmV1K2UMkRRPUBAojfBtm7Z8-vhNvqPriHg0PYtKVE4F0AFSl0GqR-WB4LFH7UO)

## Data Flow

-   The microservices expose REST APIs for the presentation layer to consume.
-   The services delegate calls to the Hyperledger Gateway for blockchain invocations.
-   The blockchain calls are asynchronous. Once the transactions are added to the blockchain the Event Hub pushes the signal to all interested subscribers through a callback endpoint.
-   We use WebSockets for the presentation layer to react to the events from the Event hub.

![](https://lh5.googleusercontent.com/Eh80OvHl9ZIPP322cnP3Y92NQqN_xNr1aZg4KMDWVheNLQwvds-FQu7Kuki5CIglyyv3aeD6Ct0Kg1PVtp-9X3iIE2aU2I4DfsxAKGN57LtaDaiQDLvYmaESoua71t-TQPuNABvT)

## more here
https://www.objectfrontier.com/blog/ibm-blockchain-platform-2-0-beta-run-your-blockchain-n-w-in-10-simple-steps/

https://www.objectfrontier.com/blog/demystifying-the-fabric-chaincode-with-an-example/

https://www.objectfrontier.com/blog/invoking-fabric-chaincode-from-your-application/

## Microservices Code Repositories

Hyperledger Gateway Services https://github.com/objectfrontiergit/IBP-Gateway 

Applicant Services
https://github.com/objectfrontiergit/IBP-ApplicantAPI

Lender Services
https://github.com/objectfrontiergit/IBP-Lender2API
https://github.com/objectfrontiergit/IBP-BankAPI

Partner Services
https://github.com/objectfrontiergit/IBP-MortgageAPI

The Web Application
https://github.com/objectfrontiergit/IBP-LoanMgtUI

## contact
Please reachout to ganeshram.ramamurthy@objectfrontier.com for any questions about the demo and codebase.
