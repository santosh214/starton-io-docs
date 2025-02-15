---
title: Deploy your NFTs on BNB Smart Chain with Starton
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Deploy your NFTs on BNB Smart Chain with Starton

In this tutorial, we will see how we can deploy a smart contract and interact with it to mint NFTs on BNB Chain testnet in 6 steps.
We will use random images uploaded on IPFS (a distributed file storage system) and assigned to an BNB Chain address.

You will:

-   Upload the contract-level metadata
-   Use a template from Starton for our smart contract (ERC721).
-   Deploy it with Starton from the dashboard. To deploy contracts from code, see [Deploying a smart contract from code](/Tutorials/deploy-a-contract-from-code.md).
-   Upload the content of our NFTs and their metadata on IPFS.
-   Interact with our smart contract using Starton API in order to mint the NFTs and give it to a specific address.

If you feel stuck or have questions, feel free to get in touch in our [Discord](https://discord.com/invite/2jUShxHddn).
We’ll be glad to help you!

## Upload the contract level metadata on IPFS

To enter a name on the marketplace’s dashboard and perceive fees when someone sells one of our NFTs, we need to implement the contract-level metadata.

Here are the values we will use:

```jsx showLineNumbers
{
  "name": “My Super NFT on BNB“,
  "description": “You’ve never seen NFTs this beautiful.”,
  "image": "URI of your image",// This will be used as the image of your collection
  "external_link": "",
  "seller_fee_basis_points": 100,
  "fee_recipient": “PUT YOUR ADDRESS HERE”
}
```

We now have the content of our metadata, we need to upload it on IPFS.

To upload our files on IPFS we will now use the [Starton IPFS](/IPFS/understanding-IPFS.md) pinning service.

As the contract-level metadata only needs to be uploaded once, we can directly do it from [our dashboard](https://app.starton.com/ipfs).

<div class="row-is-multiline">

<div class="col col--2" class="cards">
	<a class="button-card button-card--vertical" href="https://app.starton.com/projects">
		<h3>Upload a file on the Dashboard</h3>
		<div class="button-card__inner">
			<p color="white">Go to <b>Starton Dashboard</b> and upload all your files in three clicks.</p>
		</div>
	</a>
</div>

</div>

1. Go to **IPFS**.
1. Click **Upload**.
1. Select **JSON**.
1. Enter JSON content.
1. Enter a name for the file.
1. Click **Upload**.

Once done, a column “CID” appears with a value for our file. We will use this data for the Contract Uri Suffix of our smart contract!

## Choose a smart contract template

Several standards of smart contracts have been developed for NFTs.

The most used ones are ERC721 and the ERC1155.
If you want to see in more details the differences between the two standards, read [The Difference Between ERC721 vs ERC1155](https://www.altcoinbuzz.io/bitcoin-and-crypto-guide/the-difference-between-erc721-vs-erc1155/).

Today, we will use the ERC721 smart contract template.

### Deploy the contract with Starton

We will deploy the contract only once, so we will do it directly from Starton’s dashboard.

We can access the list of templates in the [Smart contract section](/Smart-contract/understanding-smart-contracts.md).

1. Select the ERC721 template.
1. Enter:
    - a name for your contract,
    - a wallet to sign the transaction,
    - a blockchain / network on which to deploy, here BNB testnet.
    - the parameters of our contract.
      For more information on parameters, check out the [Deploying a Smart Contract](/Smart-contract/parameters-and-functions.mdx).

For example, we can call our contract “Best NFTs on BNB” and deploy it on the BNB Testnet network.

The following constructor parameters are:

-   Name: This name stored on blockchain, we will use “Best NFTs on BNB”.
-   Symbol: The symbol that will be displayed on blockchain explorers for example. We’ll use “BNFTBNB”.
-   Base Uri: This corresponds to the root of the url that will be used to find the content. We’ll use “ipfs://ipfs/“ as we store the content on IPFS.
-   Owner Or Multi Sig Contract: This is the address of the owner of the smart contract. We will put our Starton account address that can be found in the “Wallets” section and should be the one chosen at the top as well.
-   Contract Uri Suffix: This corresponds to the CID of your contract-level metadata on IPFS. This is needed for example if you want to perceive fees when your NFTs are sold on OpenSea.

We can finally deploy our contract!

With our smart contract deployed, you are redirected on the page where we will interact with our contract.

## Minting an NFT

The process of minting a new NFT and sending it to an address goes in three steps:

-   We upload the content of our NFT on IPFS (as it is too heavy to be stored on-chain) and get the CID of the content.
-   We upload a metadata object as a JSON file on IPFS as we do not reference the content directly in the contract. Instead, we put the CID of the content in a metadata object that we upload on IPFS.
-   We call the function “mint” of our smart contract, giving the CID of our metadata object and the address that will receive the NFT.

You can choose to mint NFT from code or from Starton's interface.

<Tabs>
<TabItem value="code" label="From Code" default>

## Minting an NFT from code

You will see next how to upload dynamically our images on IPFS from code with the API.

### Prepare our connection to the Starton API

```jsx showLineNumbers
const axios = require("axios")
const FormData = require("form-data")
const starton = axios.create({
	baseURL: "https://api.starton.com/v3",
	headers: {
		"x-api-key": "YOUR_STARTON_API_KEY",
	},
})
```

:::caution

Do not forget the replace the x-api-key value by your own! It is needed to authenticate yourself with our API.

:::

You can find your API keys and generate new ones in [the API section](https://app.starton.com/api).

### Upload an image on IPFS with the Starton API

We’ll also use IPFS to store the content that will be referenced in our deployed contract.
We do not store the content directly on blockchain as it is too heavy and would induce a very high cost.
The best solution is to store it somewhere else and only store a reference on-chain.

We can create a simple function like this one:

```jsx showLineNumbers
// The image variable should be a buffer
async function uploadImageOnIpfs(image, name) {
	let data = new FormData()
	data.append("file", image, name)
	data.append("isSync", "true")

	const ipfsImg = await starton.post("/ipfs/file", data, {
		maxBodyLength: "Infinity",
		headers: { "Content-Type": `multipart/form-data; boundary=${data._boundary}` },
	})
	return ipfsImg.data
}
```

By calling this function, providing our image as a buffer as a parameter, we should get back an object containing our image’s CID which we will use next in the metadata object.

### Upload the metadata json of our NFT

Consult the [metadata standard format](https://docs.element.market/welcome-to-element/) on your marketplace's documentation.
We can define a new function using our image’s CID to upload the metadata on IPFS:

```jsx showLineNumbers
async function uploadMetadataOnIpfs(imgCid) {
	const metadataJson = {
		name: `A Wonderful NFT`,
		description: `Probably the most awesome NFT ever created !`,
		image: `ipfs://ipfs/${imgCid}`,
	}
	const ipfsMetadata = await starton.post("/ipfs/json", {
		name: "My NFT metadata Json",
		content: metadataJson,
		isSync: true,
	})
	return ipfsMetadata.data
}
```

Feel free to change the name and description that suit your needs.

### Mint the NFT on the smart contract using the metadata’s CID

Finally, we can call our smart contract “mint” function with the receiver’s address and the CID of the metadata we just uploaded:

```jsx showLineNumbers
const SMART_CONTRACT_NETWORK = "binance-testnet"
const SMART_CONTRACT_ADDRESS = ""
const WALLET_IMPORTED_ON_STARTON = ""
async function mintNft(receiverAddress, metadataCid) {
	const nft = await starton.post(`/smart-contract/${SMART_CONTRACT_NETWORK}/${SMART_CONTRACT_ADDRESS}/call`, {
		functionName: "mint",
		signerWallet: WALLET_IMPORTED_ON_STARTON,
		speed: "low",
		params: [receiverAddress, metadataCid],
	})
	return nft.data
}
```

You will need to modify the SMART_CONTRACT_ADDRESS and WALLET_IMPORTED_ON_STARTON variables so they match your contract and your wallet.

### Assemble everything to complete the flow

We can now make the complete flow in only a few lines of code.

> Notice that you need to define the receiver of the NFT and set the imgBuffer variable.

```jsx showLineNumbers
const RECEIVER_ADDRESS = ""
const imgBuffer = ""
const ipfsImg = await uploadImageOnIpfs(imgBuffer, "filename.png")
const ipfsMetadata = await uploadMetadataOnIpfs(ipfsImgData.cid)
const nft = await mintNft(RECEIVER_ADDRESS, ipfsMetadata.cid)
```

</TabItem>
<TabItem value="dashboard" label="From Webapp">

## Minting an NFT from Starton's interface

### Upload an image on IPFS

We’ll also use IPFS to store the content that will be referenced in our deployed contract.
We do not store the content directly on blockchain as it is too heavy and would induce a very high cost.
The best solution is to store it somewhere else and only store a reference on-chain.

1. Go to **IPFS**.
1. Click **Upload**.
1. Select File(s).
1. Select content.
1. Enter a name for the file.
1. Click **Upload**.

Once our image uploaded, let's upload its metadata so that we can call it from our smart contract function.

### Upload your image's metadata on IPFS

:::info

Consult the [metadata standard format](https://docs.element.market/welcome-to-element/) on your marketplace's documentation.

:::

1. Go to **IPFS**.
1. Click **Upload**.
1. Select JSON.
1. Select content.
1. Enter a name for the file.
1. Click **Upload**.

Everything we need to mint our NFT is done! Let's start minting!

### Calling the mint function

This is the last step of your journey. We're going to create the NFT for what you uploaded on IPFS.

1. In **Smart Contract**, select the smart contract we have created: “Best NFTs on BNB”.
1. Select the function 'mint'.
1. Enter the parameters:
    1. In **To**, enter the receiving address.
    1. In **URI**, enter the CID of your NFT metadata.
1. Click **Run**.

</TabItem>
</Tabs>

## Results

![my first NFT](img/my-first-NFT-on-BNB.png)

Annnnnd it’s done! Congratulations!

Once all of this is executed, the content should be on IPFS, and associated to the given address in our ERC721 contract.

You can check our NFT on the [market element marketplace](https://testnets.element.market/collections/best-nfts-on-bnb). Use your contract address to modify the following link: https://testnets.element.market/assets/bsctest/YOUR_CONTRACT_ADDRESS/0

## Conclusion

We have seen in this tutorial how to upload NFTs on a decentralised file system, how to deploy an ERC721 smart contract using Starton, how to make it compatible with marketplaces standards and how we can dynamically mint the NFTs from code to send them to people!

We hope you liked this tutorial and that you will follow along in this epic journey of making Web3 the new standard for Internet!
We are very eager to see what you can build with NFTs.
Do not hesitate to share what you’ve done!
