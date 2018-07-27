
<h1>Developer tutorial for creating a Hyperledger Composer solution</h1><hr>

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

<a href="https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html">Developer tutorial for creating a Hyperledger Composer solution</a>

Pre-requisites
<p>
<b>yo hyperledger-composer:businessnetwork<b> 
<p>
Open org.example.mynetwork.cto model file.
  
~~~~
  /**
 * My commodity trading network
 */
namespace org.example.mynetwork
asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    o Double quantity
    --> Trader owner
}
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}
transaction Trade {
    --> Commodity commodity
    --> Trader newOwner
}
~~~~  

Save your changes to org.example.mynetwork.cto.

<p><h2>Adding JavaScript transaction logic</h2>

Open the logic.js file

And replace the code with this one and save.
~~~~
/**
 * Track the trade of a commodity from one trader to another
 * @param {org.example.mynetwork.Trade} trade - the trade to be processed
 * @transaction
 */
async function tradeCommodity(trade) {
    trade.commodity.owner = trade.newOwner;
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}
~~~~

<h2>Adding access control</h2>
<hr>
In the file permissions.jcl
~~~~

/**
 * Access control rules for tutorial-network
 */
rule Default {
    description: "Allow all participants access to all resources"
    participant: "ANY"
    operation: ALL
    resource: "org.example.mynetwork.*"
    action: ALLOW
}

rule SystemACL {
  description:  "System ACL to permit all access"
  participant: "ANY"
  operation: ALL
  resource: "org.hyperledger.composer.system.**"
  action: ALLOW
}

~~~~

<h2>Step Three: Generate a business network archive
</h2>
<ol>
<li>Using the command line, navigate to the tutorial-network directory.
<p><li>From the tutorial-network directory, run the following command:
</ol>
  
<b>composer archive create -t dir -n . </b>

After the command has run, a business network archive file called tutorial-network@0.0.1.bna has been created in the tutorial-network directory.

<h1>
<a href="https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html">Installing the development environment</a></a>

Developer tutorial for creating a Hyperledger Composer solution
<p>
This tutorial will walk you through building a Hyperledger Composer blockchain solution from scratch. In the space of a few hours you will be able to go from an idea for a disruptive blockchain innovation, to executing transactions against a real Hyperledger Fabric blockchain network and generating/running a sample Angular 2 application that interacts with a blockchain network.
<p>
This tutorial gives an overview of the techniques and resources available to apply to your own use case.
Note: This tutorial was written against the latest Hyperledger Composer build on Ubuntu Linux running with Hyperledger Fabric v1.1 where referenced below and also tested for a Mac environment.
<p>
Prerequisites
Before beginning this tutorial:
•	Set up your development environment
•	Install an editor e.g. VSCode or Atom
Step One: Creating a business network structure
The key concept for Hyperledger Composer is the business network definition (BND). It defines the data model, transaction logic and access control rules for your blockchain solution. To create a BND, we need to create a suitable project structure on disk.
The easiest way to get started is to use the Yeoman generator to create a skeleton business network. This will create a directory containing all of the components of a business network.
1.	Create a skeleton business network using Yeoman. This command will require a business network name, description, author name, author email address, license selection and namespace.
Copy
  
<b>yo hyperledger-composer:businessnetwork</b>

2.	Enter tutorial-network for the network name, and desired information for description, author name, and author email.
3.	Select Apache-2.0 as the license.
4.	Select org.example.mynetwork as the namespace.
5.	Select No when asked whether to generate an empty network or not.
Step Two: Defining a business network
A business network is made up of assets, participants, transactions, access control rules, and optionally events and queries. In the skeleton business network created in the previous steps, there is a model (.cto) file which will contain the class definitions for all assets, participants, and transactions in the business network. The skeleton business network also contains an access control (permissions.acl) document with basic access control rules, a script (logic.js) file containing transaction processor functions, and a package.json file containing business network metadata.
Modelling assets, participants, and transactions
The first document to update is the model (.cto) file. This file is written using the Hyperledger Composer Modelling Language. The model file contains the definitions of each class of asset, transaction, participant, and event. It implicitly extends the Hyperledger Composer System Model described in the modelling language documentation.
1.	Open the org.example.mynetwork.cto model file.
2.	Replace the contents with the following:
Copy
/**
 * My commodity trading network
 */
