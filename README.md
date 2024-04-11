# Pryzm Node Deployment
This repository contains Ansible scripts for the installation, updating, and removal of a Pryzm node on Linux systems. The playbooks are designed to simplify the process of setting up a Pryzm node, managing its services, and streamlining node operations.

## Prerequisites
A Linux system (Ubuntu 22.04 LTS recommended) with root access and docker installed.
Git and Ansible version 2.15 or newer installed on your machine.

## Getting Started

### Step 1: Installing Dependencies
Update your system's package list:

```
sudo apt update && sudo apt upgrade -y
```

Install Ansible and Git:
```
sudo apt install ansible git -y
```

### Step 2: Downloading the Project

Clone this repository to access the Ansible playbook and all necessary files:

```
git clone https://github.com/nodemasterpro/deploy-node-pryzm.git
cd deploy-node-pryzm
```

### Step 3: Installing Pryzm Node
Execute the playbook using the following command, specifying the moniker (node name) as an extra variable:

```
ansible-playbook install_pryzm_feeder.yml -e moniker="your_node_name"
```
Note: The moniker is the name of your node.


### Step 4: Viewing Logs Pryzm node
To view the logs for the Pryzm node:
```
journalctl -u pryzm-node -f -o cat
```

### Step 5: Requesting Testnet Funds
Request Testnet Funds: After installing the Pryzm feeder, request testnet funds by providing the Pryzm feeder wallet address generated at the end of the `install_pryzm_feeder.yml` script execution. Visit https://testnet.pryzm.zone/faucet to request funds. You will need the wallet address provided at the end of the Pryzm 
Check Node Synchronization: To ensure your node is fully synchronized, execute:
```
pryzmd status | jq .SyncInfo
```

### Step 6: Checking Node Synchronization
To check the synchronization status of your node after installing Pryzm Feeder, use the following command:
```
pryzmd status | jq .SyncInfo
```
Wait for 30 minutes to an hour for the node to synchronize. When the catching_up variable is set to false, your node is synchronized.

### Step 7: Creating the Validator and Linking to the Feeder
Once Pryzm Feeder is installed and your node is synchronized, execute the following playbook to create a validator and link it to the feeder:
```
ansible-playbook create_validator_and_link_feeder.yml
```
During this step, you will be prompted to input the following information:

moniker : "node_name"
details : "a sentence of your choice"
website : "your website"
contact info : "how to contact you"
feeder : "the wallet feeder address"

### Step 8: Viewing state validator

You can check your state validator by pasting your valoper adresse https://testnet.chainsco.pe/pryzm.  The valoper address is provided at the end of the execution of the create_validator_and_link_feeder.yml script.

## Additional Note

### Stopping Services
To stop the Pryzm node and Pryzm feeder services: 
```
sudo systemctl stop pryzm-node
sudo systemctl stop pryzm-feeder
```

### Starting Services
To start the Pryzm node and Pryzm feeder services: 
```
sudo systemctl stop pryzm-node
sudo systemctl stop pryzm-feeder
```

### Updating the Pryzm Node
To update the Pryzm node with the latest binary, use the update_pryzm_node.yml playbook. This script will stop the Pryzm node service, download the specified version of the Pryzm binary, replace the old binary, and restart the service with the new version. Run the playbook with the following command:

```
ansible-playbook update_node_pryzm.yml
```

### Removing the Pryzm Node
To remove the Pryzm node and Pryzm feeder, simply run the playbook again with the action set to remove:

```
ansible-playbook remove_node_pryzm.yml
```

Make sure to back up any important data before removing the Pryzm node, as this action may delete node data.