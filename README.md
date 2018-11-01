# vehicle-demo-using-blockchain-starter-plan


In this pattern, see how to create a network on the IBM Blockchain Platform, install a smart contract (chaincode) onto a peer in the network, and run a sample application to invoke it. You'll see how to populate the shared ledger and communicate with it by making calls from a local client application to query and update the ledger.

The IBM Blockchain Platform Starter Plan is a fully integrated and enterprise-ready service running on the IBM Cloud. The platform is designed to accelerate the development, governance, and operation of a multi-institution business network. Starter Plan is specifically aimed at test and development scenarios rather than production ones. For production scenarios you should use the Enterprise Plan.
The sample application we'll use in this tutorial is a Hyperledger Fabric sample called "fabcar" and, while you review the sample and run it locally in a Docker container, in this tutorial you'll see how to install and run it on the IBM Blockchain Platform Starter Plan on the IBM Cloud. Hyperledger is a global, open source, collaborative effort, hosted by The Linux Foundation, to advance cross-industry blockchain technologies. Hyperledger Fabric is a Hyperledger project and framework implementation whose modular architecture powers the IBM Blockchain Platform Starter Plan.


This code pattern is for developers looking to start building blockchain applications using the IBM Blockchain Platform Starter Plan. When the reader has completed this code pattern, they will understand how to:
* Create a network on the IBM Blockchain Platform
* Install a smart contract (chaincode) onto a peer in the network
* Run a sample application to query and update the ledger


## Architecture Flow
![Architecture Flow](xxxxxxxx)

1. The buyer of the vehicle views the catalog of cars on his dashboard.
2. They personalize the vehicle by selecting model, exterior and interior options and other packages/add-ons available withthe vehicle.
3. They submit the order.
4. The order is received by the manufacturer on their dashboard where they view the progress of the assembly of the vehicle and the shipment status.
5. The vehicle regulatory views and tracks all details and changes with respect to the order on the blockchain, allowing maximum transparency.


## Prerequisites
* Docker:
* Node.js
* IBM Cloud account

