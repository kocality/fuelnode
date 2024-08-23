# Guide to Run Fuel Testnet Node

![image](https://github.com/user-attachments/assets/1c677355-1bd4-4cdf-9ef9-554635835259)


## About Fuel
Fuel is an operating system specifically designed for Ethereum rollups, helping developers build scalable decentralized economies.
* [Twitter](https://twitter.com/fuel_network)
* [Website](https://fuel.network/)
* [Discord](https://discord.com/invite/xfpK4Pe)
* [Docs](https://docs.fuel.network/)
* [Github](https://github.com/FuelLabs)
* [Explorer](https://app.fuel.network/)
* [Forum](https://forum.fuel.network/)

## About Fuel Node

In the Fuel blockchain, a node—commonly known as a client—is essential software that downloads, stores, and continuously updates a local copy of the blockchain. This software plays a key role in verifying the authenticity of each block and transaction, ensuring your node stays fully synchronized with the network. Fuel's nodes operate on a Proof of Authority (PoA) consensus mechanism, where trusted validators are tasked with block creation and transaction validation. This method enables the network to deliver faster transaction times and greater energy efficiency compared to other consensus models like Proof of Work or Proof of Stake.

### Why Run a Node on Fuel?

Running your own Fuel node comes with significant benefits:

- **Full Control:** By hosting your own node, you maintain complete control over your interactions with the Fuel blockchain, avoiding rate limits and third-party dependencies.
- **Network Independence:** Running your own node ensures that you're not reliant on external services, granting you the freedom to execute queries and interact directly with the blockchain without restrictions.

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

#### You can check Fuelup:
```bash
fuelup --version
```

### Generate the P2P Key
```bash
fuel-core-keygen new --key-type peering
```
