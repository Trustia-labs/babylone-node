# Babylon Node Docker Setup

This project contains the setup for running a Babylon Node within a Ubuntu Docker container. It includes the Dockerfile for building the image and the docker-compose.yml for easy setup and management. This project is based on the excellent article from BreizhNode (https://medium.com/@breizh-node/babylon-lancez-votre-n%C5%93ud-validateur-en-quelques-%C3%A9tapes-simples-fr-en-deb8084d2b22). 

## Prerequisites

- Docker
- Docker Compose
- An internet connection for downloading dependencies

## Getting Started

### Step 1: Clone the Repository

Clone this repository to your local machine to get started.

```bash
git clone https://github.com/Trustia-labs/babylone-node.git
cd babylone-node
```
### Step 2: Build and Run the Container
Navigate to the project directory and use Docker Compose to build and start the Babylon Node.

```bash
docker-compose up -d --build
```
This command builds the Docker image if it hasn't been built and starts the container in detached mode.

### Step 3: Verify Container is Running

Check that the container is running successfully.

```bash
docker ps
```
You should see your babylon-node container listed.

# Post-Installation Steps
After the container is up and running, you'll need to perform a few steps inside the container to finalize the setup.

Accessing the Container

Enter the container using the following command:

```bash
docker exec -it babylon-node /bin/bash
```
`
# Creating a Wallet and register your node
## Check Node Synchronization
Before creating your wallet, ensure the node is fully synchronized with the network.
```bash
babylond status | jq '.sync_info.catching_up'
```

Wait until the catching_up value is false, indicating synchronization is complete.

## Create Wallet 
Create a new wallet by running:
```bash
babylond keys add wallet
```

Follow the prompts to secure your wallet.

## Linking Wallet and Configuring Node
Link Wallet
Replace <WALLET> with your wallet's address to link it.

```bash
babylond create-bls-key <WALLET>
```

### Configure config.toml
Adjust the timeout commit to 30s for better performance.

```bash
sed -i 's/^timeout_commit\s*=.*/timeout_commit = "30s"/' ~/.babylond/config/config.toml
```

## Registering the Node
Before registering your node, ensure you have at least 1 $BBN in your wallet.

- Check Wallet Balance : Visit BabylonScan and enter your wallet address to verify your balance.
- Create Validator Profile : Create your validator profile in .babylond/config/validator.json. Replace placeholders with your information.

```json
{
"pubkey": "<PUBLIC_KEY>",
"amount": "1000000ubbn",
"moniker": "<NAME>",
"website": "<WEBSITE>",
"details": "<DESCRIPTION>",
"commission-rate": "0.10",
"commission-max-rate": "0.20",
"commission-max-change-rate": "0.01",
"min-self-delegation": "1"
}
```

## Register Node as Validator
Run the following command, replacing placeholders as necessary:
```bash
babylond tx checkpointing create-validator .babylond/config/validator.json --chain-id="bbn-test-3" --gas="auto" --gas-adjustment="1.5" --gas-prices="0.025ubbn" --from=wallet
```
# Monitoring and Management
- Monitoring: Access logs and monitor the node using Docker commands.
- Management: Perform routine node management tasks within the container environment.
- Support For more information, visit the Babylon official documentation or join the Babylon Discord community.

# Stay Connected and Support Us
We're constantly working on improving Babylon Node Docker Setup and adding new features based on community feedback. Here's how you can stay connected and support our project:

 - Star this Repository: If you find this project useful, please consider starring it by clicking the ⭐ button at the top. It helps more people discover our work!
- Follow  https://github.com/orgs/Trustia-labs : For updates on this project and others, follow @Trustia-labs on GitHub.

Your support and contributions make this project better every day. Thank you for being a part of our community!
