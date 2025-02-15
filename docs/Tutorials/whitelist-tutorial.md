---
title: How to sell NFTS to a list of approved addresses 
description: Learn how to sell NFTS to a list of approved addresses
---
# How to sell NFTS to a list of approved addresses

When creating and selling a collection, you can allow only a few chosen people to buy them for a defined price.
This system is called a whitelist sale and it is what we are going to see today.

:::info YOU WILL NEED: 

- nodejs (version 16 or over)
- npm 
- ethers v5
- merkletreejs
- a deployed ERC721 NFT contract

:::

:::note

To deploy a Sale contract, you must have deployed your NFT by deploying an ERC721 contract first. Learn how to [Deploy your NFT with Starton](/Tutorials/deploy-Nfts-with-Binance.md)

:::

**In this tutorial, we will:**

-   [Create a merkle root from all approved addresses](#creating-a-merkle-root-from-all-approved-addresses)
  - [Deploy the Sale contract from API](#deploying-the-sale-contract-from-api)
-   [Interact with the contract](#interacting-with-the-contract)


To deploy this contract, you need to compute a special parameter called *definitiveMerkleRoot*.

## Creating a merkle root from all approved addresses 

The best way to create a whitelist in solidity is by **generating a merkle tree**. 
It is a data structure that can store many data in a limited space which ends up being a merkle root.

To be able to generate this parameter, you will need to use this code snippet:

```jsx showLineNumbers
import { ethers } from "ethers";
import { MerkleTree } from "merkletreejs";

// Compute the root of the merkletree
const addresses = [
    /* The addresses of the wallets allowed to mint */
];
const leaves = addresses.map((addr) => ethers.keccak256(addr));
const merkleTree = new MerkleTree(leaves, ethers.keccak256, { sortPairs: true });
const rootHash = merkleTree.getRoot();
```
:::caution 

Don’t forget to replace addresses with the actual wallet addresses of the whitelisted accounts.

:::

## Deploying the Sale contract from API 

After you computed the root of the Merkle Tree, you can deploy the contract:

```jsx showLineNumbers
const axios = require("axios")

const axiosInstance = axios.create({
    baseURL: "https://api.starton.com",
    headers: { "x-api-key": "PUT HERE YOUR API KEY" },
})

axiosInstance
    .post("/v3/smart-contract/from-template", {
        network: "",
        signerWallet: "",
        templateId: "ERC721_SALE",
        name: "My first NFT Whitesale",
        description: "This is my first NFT Whitesale ",
        params: [
            "", // definitiveTokenAddress
						"", // definitiveMerkleRoot
						"", // definitivePrice
						"", // definitiveStartTime
						"", // definitiveEndTime
						"", // definitiveMaxTokensPerAddress
						"", // definitiveMaxSupply
						"", // definitiveFeeReceiver
        ],
        speed: "average",
    })
    .then((response) => {
        console.log(response.data)
    })
```

- **definitiveTokenAddress**: The address of the previously deployed ERC721
- **definitiveMerkleRoot**: The merkle root
- **definitivePrice**: The price to sell each token
- **definitiveStartTime**: The time when the sale will start
- **definitiveEndTime**: The time when the sale will end
- **definitiveMaxTokensPerAddress**: The max amount of tokens a single address can buy
- **definitiveMaxSupply**: The total amount of tokens that can be sold
- **definitiveFeeReceiver**: The address that will receive all the price paid to mint

## Granting role on your base contract

For the whitelist sale contract to be able to mint new tokens, you are going to need to grant the newly created contract the MINTER_ROLE over the base contract deployed.

You will need:

- the address of the base ERC721 contract
- the address of the Sale contract
- the data of the Minter Role (in bytes)

### Retrieving the MINTER_ROLE

First, let's get the minter role:
```jsx showLineNumbers
    const axios = require("axios")
    
    const axiosInstance = axios.create({
        baseURL: "https://api.starton.com",
        headers: {
        "x-api-key": "PUT HERE YOUR API KEY",
    },
    })
    
    axiosInstance.post(
    "/v3/smart-contract/{network}/{YOUR_BASE_CONTRACT}/read",
        {
        functionName: "MINTER_ROLE",
        params: []
        }
    ).then((response) => {
    console.log(response.data)
    })
```
### Granting the minter role to your Sale contract

To grant the minter role to your contract, you can use the following request:

```jsx
const axios = require("axios")

const axiosInstance = axios.create({
    baseURL: "https://api.starton.com",
    headers: { "x-api-key": "PUT HERE YOUR API KEY" },
})

axiosInstance.post(
	"/v3/smart-contract/${network}/${YOUR SMART CONTRAT ADDRESS}/call",
	{
		functionName: "grantRole(bytes32,address)",
		params: [
			"0x9f2df0fed2c77648de5860a4cc508cd0818c85b8b8a1ab4ceeef8d981c8956a6", // MINTER_ROLE
			"" // account
		],
		signerWallet: "",
		speed: "average"
	}
).then((response) => {
	console.log(response.data)
})
```

- account: The whitelist sale contract address

## Interacting with the contract

You can access different parameters of the contract using these functions:

- **price**: Get the price to mint one token
- **startTime**: Get the start time of the sell
- **endTime**: Get the end time of the sell
- **maxTokensPerAddress**: Get the max tokens per address
- **leftSupply**: Get the total amount of tokens that can still be minted
- tokensClaimed(address): Get the total amount already minted by an address

And finally you can use the `withdraw` function to withdraw all the fees earned to the `definitiveFeeReceiver`.

- definitiveMaxTokensPerAddress: The max amount of tokens a single address can buy
- definitiveMaxSupply: The total amount of tokens that can be sold
- definitiveFeeReceiver: The address that will receive all the price paid to mint

### Minting a NFT 

Now that the contract is deployed, every user can mint a token.

To be able to do this, every user will need to specify two parameters while minting:

- the address of the receiver of the NFT
- the merkle proof

This proof is also linked to the merkle tree and it can prove that an address is inside the merkle tree To be able to generate it, you can use this code snippet:

```jsx
import { keccak256 } from "ethers/lib/utils";
import { MerkleTree } from "merkletreejs";

// Compute the root of the merkletree
const addresses = [
	/* The addresses of the wallets allowed to mint */
];
const leaves = addresses.map((addr) => keccak256(addr));
merkleTree = new MerkleTree(leaves, keccak256, { sortPairs: true });

// replace MINTER_ADDRESS with the address of the account that will mint the NFT
const proof = merkleTree.getHexProof(keccak256(MINTER_ADDRESS));
```

Users will then be able to mint a new token using their wallet such as metamask with the right parameters.

## Integrating the Sale to your application

Congrats! You're now all set for your NFT Sale. You can connect this smart contract using the mint function to a button. By, using etherjs/web3js, call the mint function of the sale contract with the desired address.

You can also give more information on this mint page such as NFT left to mints or end time of the sale.