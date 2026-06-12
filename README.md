## HyperledgerFabrics-Go
### KBA-Automobile

- This SAMPLE-APP Contains the User Interface Section, the front end that the user sees using HTML and CSS (the manufacturer dashboard).
A simple manufacturer dashboard is created, and add fields and buttons to add a new car details to the network and retrieve any car information from the network.

### Steps to follow

First, create a directory named KBA-CHF to keep all files related to this application.

Add fabric-samples and KBA-Automobile to it; 
copy fabric-samples folder link from: https://drive.google.com/drive/folders/1yaAX7b_qaIn7zDj8IqZmGCTgirAqlc3k?usp=drive_link

KBA-Automobile folder link from :https://drive.google.com/drive/folders/1uUCBKZMzG2rZi_sRHeVk0VBhwdlGsGQS?usp=sharing

### Commands For Executing this application
## Bring Up the Test Network

Navigate to the Hyperledger Fabric test network directory and start the network with:

- A channel named **autochannel**
- Certificate Authorities (CA) enabled
- CouchDB as the state database

```bash
cd fabric-samples/test-network

./network.sh up createChannel \
  -c autochannel \
  -ca \
  -s couchdb
```

### Parameters

| Parameter | Description |
|------------|-------------|
| `up` | Starts the Fabric network |
| `createChannel` | Creates and joins a channel |
| `-c autochannel` | Creates a channel named `autochannel` |
| `-ca` | Starts Fabric Certificate Authorities |
| `-s couchdb` | Uses CouchDB as the state database instead of LevelDB |

### Verify the Network

Check that the containers are running:

```bash
docker ps
```

You should see containers for:
- Orderer
- Peer0 Org1
- Peer0 Org2
- CouchDB instances
- Certificate Authorities






