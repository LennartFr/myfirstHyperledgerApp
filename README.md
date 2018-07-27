
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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">

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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">
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
<img src="https://farm5.staticflickr.com/4503/37148677233_71edc5a37b_o.png" width="1041" height="53" alt="blueband">
<h2>Step Three: Generate a business network archive
</h2>
<ol>
<li>Using the command line, navigate to the tutorial-network directory.
<p><li>From the tutorial-network directory, run the following command:
</ol>
  
<b>composer archive create -t dir -n . </b>

After the command has run, a business network archive file called tutorial-network@0.0.1.bna has been created in the tutorial-network directory.