namespace org.example.mynetwork
asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    o Double quantity
    --> Trader owner
}
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}
transaction Trade {
    --> Commodity commodity
    --> Trader newOwner
}
3.	Save your changes to org.example.mynetwork.cto.
Adding JavaScript transaction logic
In the model file, a Trade transaction was defined, specifying a relationship to an asset, and a participant. The transaction processor function file contains the JavaScript logic to execute the transactions defined in the model file.
The Trade transaction is intended to simply accept the identifier of the Commodity asset which is being traded, and the identifier of the Trader participant to set as the new owner.
1.	Open the logic.js script file.
2.	Replace the contents with the following:
Copy
/**
 * Track the trade of a commodity from one trader to another
 * @param {org.example.mynetwork.Trade} trade - the trade to be processed
 * @transaction
 */
async function tradeCommodity(trade) {
    trade.commodity.owner = trade.newOwner;
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}
3.	Save your changes to logic.js.
Adding access control
1.	Replace the following access control rules in the file permissions.acl :
Copy
/**
 * Access control rules for tutorial-network
 */
rule Default {
    description: "Allow all participants access to all resources"
    participant: "ANY"
    operation: ALL
    resource: "org.example.mynetwork.*"
    action: ALLOW
}

rule SystemACL {
  description:  "System ACL to permit all access"
  participant: "ANY"
  operation: ALL
  resource: "org.hyperledger.composer.system.**"
  action: ALLOW
}
2.	Save your changes to permissions.acl.


Step Three: Generate a business network archive
Now that the business network has been defined, it must be packaged into a deployable business network archive (.bna) file.
1.	Using the command line, navigate to the tutorial-network directory.
2.	From the tutorial-network directory, run the following command:
Copy
composer archive create -t dir -n .
After the command has run, a business network archive file called tutorial-network@0.0.1.bna has been created in the tutorial-network directory.
Step Four: Deploying the business network
After creating the .bna file, the business network can be deployed to the instance of Hyperledger Fabric. Normally, information from the Fabric administrator is required to create a PeerAdmin identity, with privileges to install chaincode to the peer as well as start chaincode on the composerchannel channel. However, as part of the development environment installation, a PeerAdmin identity has been created already.
After the business network has been installed, the network can be started. For best practice, a new identity should be created to administer the business network after deployment. This identity is referred to as a network admin.
Retrieving the correct credentials
A PeerAdmin business network card with the correct credentials is already created as part of development environment installation.
Deploying the business network
Deploying a business network to the Hyperledger Fabric requires the Hyperledger Composer business network to be installed on the peer, then the business network can be started, and a new participant, identity, and associated card must be created to be the network administrator. Finally, the network administrator business network card must be imported for use, and the network can then be pinged to check it is responding.
1.	To install the business network, from the tutorial-network directory, run the following command:
Copy
composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network@0.0.1.bna
The composer network install command requires a PeerAdmin business network card (in this case one has been created and imported in advance), and the the file path of the .bna which defines the business network.
2.	To start the business network, run the following command:
Copy
composer network start --networkName tutorial-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card
The composer network start command requires a business network card, as well as the name of the admin identity for the business network, the name and version of the business network and the name of the file to be created ready to import as a business network card.
3.	To import the network administrator identity as a usable business network card, run the following command:
Copy
composer card import --file networkadmin.card
The composer card import command requires the filename specified in composer network start to create a card.
4.	To check that the business network has been deployed successfully, run the following command to ping the network:
Copy
composer network ping --card admin@tutorial-network
The composer network ping command requires a business network card to identify the network to ping.
Step Five: Generating a REST server
Hyperledger Composer can generate a bespoke REST API based on a business network. For developing a web application, the REST API provides a useful layer of language-neutral abstraction.
1.	To create the REST API, navigate to the tutorial-network directory and run the following command:
Copy
composer-rest-server
2.	Enter admin@tutorial-network as the card name.
3.	Select never use namespaces when asked whether to use namespaces in the generated API.
4.	Select No when asked whether to secure the generated API.
5.	Select Yes when asked whether to enable event publication.
6.	Select No when asked whether to enable TLS security.
The generated API is connected to the deployed blockchain and business network.
Step Six: Generating an application
Hyperledger Composer can also generate an Angular 4 application running against the REST API.
1.	To create your Angular 4 application, navigate to tutorial-network directory and run the following command:
Copy
yo hyperledger-composer:angular
2.	Select Yes when asked to connect to running business network.
3.	Enter standard package.json questions (project name, description, author name, author email, license)
4.	Enter admin@tutorial-network for the business network card.
5.	Select Connect to an existing REST API
6.	Enter http://localhost for the REST server address.
7.	Enter 3000 for server port.
8.	Select Namespaces are not used
The Angular generator will then create the scaffolding for the project and install all dependencies. To run the application, navigate to your angular project directory and run npm start . This will fire up an Angular 4 application running against your REST API at http://localhost:4200 .

