# Avalanche HyperSDK: Building a Powerfull Blockchain Network 

The objective of this project is to leverage the HyperSDK to create a custom virtual machine, enabling the development of a blockchain that meets specific requirements. This custom blockchain will support functionalities such as minting and transferring tokens, as well as managing order books for asset trading. By utilizing the HyperSDK, the project aims to provide complete control over the blockchain’s rules and functionality, ensuring a tailored solution that aligns with the unique needs of the startup.

##### 🚀 This project is introduced by MetaCrafters and utilizes the Avalanche framework.

### Avalanche HyperSDK Introduction [Avalanche Documentation](https://www.avax.network/blog/introducing-hypersdk-a-foundation-for-the-fastest-blockchains-of-the-future)

The Avalanche HyperSDK is a framework designed to allow developers to build custom virtual machines (VMs) on the Avalanche network. It provides a flexible and powerful toolset for creating and deploying blockchains that are tailored to specific use cases. With HyperSDK, developers can define the rules, functionality, and features of their blockchain, such as custom token standards, consensus mechanisms, and transaction logic. This level of customization enables businesses and developers to create blockchain solutions that precisely meet their requirements, whether for decentralized finance (DeFi), asset trading, or any other blockchain-based application.

### Initial Process

1. **Launch Your TokenVM Subnet:**  
   To get started, launch your own TokenVM Subnet by running the following command from the specified directory. This process may take a few minutes:
   ```
   ./scripts/run.sh
   ```
   By default, this command allocates all network funds to the address `token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp`. The corresponding private key is `0x323b1d8f4eed5f0da9da93071b034f2dce9d2d22692c172f3cb252a64ddfafd01b057de320297c29ad0c1f589ea216869cf1938d88c9fbd70d6748323dbf2fa7`, and for convenience, it is also stored in `demo.pk`.

   *Note:* If you only need one Subnet for testing, you can run the following command instead:
   ```
   MODE="run-single" ./scripts/run.sh
   ```

2. **Build the Token-CLI:**  
   To facilitate interaction with the TokenVM, you’ll need to build the `token-cli`. Run the following command from the specified directory:
   ```
   ./scripts/build.sh
   ```
   The compiled CLI will be located in `./build/token-cli`.

3. **Add Chains and Import Default Key:**  
   Finally, add the chains you created and import the default key into the `token-cli` by running these commands:
   ```
   ./build/token-cli key import demo.pk
   ./build/token-cli chain import-anr
   ```
   The `chain import-anr` command connects to the Avalanche Network Runner server running in the background and retrieves the URIs of all nodes tracking each chain you created.

### Tokenevm (Custom HyperChain) Introduction 

The TokenVM is a specialized virtual machine created to demonstrate the capabilities of the HyperSDK within the context of token minting and trading, a familiar application for most users. With TokenVM, users can create assets, mint additional tokens, modify asset metadata, and burn tokens as needed. Additionally, it features an embedded on-chain exchange that allows users to create and fill orders, including partial fills, with other participants.

The TokenVM includes a powerful command-line interface (CLI) and serves RPC requests using an in-memory order book, which it maintains by syncing blocks. This allows for efficient and user-friendly interaction with the blockchain.

#### Key Features:

**Arbitrary Token Minting:** Easily create, mint, transfer, and manage user-generated tokens. The storage engine is optimized for efficient use, enabling parallel execution of transfers.

**On-Chain Trading:** Supports fully on-chain trading with custom assets. Users can create and fill orders, leveraging an optimized storage format for efficient order management and execution.

**In-Memory Order Book:** Facilitates easy interaction with the TokenVM through an in-memory order book that listens for and manages orders submitted on-chain.

**Security and Resilience:** Includes features like sandwich-resistant order execution, partial fill refunds, and expiring fills to ensure secure and efficient trading.

**Avalanche Warp Support:** Enables seamless asset transfers between TokenVMs without relying on external relayers or bridges, maintaining security and transparency. Easily create, mint, transfer, and manage user-generated tokens. The storage engine is optimized for efficient use, enabling parallel execution of transfers.

