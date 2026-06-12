## HyperledgerFabrics-Go 
## KBA-Automobile

- This SAMPLE-APP Contains the User Interface Section, the front end that the user sees using HTML and CSS (the manufacturer dashboard).
A simple manufacturer dashboard is created, and add fields and buttons to add a new car details to the network and retrieve any car information from the network.

## Steps to follow

First, create a directory named KBA-CHF to keep all files related to this application.

Add fabric-samples and KBA-Automobile to it; 
copy fabric-samples folder link from: https://drive.google.com/drive/folders/1yaAX7b_qaIn7zDj8IqZmGCTgirAqlc3k?usp=drive_link

KBA-Automobile folder link from :https://drive.google.com/drive/folders/1uUCBKZMzG2rZi_sRHeVk0VBhwdlGsGQS?usp=sharing

## Commands For Executing this application:
## 1.Bring Up the Test Network

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

Check that the containers are running:ou

```bash
docker ps
```

We must see containers for:
- Orderer
- Peer0 Org1
- Peer0 Org2
- CouchDB instances
- Certificate Authorities

## 2.Add Organization 3 (Org3)

Navigate to the `addOrg3` directory and add Org3 to the existing network.

```bash
cd addOrg3

./addOrg3.sh up \
  -c autochannel \
  -ca \
  -s couchdb

cd ..
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `up` | Adds Org3 to the running network |
| `-c autochannel` | Uses the existing channel `autochannel` |
| `-ca` | Uses Certificate Authorities |
| `-s couchdb` | Uses CouchDB as the state database |

---

## 3.Verify Docker Containers

Check that all network components, including Org3, are running.

```bash
docker ps -a
```

Results in following containers for:

- Orderer
- Peer0 Org1
- Peer0 Org2
- Peer0 Org3
- CouchDB instances
- Certificate Authorities
- Chaincode containers (after deployment)

---

## 4.Deploy Chaincode

Deploy the KBA Automobile chaincode to the `autochannel` channel.

```bash
./network.sh deployCC \
  -ccn KBA-Automobile \
  -ccp ../../KBA-Automobile/Chaincode/ \
  -ccl go \
  -c autochannel \
  -cccg ../../KBA-Automobile/Chaincode/collections.json
```

### Parameters

| Parameter | Description |
|-----------|-------------|
| `-ccn KBA-Automobile` | Chaincode name |
| `-ccp ../../KBA-Automobile/Chaincode/` | Path to the chaincode source code |
| `-ccl go` | Chaincode language (Go) |
| `-c autochannel` | Channel on which the chaincode is deployed |
| `-cccg collections.json` | Private data collection configuration file |

### Verify Chaincode Deployment

```bash
docker ps -a
```

Look for a chaincode container similar to:

```text
dev-peer0.org1.example.com-KBA-Automobile_1.0-<hash>
```
we can verify that the chaincode has been committed:

```bash
peer lifecycle chaincode querycommitted \
  -C autochannel \
  -n KBA-Automobile
```

 ## 5.Setup the Sample Application

Add the **SAMPLE-APP** folder inside the **KBA-Automobile** project directory.

### Project Structure

```text
KBA-Automobile/
├── Chaincode/
├── Client/
├── Events/
└── SAMPLE-APP/
```

### Copy Client Files

Copy the following files from the **Client** module into the **SAMPLE-APP** folder:

- `client.go`
- `profile.go`
- `connect.go`

Also add the application's `main.go` file to the **SAMPLE-APP** folder.

### Initialize the Go Module

Navigate to the SAMPLE-APP directory and initialize the Go project.

```bash
cd SAMPLE-APP

go mod init sampleapp
```

### Install Required Dependencies

Download and manage all required packages.

```bash
go mod tidy
```

This command will:

- Download required dependencies
- Add missing modules to `go.mod`
- Remove unused dependencies

### Run the Application

Execute the application using:

```bash
go run .
```

### Expected Outcome

If the network is running and the chaincode is deployed successfully, the application should connect to the Hyperledger Fabric network and perform the operations implemented in `main.go`.

## Bring Down the Network

After completing testing or development activities, stop and remove all network components using the following command:

```bash
./network.sh down
```

### What This Command Does

- Stops all Hyperledger Fabric containers
- Removes peer and orderer containers
- Removes CouchDB containers
- Removes Certificate Authority (CA) containers
- Removes chaincode containers
- Cleans up the network artifacts created by the test network

### Verify Cleanup

Check that the containers have been stopped and removed:

```bash
docker ps -a
```

 There should no longer see the Fabric network containers running.

### Note

For completely generated artifacts and Docker volumes, refer to the Hyperledger Fabric test network cleanup options before restarting the network.
