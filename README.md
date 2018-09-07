
<h1>Developer tutorial for creating a Hyperledger Composer solution</h1><hr>

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

<a href="https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html">Developer tutorial for creating a Hyperledger Composer solution</a>

# Sources

https://medium.com/coinmonks/fixing-transaction-issue-in-angular-for-hyperledger-fabric-blockchain-application-fe7e28a7bb6e

https://medium.com/coinmonks/building-a-blockchain-application-using-hyperledger-fabric-with-angular-frontend-part-2-22ef7c77f53

https://medium.freecodecamp.org/ultimate-end-to-end-tutorial-to-create-an-application-on-blockchain-using-hyperledger-3a83a80cbc71

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">


# Install pre-requisites for Ubuntu or Mac OS

https://hyperledger.github.io/composer/v0.19/installing/installing-prereqs.html#macos

nvm use node  // Please note, very important, do this, without this the code won't work!

# Step One: Install the CLI Tools:

https://hyperledger.github.io/composer/v0.19/installing/development-tools.html

Essential CLI tools:

<ol>
<li> npm install -g composer-cli@0.19 <br>
Utility for running a REST Server on your machine to expose your business networks as RESTful APIs:

<li>npm install -g composer-rest-server@0.19<br>
Useful utility for generating application assets:

<li>npm install -g generator-hyperledger-composer@0.19<br>

Yeoman is a tool for generating applications, which utilises generator-hyperledger-composer:
<li> npm install -g yo
</ol>

# Alternative mass install

npm install -g composer-cli@0.19 
npm install -g composer-rest-server@0.19
npm install -g generator-hyperledger-composer@0.19
npm install -g yo

# Step Two: Install Playground

npm install -g composer-playground@0.19

# Step 3: Set up your IDE

# Step 4: Install Hyperledger Fabric

~~~~
mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers

curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
tar -xvf fabric-dev-servers.tar.gz
~~~~

~~~~
cd ~/fabric-dev-servers
export FABRIC_VERSION=hlfv11
./downloadFabric.sh
~~~~

# Step Two: Install Hyperledger Fabric

After installing the prerequisites and development environment, make sure your docker is running and then run

./startFabric.sh and ./createPeerAdminCard.sh


# Step Three: Install Playground

~~~~
npm install -g composer-playground@0.19
~~~~

# Step 3: Set up your IDE

# Step 4: Install Hyperledger Fabric

~~~~
mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers

curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
tar -xvf fabric-dev-servers.tar.gz

cd ~/fabric-dev-servers
export FABRIC_VERSION=hlfv11
./downloadFabric.sh

~~~~

## Starting and stopping Hyperledger Fabric

cd ~/fabric-dev-servers
    ./startFabric.sh
    ./createPeerAdminCard.sh


<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">


# Step Three: Creating a business network structure
<p>
https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html

yo hyperledger-composer:businessnetwork 
<p>Enter tutorial-network for the network name, and desired information for description, author name, and author email.
<p>Select Apache-2.0 as the license.
Select org.example.mynetwork as the namespace.
<p>Select No when asked whether to generate an empty network or not.
    
    
    
    
  
# Step 2: Defining a business network

https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html

Open org.example.mynetwork.cto model file and replace with the following content.
  
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
</b>
Save your changes to org.example.mynetwork.cto.
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

### Adding JavaScript transaction logic

Open the logic.js file and replace with the following content

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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

### Adding access control

Open the file permissions.jcl and replace with the following content:
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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

## Step Three: Generate a business network archive file.
</h2>
<ol>
<li>Using the command line, navigate to the tutorial-network directory.
<p><li>From the tutorial-network directory, run the following command:
</ol>
  
<b>composer archive create -t dir -n . </b>

After the command has run, a business network archive file called tutorial-network@0.0.1.bna has been created in the tutorial-network directory.

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">
<h1>Deploy the Business Network</h1>

To install the business network, from the tutorial-network directory, run the following command:

<p>
  <b>composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network@0.0.1.bna</b>
<p>
The composer network install command requires a PeerAdmin business network card (in this case one has been created and imported in advance), and the the file path of the .bna which defines the business network.

<p>
  <b>To start the business network, run the following command</b>
<p>
Copy
composer network start --networkName tutorial-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card
The composer network start command requires a business network card, as well as the name of the admin identity for the business network, the name and version of the business network and the name of the file to be created ready to import as a business network card.

<p>
<b>To import the network administrator identity as a usable business network card, run the following command:</b>
<p>

<p>
Copy
<b>composer card import --file networkadmin.card</b>
<p>
The composer card import command requires the filename specified in composer network start to create a card.

To check that the business network has been deployed successfully, run the following command to ping the network:

<p>
Copy
<b>composer network ping --card admin@tutorial-network</b>

<p>

## Step Five: Generating a REST server
Hyperledger Composer can generate a bespoke REST API based on a business network. For developing a web application, the REST API provides a useful layer of language-neutral abstraction.

To create the REST API, navigate to the tutorial-network directory and run the following command:
<p><b>
composer-rest-server
  </b>
  <li>
<ol>Enter admin@tutorial-network as the card name.

<li>Select never use namespaces when asked whether to use namespaces in the generated API.
<li>Select No when asked whether to secure the generated API.
<li>Select Yes when asked whether to enable event publication.
<li>Select No when asked whether to enable TLS security.
</ol>

The generated API is connected to the deployed blockchain and business network.

<img src="https://github.com/LennartFr/myfirstHyperledgerApp/blob/master/Screen%20Shot%202018-07-27%20at%2015.32.39.png">


