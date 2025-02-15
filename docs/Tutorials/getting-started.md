---
title: Getting started
---

# Getting started

By creating a free account on Starton, you can deploy and interact with your Smart contract and watch your transactions.

## Onboarding on Starton

### Creating an free account

1. Go to [starton.com](https://app.starton.com/register).
1. Enter your information.
1. Click **Create an account**.

### Setting up your API key

You will be authenticated by Starton's API. For today, let's use a test API key.

1. From the Dashboard, click Developer.
1. Click + API key.
1. Select Test API key.
1. Enter your key information.
1. Click Next.
1. Select the scope of your API key. For today we will use:
    1. In Deploy, check Create and Edit.
    1. For Interact, use Create and Edit.
    1. For Wallet, use Depot, Sign, and Edit.

To use Starton from your application, you can instead:

1. Go to the Developer section.
1. Copy your API key.
1. Add your API key in the header of your requests, as such:

```jsx
const http = axios.create({
	baseURL: "https://api.starton.com/v3",
	headers: {
		"x-api-key": "YOUR_API_KEY", //API KEY COPIED FROM STARTON
	},
})
```

> Note that your API key should be SECRET. Everyone with it could call the Starton API by pretending to be you.

For more information on API Keys, read [Using Starton API](/Developer/API.md).

### Setting up a wallet on Starton

When deploying and interacting with **Smart Contracts** on testnet, you can create a wallet on Starton KMS to sign transactions.

1. From **Dashboard**, go to **Wallet**.
1. Click **Add a wallet**.
1. Enter a **Name** and a **Description** for your wallet.

You can now use your wallet.

You can also import your own KMS at any time and change ownership of your Smart contract. Read more in [Tutorials](https://docs.starton.com/tutorials/how-to-change-the-smart-contracts-ownership)

## Deploying your first Smart contract

1. From **Dashboard**, go to **Smart Contract**.
1. Click **Deploy with Template**.
1. Select a use case and a template.
1. Enter the Smart contract information:

-   General information
    |Parameter|Description|
    |-----|----------|
    | **Name**|The name displayed on Starton.|

-   Smart Contract Constructor
    |Parameter|Description|
    |-----|----------|
    | **Name**|Name of the Smart contract on the blockchain. |
    | **Symbol**|The symbol of your token |
    | **Base URI**|Will be used to concatenate NFT URIs and the ContractUriSuffix <br/> - Using IPFS: `ipfs://ipfs/` <br/> - Using a centralised server: it should be as such `https://yourapi.com/endpoint/` |
    | **Contract URI Suffix** |Will be concatenated with baseUri:<br/> - Using IPFS:it is the CID of the contract metadata .<br/> - Using a centralised server: the path of the contract metadata json|

1. Click **Next**.

## Interacting with your first Smart contract

1. In **Smart Contract**, select a **Smart Contract**.
1. Select a **function**.
1. Enter the **parameters**.
1. Click **Run**.

## Checking the activity from your Smart contract

To check that everything went your way, you can see the activity of your Smart contract.

1. In **Smart Contract**, select a **Smart Contract**.
1. Click on **Interact**.

You can see all the interactions with your smart contracts. The first one represents its creation on the blockchain.

**Related topics**

-   More on [Smart Contracts](/Smart-contract/understanding-smart-contracts.md)
-   More on [Relayer](/Transactions/understanding-the-relayer.md)
-   More on [Key Management Systems](/Wallet/understanding-key-management-systems.md)
