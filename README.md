
<h1>Developer tutorial for creating a Hyperledger Composer solution</h1><hr>

<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

<a href="https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial.html">Developer tutorial for creating a Hyperledger Composer solution</a>

Pre-requisites

# Step One: Creating a business network structure
# Step Two: Defining a business network
# Step Three: Generate a business network archive
# Step Four: Deploying the business network
# Step Five: Generating a REST server
# Step Six: Generating an application

<p>
<b>yo hyperledger-composer:businessnetwork</b> 
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
</b>
Save your changes to org.example.mynetwork.cto.
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

<h2>Adding JavaScript transaction logic</h2>

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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

<h2>Adding access control</h2>

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
<h1>Deplying the Business Network</h1>

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