## Included Components
* [Hyperledger Composer v0.19.4](https://hyperledger.github.io/composer/latest/) Hyperledger Composer is an extensive, open development toolset and framework to make developing blockchain applications easier
* [Hyperledger Fabric v1.1](https://hyperledger-fabric.readthedocs.io) Hyperledger Fabric is a platform for distributed ledger solutions, underpinned by a modular architecture delivering high degrees of confidentiality, resiliency, flexibility and scalability.
* [IBM Blockchain Starter Plan](https://console.bluemix.net/catalog/services/blockchain) The IBM Blockchain Platform Starter Plan allows to build and try out blockchain network in an environment designed for development and testing

## Featured Technologies
* [Nodejs](https://www.python.org/) Node.js is an open-source, cross-platform JavaScript run-time environment that executes JavaScript code server-side


## Steps
1. [Clone the repo](#1-clone-the-repo)
2. [Launch Hyperledger Fabric Network and Node App](#2-launch-hyperledger-fabric-network-and-node-app)
3. [Create the Network Channel and Invoke a Transaction](#3-Create-the-Network-Channel-and-Invoke-a-Transaction)

# Running the Application
Use the ``Deploy to IBM Cloud`` button **OR** manually deploy to IBM Cloud.

## Directly deploy to IBM Cloud
[![Deploy to IBM Cloud](https://bluemix.net/deploy/button.png)](https://console.bluemix.net/devops/setup/deploy/?repository=https%3A//github.com/ibm-blockchain/vehicle-manufacture&branch=master&env_id=ibm%3Ayp%3Aus-south&deploy-region=ibm%3Ayp%3Aus-south)

## Manually deploy to IBM Cloud
1. [Setup your machine](#1-setup-your-machine)
2. [Clone the repository](#2-clone-the-repository)
3. [Create the BNA File](#3-create-the-bna-file)
4. [Create a Blockchain Service](#4-create-a-blockchain-service)
5. [Configure certificates](#5-configure-certificates)
6. [Install and start the network](#6-install-and-start-the-network)
7. [Provision Cloudant](#7-provision-cloudant)
8. [Deploy the applications](#8-deploy-the-applications)
9. [Start the applications](#9-start-the-applications)

### 1. Create a network on the IBM Blockchain Platform Starter Plan
To complete this tutorial, you need an IBM Cloud account. Whilst use of the IBM Blockchain Platform Starter Plan is currently free whilst the service is in beta, an account is required for logging in. Please note that the Platform is updated often, and the screen shots in this tutorial may not exactly match the ones you see.

Starting on the [Rapidly build with IBM Blockchain Platform](xx) page, select `Service Plans`. Make sure you are logged-in and choose `US South` as the region as at the time of writing Starter Plan is only deployed to that region. Enter `Blockchain-demo` as the Service name, then select `Starter Plan Membership` and click `Create`. 

![create blockchain service](readme-images/service-name.png)

You can now see the `Network created!`` screen. Click `Launch` from this screen to see the dashboard for your network.

![launch network](readme-images/launch-network.png)

The next screen is the getting started “welcome screen”. Click “Got it” when you have finished reading.

![Learn more](readme-images/learn-more.png)

If you look at the top left you will see that your network has been given a generated name. Click on the name and change it to `fabcar`.

![Learn more](readme-images/set-network-name1.png)
![Learn more](readme-images/set-network-name2.png)

Once you have changed the name you will get a pop-up in the top right for a few seconds informing you that the change has been successful:

![Learn more](readme-images/success-message.png)

Out of the box, Starter Plan creates you a working simple network. If you select the `Overview` tab you can see an overview of what was created for you automatically:

![Learn more](readme-images/overview-tab.png)

The network actually consists of two organizations Company A (Org1) and Company B (Org2), although you are logged on as Org1 by default and so can only see the `Orderer` service which is shared between both orgs, and a Certificate Authority (CA) and a Peer for “org1”.
If you select the “Members” tab on the left you can see these members in a little more detail:

![Learn more](readme-images/members-tab.png)

If you select the `Channels` tab on the side you will see that there is also a channel called `defaultchannel` that has been created as well:

![Learn more](readme-images/channel.png)

For this tutorial we will use this defaultchannel for simplicity. Click on the “defaultchannel” row in the table to see more details on the channel:

![Learn more](readme-images/default-channel.png)

Here you can see that there are already 3 blocks on the chain held by the channel that store the initial configuration information. You can select each row to see more details if you wish.

### 2. Install chaincode on the channel

The next step in this tutorial is to deploy the chaincode to the channel and first we have to get the code to install from a github repository using git clone.

In order to complete this next step you will need to have `git` installed on your local machine. Open a command-line or terminal window, navigate to a suitable directory and create a new directory called `fabcar`. Navigate to the new ’fabcar’ directory and run the following command to clone the fabric samples source:

`git clone https://github.com/hyperledger/fabric-samples.git`

This will copy the code for all the samples to your local machine. Now we have the code we need to deploy it. Using the Starter Plan UI, select the “Install Code” tab from the left hand sidebar to see the `Install code` screen:

![Learn more](readme-images/install-code.png)

Click the drop-down called `Choose-peer` and select `org1-peer1`. Now click the `Install Chaincode` button.

![Learn more](readme-images/install-chaincode.png)

On the `Install chaincode on org1peer1` dialog, enter “fabcar” for the `Chaincode ID`, enter `v1` for the `Chaincode Version`.

You have a choice to deploy either the Nodejs or Golang version of the `fabcar` chaincode. Both provide the same functionality, so the choice comes down in part to skills. Choose `Node` or `Golang` for the `Chaincode Type`.

Finally, on this screen click `Choose files` to select the chaincode source to install.

If you want to use the go version, select the `fabcar.go` chaincode single file from the `fabric-samples/chaincode/fabcar/go` folder that you downloaded from GitHub. However, if you prefer to use the node version, select both the `fabcar.js` file and the `package.json` files from the `fabric-samples/chaincode/fabcar/node` folder that was also downloaded from github.

Example install of the go version of the fabcar chaincode:

![Learn more](readme-images/install-gochaincode.png)

Example install of the node version of the fabcar chaincode:

![Learn more](readme-images/install-nodechaincode.png)

Once you have made your choice, click the Submit button to upload the chaincode to Starter Plan. 
Next you need to instantiate the chaincode. Click on the three dots in the `Actions` column to see the menu and choose `Instantiate`.

![Learn more](readme-images/instantiate-chaincode.png)

On the `Instantiate chaincode` dialog, there are no arguments to provide for fabcar as it does not need any. However, you do need to select `defaultchannel` from the `Channel` drop-down and match the `Chaincode Type` to the language (Node or Golang) of the code you uploaded above. Then click `Submit`.

![Learn more](readme-images/submit-instantiate-chaincode.png)

Once this completes, you can select the `fabcar` row to see that the chaincode is now instantiated on the defaultchannel:

![Learn more](readme-images/chaincode-on-defaultchannel.png)

Now the chaincode has been instantiated on the channel, select the Channels side tab and select the `defaultchannel` row to see its details:

![Learn more](readme-images/chaincode-channel-complete.png)

You can now see there is one extra block on the channel which is the record of the instantiate operation we performed above. Now select the `Chaincode` tab on this screen and select the `fabcar` row to expand it. You can now see two buttons under fabcar:

![Learn more](readme-images/json-credentials.png)

**JSON**: This JSON file holds the credentials and peer information for the blockchain network.

**Delete**: This stops and deletes the chaincode instance.

Next, in the `fabric-samples/fabcar` folder that was downloaded from github, create a new folder called **config**. Click the **JSON** button shown above, and a new tab will open. Copy all the data shown in this tab into a new file called `network-profile.json` and save this file inside the `fabric-samples/fabcar/config` folder you just created.


### 3. Configure your application to run on the IBM Blockchain Platform

The fabcar applications you downloaded from github are hard-coded to use a local instance of Fabric. Because we are using Fabric located remotely on IBM Blockchain Platform, this article comes with new versions of the fabcar apps that will connect securely to the remote platform using the ‘network-profile.json’ configuration file you just download and saved above. The original four fabcar applications are called:

- enrollAdmin.js

- registerUser.js

- invoke.js

- query.js

The zip file that comes with this article contains new versions of these files called:

- enrollAdminNetwork.js
- registerUserNetwork.js
- invokeNetwork.js
- queryNetwork.js

These new files all end with the word `Network` to indicate these are the new versions that will access fabcar remotely and to ensure they will not replace the original files. The zip also contains one extra configuration file called:

- client-profile.json

The `client-profile.json` file contains some extra configuration needed to access the remote fabcar chaincode.

Your next step is to extract these files from the zip file and put them into the correct locations. The first four files ending in `Network` should be placed into the `fabric-samples/fabcar` folder alongside the original four files from githhub. The `client-profile.json` file should be placed into the `fabric-samples/fabcar/config` folder alongside the `network-profile.json` file you downloaded and saved earlier.

**Run npm install**
The fabcar client application is written in Node.js. In order to complete this next step, you will need to have `Node.js` and `npm` installed on your local machine. *Note that currently Node.js v7 is not supported and you need to use v6.9.0 or earlier.*  Return to the command-line or terminal window and run the npm install command from within the fabcar folder:

`cd fabric-samples/fabcar`

`npm install`

On Windows, you may need to install the Windows Build Tools if you get build errors:

`npm install --global windows-build-tools`

All the prerequisite node packages are now installed in order to run the fabcar client application.

*Enrolling an Admin*

When your network was created in Starter Plan, each organisation had an Administrator user called “admin” automatically registered with the Certificate Authority (CA). You now need to send an enrolment request to the CA to retrieve their enrolment certificate (eCert). From within the fabcar folder run:

`node enrollAdminNetwork.js`

You should see output like this:

```
> node enrollAdminNetwork.js
Found organization: org1 and ca name: org1-ca
Enrolling using enrollmentId: admin and EnrollmentSecret: 2d87ae1b59
Successfully enrolled admin user "admin" with msp: "org1
The enrollAdminNetwork.js application will create a local public/private key pair in a folder it creates called ‘hfc-key-store’ and send a Certificate Signing Request (CSR) to the remote CA for org1 to issue the eCert. The eCert, along with some metadata, will also be stored in the ‘hfc-key-store’ folder. The connection details of where the CA is located and the TLS certificate needed to connect to it are all obtained by the application from the ‘network-profile.json’ you downloaded earlier.

```

**Enrolling a new user**

Using the eCert for the admin user we will now register and enrol a new user called ‘user1’ with the CA. This user will be the one whose identity we will use to update and query the ledger. From within the fabcar folder run:

`node registerUserNetwork.js`

You should see output like this:

```
> node registerUserNetwork.js
Successfully loaded admin from persistence
Successfully registered "user1" - with secret:gTYshgoNxoLH
Successfully enrolled member user "user30" with msp: "org1"
"user1" was successfully registered and enrolled and is ready to interact with the fabric network
Like the previous command this application has created a new public/private key pair and sent a CSR request to the CA to issue the eCert for user1. If you look in the ‘hlf-key-store’ folder you should see six files, three for each identity.

```

**Initialize the ledger**

The original fabcar sample from github has a script that automatically calls an `initledger` transaction to populate the ledger with 10 initial vehicles to get you started quickly. Because we are not using this script, we need to call this transaction ourselves. The `invokeNetwork.js` command has been written to call this directly so we can just run this command next:


`node invokeNetwork.js`

You should see output that ends like this:

```
...

Successfully committed the change to the ledger by the peer

```

As the chaincode gets run in a separate chaincode docker container, it can take sometime to start this container on first use. If you get a timeout or a `premature execution` error, just try running the command again. If you take a look at the `Channel Overview` after the command has completed successfully, you should see there is now an extra block on the ledger:

![Learn more](readme-images/ledger.png)

**Query the ledger**

The `invokeNetwork.js` command has now populated the ledger with sample data for 10 cars, so let's query the ledger to see the data. To do this we are going to run the queryNetwork.js command which is set up to query all cars that exist on the ledger:

`node queryNetwork.js`

You should see output like this:

```
> node queryNetwork.js
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},{"Key":"CAR1", "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},{"Key":"CAR2", "Record":{"colour":"green","make":"Hyundai","model":"Tucson","owner":"Jin Soo"}},{"Key":"CAR3", "Record":{"colour":"yellow","make":"Volkswagen","model":"Passat","owner":"Max"}},{"Key":"CAR4", "Record":{"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}},{"Key":"CAR5", "Record":{"colour":"purple","make":"Peugeot","model":"205","owner":"Michel"}},{"Key":"CAR6", "Record":{"colour":"white","make":"Chery","model":"S22L","owner":"Aarav"}},{"Key":"CAR7", "Record":{"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}},{"Key":"CAR8", "Record":{"colour":"indigo","make":"Tata","model":"Nano","owner":"Valeria"}},{"Key":"CAR9", "Record":{"colour":"brown","make":"Holden","model":"Barina","owner":"Shotaro"}}]

```

**Update the ledger**

Finally, let's make an update to the ledger. To do this, you need to make a simple change to the invokeNetwork.js command.
Open the invokeNetwork.js file in an editor of your choice such as atom or VSCode, then find and edit the request variable as shown below so that it will invoke the `createCar` chaincode with a set of arguments that describe the car to be created. The changed request variable should look like this: 

```
var request = {
  chaincodeId: 'fabcar',
  fcn: 'createCar',
  args: ['CAR10', 'Honda', 'Accord', 'Black', 'Dave'],
  txId: tx_id
};

```

Save the file and run the edited command again using node invokeNetwork.js. The expected output is: 

```
…
Successfully committed the change to the ledger by the peer.
This has created a new Honda vehicle with the owner "Dave" and stored it on the ledger. You can see the new car on the ledger by running the queryNetwork.js command again as you did before. You can now experiment with creating new cars on the ledger with different names and owners. If you look at the “Channel Overview” you should see new blocks added as you create new cars.
Finally, you may want to experiment with the changeOwner transaction to change the owner of a vehicle. To do this, change the request variable in invokeNetwork.js again to look like this: 
var request = {
  chaincodeId: 'fabcar',
  fcn: 'changeCarOwner',
  args: ['CAR10', 'MGK'],
  txId: tx_id
};

```

Now save the file and run the command again using node invokeNetwork.js. The expected output is: 

```
…
Successfully committed the change to the ledger by the peer
You can see the updated owner on the ledger by running the queryNetwork.js command again. You can see the owner of CAR10 has changed from "Dave" to "MGK".
If you want to query for a single car rather than for all cars, make this change to the request variable in the queryNetwork.js command and rerun it: 
const request = {
  chaincodeId: 'fabcar',
  fcn: 'queryCar',
  args: ['CAR10']
};
```

You should now see the information for a single car:

```
> node queryNetwork.js
Successfully loaded user1 from persistence
Query has completed, checking results
Response is {"colour":"Black","make":"Honda","model":"Accord","owner":"MGK"}
```

### 4. Summary
You now have a running network on the IBM Blockchain Platform Starter Plan, with a sample chaincode deployed to a peer and instantiated on a channel. You also have a running application that you can easily work with locally. You've populated the ledger with sample data, and your application can now communicate (query and update) with the blockchain on the IBM Blockchain Platform. Happy blockchaining!

### 5. Acknowledgments
The authors thank Anthony O'Dowd of the IBM Blockchain Labs Global Engagement team for their expert guidance and support throughout the development of this tutorial.

## Additional Documentation

* [Solidity contract with Truffle](./docs/truffle-commands.md)
* [Using remix to get ABI and bytecode](./docs/using-remix.md)
* [Setup Fab Proxy and using Web3](./docs/proxy-web3-commands.md)
* [Loyalty Program Use Case](./docs/use-case.md)

## Links
* [Fabric Chaincode EVM](https://github.com/hyperledger/fabric-chaincode-evm)
* [Hyperledger Fabric Docs](http://hyperledger-fabric.readthedocs.io/en/latest/)
* [Solidity](https://solidity.readthedocs.io/en/v0.4.25/index.html)


## License
[Apache 2.0](LICENSE)















































=====================================================================================================================================

### 1. Setup your machine
- [npm](https://www.npmjs.com/)  (v5.x)
- [Node](https://nodejs.org/en/) (version 8.9 or higher - note version 9 is not supported)
* to install specific Node version you can use [nvm](https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html)

  Example:
  + 1. `nvm install --lts`
  + 2. `nvm use --lts`
  + 3. Output `Now using node v8.11.3 (npm v5.6.0)`
- [Hyperledger Composer](https://hyperledger.github.io/composer/installing/development-tools.html)
  * to install composer cli
    `npm install -g composer-cli@0.19.5`
- [Cloud Wallet Package](https://www.npmjs.com/package/@ampretia/composer-wallet-cloudant)
  * `npm install -g @ampretia/composer-wallet-cloudant@0.2.1`
- [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/install-go-cli.html)
  * for Mac OS X Installation
    ```
    brew tap cloudfoundry/tap
    brew install cf-cli
    ```
  * for Linux (Debian and Ubuntu based) Installation
    ```
    wget -q -O - https://packages.cloudfoundry.org/debian/cli.cloudfoundry.org.key | sudo apt-key add -
    echo "deb https://packages.cloudfoundry.org/debian stable main" | sudo tee /etc/apt/sources.list.d/cloudfoundry-cli.list
    sudo apt-get update
    sudo apt-get install cf-cli
    ```

### 2. Clone the repository

```
git clone https://github.com/IBM-Blockchain/vehicle-manufacture.git
```

### 3. Create the BNA File

```
cd vehicle-manufacture
mkdir contracts/dist
composer archive create -t dir -n contracts/vehicle-manufacture-network -a contracts/dist/vehicle-manufacture-network.bna
```

### 4. Create a Blockchain Service

Create a new Blockchain service in your IBM Cloud space ([link](https://console.bluemix.net/catalog/services/blockchain)). Give your service a  name, select 'Starter Membership Plan' for the pricing and then press **Create**. You should then be taken to the dashboard for your service.

![Create blockchain service screenshot](media/create-blockchain-service.png)

### 5. Configure certificates

In your blockchain service dashboard press the **Launch** button. Use the nav bar on the left to open the 'Channels' page and confirm that your peer is in the default channel. If it is not, add it manually by using the three dots in the actions column.

![Channel page](media/check-channel.png)

Change to the 'Overview' page and press the **Connection Profile** button and in the popup press **Download**. Rename your file to `connection-profile.json` and move it to the `contracts/dist` folder in your cloned copy of this repository.
Open your `connection-profile.json` file and scroll to the bottom. In the `registrar` field there is an `enrollSecret` property. This will be needed in future commands so make a note of it.

Use the enrollSecret you retrieved above to create a certificate authority (CA) card:

```
composer card create -f ca.card -p contracts/dist/connection-profile.json -u admin -s <ENROLL_SECRET>
```

Import your CA card:

```
composer card import -f ca.card -c ca
```

Exchange the enrollSecret for valid certificates from the CA.:

```
composer identity request --card ca --path ./credentials -u admin -s <ENROLL_SECRET>
```

The above command will generate a credentials directory where you run the command containing the certificate files.

Add these certificate files to your starter plan instance by opening the 'Members' page in the blockchain service UI and then pressing the certificates tab . Press **Add Certificate** and enter a name in the popup. Copy the contents of `credentials/admin-pub.pem` to your clipboard and paste in the certificate textbox of the popup.

![Channel page](media/add-admin-cert.png)

Press **Submit** and then **Restart**. Restarting the peer may take several minutes.

Finally sync the certificates to the channel by opening the 'Channels' page and in the default channel press the three dots in the actions column to open the menu. Click **Sync Certificate** and then **Submit** in the popup.

### 6. Install and start the network

> Note: Ensure that your terminal is in the cloned repository folder.

Create a card with channel and peer admin roles:

```
composer card create -f adminCard.card -p ./contracts/dist/connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem --role PeerAdmin --role ChannelAdmin
```

Import the admin card:

```
composer card import -f adminCard.card -c adminCard
```

Install the network:

```
composer network install -c adminCard -a ./contracts/dist/vehicle-manufacture-network.bna
```

Start the network:

```
composer network start -c adminCard -n vehicle-manufacture-network -V 0.2.5 -A admin -C ./credentials/admin-pub.pem -f delete_me.card
```

If this command fails you may need to try again.

>Note: Ensure that the version used in the command above matches that in the output of the install command.

Delete the card created by starting the network:

```
rm delete_me.card
```

Create a new card that references the certificates retrieved earlier:

```
composer card create -n vehicle-manufacture-network -p ./contracts/dist/connection-profile.json -u admin -c ./credentials/admin-pub.pem -k ./credentials/admin-priv.pem
```

Import the new card using:

```
composer card import -f ./admin@vehicle-manufacture-network.card
```

Use ping to check that the network has deployed:

```
composer network ping -c admin@vehicle-manufacture-network
```

![network ping](media/network-ping.png)

Congrats, you've configured your blockchain network!

### 7. Provision Cloudant

Create a new Cloudant service in the same space as your blockchain service ([link](https://console.bluemix.net/catalog/services/cloudantNoSQLDB)). Give your service a name, select 'Lite' for the plan and then press **Create**. you should then be taken to the Cloudant dashboard.

![Create cloudant service screenshot](media/create-cloudant-service.png)

In the Cloudant dashboard use the left navigation bar to go to the 'Service credentials page'. Press **New credential** then press **Add** in the popup leaving the name as Credentials-1.

Create a new file called `cloudant.json` in your vehicle-manufacture directory and paste the following JSON into it:

```
{
    "composer": {
        "wallet": {
            "type": "@ampretia/composer-wallet-cloudant",
            "options": {}
        }
    }
}
```

Get the JSON data of your credentials by clicking **View credentials** on the Credentials-1 row of the 'Service credentials page'. Replace the data in the options field of your `cloudant.json` file with this JSON adding an additional field to the copied JSON with the value "database": "wallet". Your file should look something like this:

```
{
    "composer": {
        "wallet": {
            "type": "@ampretia/composer-wallet-cloudant",
            "options": {
               "host": "XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX-bluemix.cloudant.com",
                "password": "***********",
                "port": 443,
                "url": "https://XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX-bluemix:***********@XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX-bluemix.cloudant.com",
                "username": "XXXXXXXX-XXXX-XXXX-XXXXXXXXXXXX-bluemix",
                "database": "wallet"
            }
        }
    }
}
```

Create the Cloudant database using the value in the JSON for the url field:

```
curl -X PUT <CLOUDANT_URL>/wallet
```

Set the NODE_CONFIG environment variable on your machine using the contents of your `cloudant.json` file with new lines removed:

```
export NODE_CONFIG=$(awk -v RS= '{$1=$1}1' < cloudant.json)
```

>Note: You will need to remove the line breaks before using the file data to replace <CLOUDANT_CREDENTIALS>.

Import the admin card to the Cloudant service:

```
composer card import -f ./admin@vehicle-manufacture-network.card
```

### 8. Deploy the applications

Log in to Cloud Foundry and select the space you deployed your Blockchain service to:
```
cf login
```

>Note: If you use single sign-on you will need to use the `--sso` option with the above command.

#### REST Server

Push the REST server using the docker image:

```
cf push vehicle-manufacture-rest --docker-image ibmblockchain/composer-rest-server:0.19.5 -i 1 -m 256M --no-start --no-manifest --random-route
```

Set the NODE_CONFIG environment variable for the REST server:

```
cf set-env vehicle-manufacture-rest NODE_CONFIG "$NODE_CONFIG"
```

Set the other environment variables for the REST server:

```
cf set-env vehicle-manufacture-rest COMPOSER_CARD admin@vehicle-manufacture-network
cf set-env vehicle-manufacture-rest COMPOSER_NAMESPACES required
cf set-env vehicle-manufacture-rest COMPOSER_WEBSOCKETS true
```

#### Playground

Push playground using the docker image:

```
cf push vehicle-manufacture-playground --docker-image ibmblockchain/composer-playground:0.19.5 -i 1 -m 256M --no-start --random-route --no-manifest
```

Set the NODE_CONFIG environment variable for the playground using the contents of your `cloudant.json` file:

```
cf set-env vehicle-manufacture-playground NODE_CONFIG "$NODE_CONFIG"
```

#### Vehicle manufacture application

Push the application using the files in your clone of this repository:

```
cf push vehicle-manufacture -f ./apps/vehicle-manufacture/manifest.yml -i 1 -m 128M --random-route --no-start
```

Bind the blockchain service to the vehicle manufacture application using the name of your blockchain service:

```
cf bind-service vehicle-manufacture <BLOCKCHAIN_SERVICE_NAME> -c '{"permissions":"read-only"}'
```

Set the environment variable used to tell the vehicle manufacture application where to send requests. To do this you will need to get the REST server's URL. This can be retrieved by in your IBM Cloud dashboard clicking 'vehicle-manufacture-rest' and then using the routes dropdown at the top right of the page. The URL is then a combination of the two textboxes in the popup (e.g. vehicle-manufacture-rest-undichotomous-fresser.mybluemix.net).

![Get route screenshot](media/get-route-screenshot.png)

Use this command to set the environment variable replacing <REST_SERVER_URL> with the URL retrieved above:

```
cf set-env vehicle-manufacture REST_SERVER_URLS '{"vehicle-manufacture-rest": {"httpURL": "https://<REST_SERVER_URL>/api", "webSocketURL": "wss://<REST_SERVER_URL>"}}'
```

Set the environment variable used to tell the vehicle manufacture application where the playground is located. to do this retrieve the playground URL using the method described above but for the playground application and then use the following command replacing <PLAYGROUND_URL> with the URL retrieved:

```
cf set-env vehicle-manufacture PLAYGROUND_URL 'https://<PLAYGROUND_URL>'
```

### 9. Start the applications

Start the REST server:

```
cf start vehicle-manufacture-rest
```

Start the playground:

```
cf start vehicle-manufacture-playground
```

Start the vehicle manufacture application:

```
cf start vehicle-manufacture
```

You can now run the tutorial by clicking vehicle-manufacture in your IBM Cloud dashboard and then clicking **Visit app URL**.

# Potential Errors
## Car Builder - Place Order >  Error Occurred
In the home screen of the car builder press the cog icon. Check that the URLs for the REST server are set to the route of your REST server in IBM Cloud. The HTTP URL should be https://<YOUR_ROUTE>/api and the Web Socket URL wss://<YOUR_ROUTE>.

If you have pressed the **Update** button before, the settings used by the car builder will come from cookies stored in your browser. These settings will overwrite the REST server settings recorded in the environment variables for the vehicle manufacture app. Entering the correct REST URL and pressing **Update** will update this cookie and should fix the error, alternatively to use the environment variable value in the developer console enter `localStorage.clear()`.