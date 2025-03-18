# Decentralized Identity Management System

This project implements a **Decentralized Identity Management System** using **Hyperledger Fabric**. It provides a secure and tamper-proof way to manage identities, such as Aadhaar-based digital identities.

## Prerequisites

Ensure the following dependencies are installed on your system before proceeding:

### **Install Required Packages** (Linux)

```bash
sudo apt-get update
sudo apt-get install -y curl git
```

### **Install Docker**

```bash
sudo apt-get -y install docker-compose
```

Verify installation:

```bash
docker --version
docker-compose --version
```

### **Clone the Repository**

```bash
git clone https://github.com/MahimYadav2006/Decentralised-Identity-Management-System.git
cd Decentralised-Identity-Management-System
```

## **Setup Hyperledger Fabric**

### **Navigate to the Chaincode Directory**

```bash
cd fabric-samples/asset-transfer-ledger-queries/chaincode-javascript/
```

### **Install Dependencies**

```bash
npm install
```

### **Navigate to the Test Network Directory**

```bash
cd fabric-samples/test-network
```

### **Restart and Setup the Network**

```bash
./network.sh down  # Shut down any existing network
./network.sh up createChannel -ca  # Start a new channel
./network.sh deployCC -ccn identityContract -ccp ../asset-transfer-ledger-queries/chaincode-javascript -ccl javascript
```

## **Set Environment Variables**

### **Update CLI Path for Binaries**

```bash
export PATH=${PWD}/../bin:$PATH
```

### **Set Fabric Configuration Path**

```bash
export FABRIC_CFG_PATH=$PWD/../config/
```

### **Set Peer CLI as Org1 Admin**

```bash
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```

### **Check Variables**

```bash
echo $ORDERER_CA
echo $PEER0_ORG1_CA
```

If not set, configure them manually:

```bash
export ORDERER_CA=${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
export PEER0_ORG1_CA=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
```

## **Query the Ledger**

### **Read an Identity Record**

```bash
peer chaincode query -C mychannel -n identityContract -c '{"Args":["GetIdentity", "Aadhar-1003"]}'
```

### **Create a New Identity**

```bash
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com \
--tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
-C mychannel -n identityContract \
--peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt \
--peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
-c '{"Args":["IssueIdentity", "Aadhar-1003", "Mahim Yadav", "2002-07-15", "IIT Jammu, India", "9876543210"]}'
```

### **Update an Existing Identity**

```bash
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com \
--tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem \
-C mychannel -n identityContract \
--peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt \
--peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt \
-c '{"Args":["UpdateIdentity", "Aadhar-1003", "Mahim Yadav", "2002-07-15", "New Address, India", "9998887776"]}'
```

## **Contributors**

- **Mahim Yadav**