**On-Chain Trading:** Supports fully on-chain trading with custom assets. Users can create and fill orders, leveraging an optimized storage format for efficient order management and execution.

**In-Memory Order Book:** Facilitates easy interaction with the TokenVM through an in-memory order book that listens for and manages orders submitted on-chain.

**Mint and Trade: A Step-by-Step Guide**

```bash
# Step 1: Create Your Asset
./build/token-cli action create-asset

# After execution, you should see an output similar to this:
# - database: The directory where token data is stored.
# - address: The address linked to the default private key.
# - chainID: The ID of the blockchain network.
# - metadata: Asset metadata (modifiable later).
# - txID: The transaction ID, which also serves as your asset's ID.
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
metadata (can be changed later): MarioCoin
continue (y/n): y
✅ txID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
```

---

```bash
# Step 2: Mint Your Asset
./build/token-cli action mint-asset

# The output should look like this:
# - assetID: The ID of the asset you're minting.
# - recipient: The address receiving the newly minted tokens.
# - amount: Number of tokens to be minted.
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
assetID: 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 0
recipient: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
amount: 10000
continue (y/n): y
✅ txID: X1E5CVFgFFgniFyWcj5wweGg66TyzjK2bMWWTzFwJcwFYkF72
```

```bash
# Step 4: Create an Order
./build/token-cli action create-order

# The output will show details like:
# - in assetID: The asset being exchanged (TKN is the native token).
# - in tick: The amount of "in assetID" needed to get "out tick" of the "out assetID".
# - out assetID: The asset being received in exchange.
# - out tick: The amount of "out assetID" to be received per "in tick".
# - supply: The amount of "out assetID" available for trade.
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
in assetID (use TKN for native token): TKN
✔ in tick: 1█
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 10000
warp: false
balance: 10000
out tick: 10
supply (must be multiple of out tick): 100
continue (y/n): y
✅ txID: 2TdeT2ZsQtJhbWJuhLZ3eexuCY4UP6W7q5ZiAHMYtVfSSp1ids

# The txID here serves as the orderID for the new order.
```

---

```bash
# Step 5: Fill Part of the Order
./build/token-cli action fill-order

# Output details:
# - available orders: List of active orders available for filling.
# - Rate(in/out): Exchange rate between in asset and out asset.
# - Remaining: Quantity of out asset remaining in the order.
# - value: Quantity of in asset you want to exchange.
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
in assetID (use TKN for native token): TKN
balance: 997.999993843 TKN
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
metadata: MarioCoin
supply: 10000
warp: false
available orders: 1
0) Rate(in/out): 100000000.0000 InTick: 1.000000000 TKN OutTick: 10 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug Remaining: 100 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
select order: 0
value (must be multiple of in tick): 2
in: 2.000000000 TKN out: 20 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
continue (y/n): y
✅ txID: uw9YrZcs4QQTEBSR3guVnzQTFyKKm5QFGVTvuGyntSTrx3aGm

# This command executes a trade based on the selected order, exchanging TKN for the specified out asset.
```

---

```bash
# Step 6: Close Order
./build/token-cli action close-order

# Output details:
# - orderID: The ID of the order you want to close.
# - txID: Transaction ID confirming the order has been closed.
database: .token-cli
address: token1rvzhmceq997zntgvravfagsks6w0ryud3rylh4cdvayry0dl97nsjzf3yp
chainID: Em2pZtHr7rDCzii43an2bBi1M2mTFyLN33QP1Xfjy7BcWtaH9
orderID: 2TdeT2ZsQtJhbWJuhLZ3eexuCY4UP6W7q5ZiAHMYtVfSSp1ids
out assetID (use TKN for native token): 27grFs9vE2YP9kwLM5hQJGLDvqEY9ii71zzdoRHNGC4Appavug
continue (y/n): y
✅ txID: poGnxYiLZAruurNjugTPfN1JjwSZzGZdZnBEezp5HB98PhKcn

# Closing the order will return any unspent assets locked in the order back to your account.
```

<img src="https://github.com/Metacrafters/tokenvm/raw/main/assets/hypersdk.png">


