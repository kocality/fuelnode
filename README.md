# Guide to Run Fuel Testnet Node

![image](https://github.com/user-attachments/assets/e44ea8a4-4bea-488f-a0e6-388e170f4ae2)

## About Fuel
Fuel is an operating system specifically designed for Ethereum rollups, helping developers build scalable decentralized economies.
* [Twitter / X](https://twitter.com/fuel_network)
* [Website](https://fuel.network/)
* [Discord](https://discord.com/invite/xfpK4Pe)
* [Docs](https://docs.fuel.network/)
* [GitHub](https://github.com/FuelLabs)
* [Explorer](https://app.fuel.network/)
* [Forum](https://forum.fuel.network/)
* [Fuel Türkiye](https://x.com/fuelnetworktr)

## About Fuel Node

In the Fuel blockchain, a node—commonly known as a client—is essential software that downloads, stores, and continuously updates a local copy of the blockchain. This software plays a key role in verifying the authenticity of each block and transaction, ensuring your node stays fully synchronized with the network. Fuel's nodes operate on a Proof of Authority (PoA) consensus mechanism, where trusted validators are tasked with block creation and transaction validation. This method enables the network to deliver faster transaction times and greater energy efficiency compared to other consensus models like Proof of Work or Proof of Stake.

### Why Run a Node on Fuel?

Running your own Fuel node comes with significant benefits:

- **Full Control:** By hosting your own node, you maintain complete control over your interactions with the Fuel blockchain, avoiding rate limits and third-party dependencies.
- **Network Independence:** Running your own node ensures that you're not reliant on external services, granting you the freedom to execute queries and interact directly with the blockchain without restrictions.

## System Requirements

| Hardware   | Minimum | Recommended |
|------------|---------|-------------|
| Processor  | 2 Cores | 8 Cores     |
| Memory     | 4 GB    | 12 GB       |
| Storage    | 30 GB   | 100 GB      |


## Step 1: System Updates and Installation of Required Tools

### Update System Packages
```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install wget curl
```

### Rust and Cargo Installation
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

source $HOME/.cargo/env
```

#### Check Rust version:
```bash
rustup --version
```

## Step 2: Fuel Toolchain

### Download and Install Fuel 
```bash
curl https://install.fuel.network | sh
source /root/.bashrc
```
<img width="349" alt="Ekran Resmi 2024-08-23 14 02 04" src="https://github.com/user-attachments/assets/24ecc340-5378-439a-a75e-5752da1ee6d9">

#### Check the fuelup to make sure it is installed correctly:
```bash
fuelup --version
```
<img width="349" alt="Ekran Resmi 2024-08-23 14 03 28" src="https://github.com/user-attachments/assets/c6519335-bdb3-42f5-8c54-a92121966566">

### Generating a P2P Key Pair
#### Note: After entering the command, it will give you important information. Remember to write it down and do not share it with anyone.
```bash
fuel-core-keygen new --key-type peering
```
<img width="1258" alt="Ekran Resmi 2024-08-23 14 05 12" src="https://github.com/user-attachments/assets/d5bcb155-95a5-4144-a9ba-a78e0abefce6">


### Cloning the Chain Configuration and Copying Files
```bash
git clone https://github.com/FuelLabs/chain-configuration chain-configuration
mkdir ~/.fuel-testnet
cp -r chain-configuration/ignition/* ~/.fuel-testnet/
```
#### To obtain an RPC endpoint for `Sepolia Ethereum`, visit [Alchemy](https://www.alchemy.com/) and navigate to Apps > Endpoints > Networks. Under the Ethereum section, select `Sepolia` to obtain your RPC URL. We will use this URL as the `Sepolia-ETH-RPC-ENDPOINT` in the node configuration.

<img width="1201" alt="Ekran Resmi 2024-08-23 14 17 43" src="https://github.com/user-attachments/assets/c1fcaf63-417d-444c-b1f5-be01d2c6ee2c">

## Step 3: Running Node

### Create Screen
```bash
screen -S fuel-node
```

### Run Node
#### Note: Change the `NODENAME`, `P2P-SECRET`, `Sepolia-ETH-RPC-ENDPOINT` values to your own.
```bash
fuel-core run \
--service-name=`NODENAME` \
--keypair `P2P-SECRET` \
--relayer `Sepolia-ETH-RPC-ENDPOINT` \
--ip=0.0.0.0 --port=4000 --peering-port=30333 \
--db-path ~/.fuel-testnet \
--snapshot ~/.fuel-testnet \
--utxo-validation --poa-instant false --enable-p2p \
--reserved-nodes /dns4/p2p-testnet.fuel.network/tcp/30333/p2p/16Uiu2HAmDxoChB7AheKNvCVpD4PHJwuDGn8rifMBEHmEynGHvHrf \
--sync-header-batch-size 100 \
--enable-relayer \
--relayer-v2-listening-contracts=0x01855B78C1f8868DE70e84507ec735983bf262dA \
--relayer-da-deploy-height=5827607 \
--relayer-log-page-size=500 \
--sync-block-stream-buffer-size 30
```

<img width="900" alt="Ekran Resmi 2024-08-23 14 24 53" src="https://github.com/user-attachments/assets/dc9e6317-e6c4-49e0-b7af-0fa992f87206">

#### The logs should look like this. You can exit the screen with `CTRL A+D`. When you exit in this way, your node will continue to run on the screen. 


### Stopping Node

To stop the node, use the following command to reattach to the screen session:

```bash
screen -r fuel-node
```
#### Once inside the screen session, press `CTRL + C` to stop the node.
