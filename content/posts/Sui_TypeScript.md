+++
title = "How to Use the Sui TypeScript SDK"
date = "2024-10-26"
author = "Ukeje Goodness"
description = "Learn how to read the state of the blockchain, write transactions to the chain from your TypeScript programs"

[taxonomies]
tags = ["Sui", "Blockchain", "Web3", "TypeScript"]
+++

---

Sui is the chain of the moment, and although Move is the holy grail for writing smart contracts on Sui, the TypeScript support is significant. You can interact with and use Sui and most of the DeFi apps on the ecosystem with TypeScript.

In this tutorial, I’ll teach you how to interact with the Sui network via TypeScript. You’ll learn how to read the state of the blockchain, write transactions to the chain from your TypeScript programs.

## Getting Started With Sui and TypeScript

The only prerequisite is that you’ll need basic JS/TS knowledge to run this tutorial smoothly. I’ll walk you through everything else.

First, create a new TypeScript project in your terminal and initialize a new Node.js project.

```bash
mkdir SuiTS
cd SuiTS
npm init -y
```

Install TypeScript as a development dependency if you don’t have it already.

```bash
npm install typescript --save-dev
npm install ts-node //runs TS without the need for transpiling
```

Now, you can initialize a new TypeScript project. This command will create a `tsconfig.json` file with default options that you can customize for your project.

```bash
npx tsc --init
```

Open the `tsconfig.json` and paste these configurations.

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "CommonJS",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "types": ["node"],
    "resolveJsonModule": true
  },
  "exclude": ["node_modules"],

  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

Create a `src` directory where you’ll add your TypeScript files.

```bash
mkdir src
touch src/index.ts
```

Finally, install the Sui TypeScript SDK with this command.

```bash
npm i @mysten/sui.js
```

You’re all set. You can start writing TypeScript programs that interact with the Sui blockchain.

## Connecting to the Sui Blockchain

You must connect to a Sui blockchain to interact with the chain.

First, import `getFullnodeUrl` and `SuiClient` from the SDK client module.

```bash
import { getFullnodeUrl, SuiClient } from '@mysten/sui/client';
```

Now, depending on your desired connection, you can use `getFullnodeUrl` to retrieve the full node URL of the Sui testnet, mainnet, localnet, or devnet; then, use the `SuiClient` to connect to the client instance.

```tsx
import { getFullnodeUrl, SuiClient } from '@mysten/sui/client';

const rpcUrl = getFullnodeUrl('mainnet');

const client = new SuiClient({ url: rpcUrl });
```

To test your connection, You can use the `getLatestSuiSystemState` to retrieve the latest state of the network.

```tsx
// index.ts
import { getFullnodeUrl, SuiClient } from '@mysten/sui/client';

const rpcUrl = getFullnodeUrl("mainnet");

const client = new SuiClient({ url: rpcUrl });

async function getNetworkStatus() {
    const currentEpoch = await client.getLatestSuiSystemState();
    console.log(currentEpoch)
}

getNetworkStatus();
```

Now, transpile the TypeScript code to JavaScript and run it with this command:



```bash
ts-node index.ts
```

You should expect an output similar to this when you execute the command.




![typescript-setup-output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/b6hdtnnms7stcmxwzs9b.png)




## Creating a Sui Wallet

Creating a wallet is another popular operation that might be handy if you build on the Sui Network.

Here’s how to generate Sui wallet keypairs and retrieve the private and public keys from the Keypair.

```tsx
import { Ed25519Keypair } from '@mysten/sui/keypairs/ed25519';
import { getFullnodeUrl, SuiClient } from '@mysten/sui/client';

const rpcUrl = getFullnodeUrl("mainnet");

const client = new SuiClient({ url: rpcUrl });

// random Keypair
const keypair = new Ed25519Keypair();

const publicKey = keypair.getPublicKey();
const privatekey = keypair.getSecretKey();

console.log(privatekey.toString());
console.log(publicKey.toSuiAddress());
```

The `Ed25519Keypair` function creates a new key pair. The `getPublicKey` and `getPrivateKey` methods give you access to the public and private keys, respectively.

Here’s the string output of the private and public keys I generated with the program:

```
suiprivkey1qq9r6rkysny207t5vr7m5025swh7w0wzra9p0553paprhn8zshqsx2rz64r
New Sui Address: 0xbd46d7582ced464ef369114252704b10317436ef70f196a33fcf2c724991fcba
```

