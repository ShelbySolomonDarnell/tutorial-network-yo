# tutorial-network-yo

Dev Tutorial network hype fab


### Step Four: Deploying the business network

#### Prefix, do these junks

Stop, teardown, restart and create peer card again.
```
{working-dir}/fabric-tools/stopFabric.sh
{working-dir}/fabric-tools/teardownFabric.sh
{working-dir}/fabric-tools/startFabric.sh
{working-dir}/fabric-tools/createPeerAdminCard.sh
cd tutorial-network-yo
```

#### Run the junks
1. To install the business network, from the `tutorial-network-yo` directory, run the following command: 
> composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network-yo@0.0.7.bna

The `composer network install` command requires a PeerAdmin business network card (in this case one has been created and imported in advance), and the the file path of the `.bna` which defines the business network.
Before trying the next command go to `~/fabric-tools/fabric-scripts/hlfv12/composer/docker-compose.yml` and add the following peer definition `CORE_CHAINCODE_STARTUPTIMEOUT=1200s`.
For your specific installation it didn't work without this modification.


2. To start the business network, run the following command:

> composer network start --networkName tutorial-network-yo --networkVersion 0.0.7 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card

The `composer network start` command requires a business network card, as well as the name of the admin identity for the business network, the name and version of the business network and the name of the file to be created ready to import as a business network card.

3. To import the network adminstrator identity as a usable business network card, run the following command:
> composer card import --file networkadmin.card

The `composer card import` command requires the filename specified in `composer network start` to create a card.

4. To check the business network has been deployed successfully, run the follwing command to ping the network:
> composer network ping --card admin@tutorial-network-yo

The `composer network ping` command requires a business network card to idenitfy the network to ping.

### Step Five: Generating a REST server
------
Hyperledger Composer can generate a bespoke REST API based on a business network. For developing a web application, the REST API provides a useful layer of language-neutral abstraction.
1. To create the REST API, navicate to the `tutorial-network` directory and run the following command:
> composer-rest-server
> composer-rest-server -c admin@tutorial-network-yo -n never -u true -w true
2. Enter `admin@tutorial-network-yo` as the card name.
3. Select **never use namespaces** when asked whether to use namespaces in the generated API.
4. Select **No** when asked whether to secure the generated API.
5. Select **Yes** when asked whether to enable event publication.
6. Select **No** when asked whether to enable TLS security.
The generated API is connected to the deployed blockchain and business network.

### Step Six: Generating an application
------
Hyperledger Composer can also generate an Angular 4 application running against the REST API.
1. To create your Angular 4 applications, navigate to `tutorial-network` directory and run the following command: 
> yo hyperledger-composer:angular
2. Select **Yes** when asked to connect to running a business network.
3. Enter standard `package.json` questions (project name, description, author name, author email, license)
4. Enter `admin@tutorial-network-yo` for the business network card.
5. Select **Connect to an existing REST API**.
6. Enter `http://localhost` for the REST server address.
7. Enter `3000` for server port.
8. Select **Namespaces are not used**
The Angular generator will then create the scaffolding for the project and install all dependencies. To run the application, navigate to your angular project directory and run `npm start`. This will fire up an Angular 4 application running against your REST API at `http://localhost:4200`.