I’m funding this wallet with 0.25 Sui for the next set of operations. Feel free to verify and scan the wallet. Do not send any funds; it’s just a dummy wallet.

## Reading Sui Wallet Balances

You can use the `getCoins` function on your client instance to retrieve details on the coins in a wallet address.

```tsx
import { getFullnodeUrl, SuiClient } from '@mysten/sui/client';

// use getFullnodeUrl to define the Devnet RPC location
const rpcUrl = getFullnodeUrl('mainnet');

// create a client connected to devnet
const client = new SuiClient({ url: rpcUrl });

async function getOwnedCoins() {
    const coins = await client.getCoins({
        owner: '0xbd46d7582ced464ef369114252704b10317436ef70f196a33fcf2c724991fcba',
    });
    console.log(coins);
}

getOwnedCoins();
```

The function returns details on the Sui coin alone and the details. The output is in MIST, the Sui gas token. 1 SUI equals 1 billion MIST.


![wallet-balance-details](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gxxlj9pyd3akc93zpo78.png)



The getAllCoins function can be used in the same way to get a list of all the coins in a wallet.

```tsx
async function getAllCoins() {
    // Get the list of owned coins (tokens) for the given owner address
    const ownedCoins = await client.getAllCoins({ owner: "0xbd46d7582ced464ef369114252704b10317436ef70f196a33fcf2c724991fcba" });

    // Access the coin data
    const coins = ownedCoins.data;

    // Iterate through the coins and print their details
    for (const coin of coins) {
        console.log(`Coin Type: ${coin.coinType}`);
        console.log(`Coin Object ID: ${coin.coinObjectId}`);
        console.log(`Balance: ${coin.balance}`);
        console.log('--------------------');
    }

    // If there is more data, handle pagination
    if (ownedCoins.hasNextPage) {
        console.log('More data available. Fetching next page...');
        // You can handle the next page using ownedCoins.nextCursor if needed
    }
}

getAllCoins();
```

For this example, I traded some Sui for $FUD on Hop Aggregator, and here’s the output after running the program.


![listing-all-balances](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/amc6xxd0fnhgtv5qd556.png)



## Send Coins or Objects

Finally, the interesting part is that you’ll learn to send transactions on the blockchain.

Let’s send some $FUD tokens to another wallet. This works for any coins on the Sui Network.

```tsx
import {getFullnodeUrl, SuiClient} from '@mysten/sui/client';
import {Ed25519Keypair} from '@mysten/sui/keypairs/ed25519';
import {Transaction} from '@mysten/sui/transactions';

// Set the RPC URL to connect to the Sui mainnet
const rpcUrl = getFullnodeUrl("mainnet");

const client = new SuiClient({url: rpcUrl});

// Create the keypair using the private key
const keypair = Ed25519Keypair.fromSecretKey("suiprivkey1qq9r6rkysny207t5vr7m5025swh7w0wzra9p0553paprhn8zshqsx2rz64r");

// FUD coin type
const FUD_TYPE = '0x76cb819b01abed502bee8a702b4c2d547532c12f25001c9dea795a5e631c26f1::fud::FUD';

async function sendFUD() {
    const tx = new Transaction();

    // Fetch FUD coins owned by the sender
    const coins = await client.getCoins({owner: keypair.getPublicKey().toSuiAddress(), coinType: FUD_TYPE});

    if (coins.data.length === 0) {
        console.log("No FUD coins found in the wallet.");
        return;
    }

    // Choose the first available FUD coin and split it for the transfer (adjust amount if needed)
    const [coin] = tx.splitCoins(coins.data[0].coinObjectId, [100000000]); 

    tx.transferObjects([coin], '0xb0042cf2c5a16d0a240fc1391d570cd5fe06548f860583f1878c327db70f2a22');

    const result = await client.signAndExecuteTransaction({signer: keypair, transaction: tx});
    await client.waitForTransaction({digest: result.digest});

    console.log("Transaction successful. Digest:", result.digest);
}

sendFUD().then(console.log).catch(console.error);
```

First, I checked that the wallet had some $FUD and split it for the transfer. The `tx.transferObjects` transfer the split coin to the specified address.

Finally, you need to sign the transaction with the `client.signAndExecuteTransaction`, and you can wait for the transaction with `waitForTransaction` to confirm the transaction went through

## Conclusion

You’ve learned to interact with the Sui blockchain using the official TypeScript SDK. There’s so much you can build on Sui with your newly acquired knowledge, such as building wallets and bots.

You can take this further by learning how to interact with Move contracts on Sui to build more sophisticated dApps 
