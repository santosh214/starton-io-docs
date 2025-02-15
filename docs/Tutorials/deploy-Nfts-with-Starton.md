---
title: Deploy your NFTs with Starton
displayedSidebar: tutorialSidebar
---

# Deploy your NFTs on blockchain with Starton

In this tutorial, we will see how we can deploy a smart contract and interact with it to mint NFTs dynamically from code.
We will use random images uploaded on IPFS (a distributed file storage system) and assigned to an Ethereum address.

You will :

-   Use a template from Starton for our smart contract (ERC721).
-   Deploy it with Starton from the dashboard. To deploy contracts from code, see [Deploying a smart contract from code](deploy-a-contract-from-code.md).
-   Upload the content of our NFTs and their metadata on IPFS.
-   Interact with our smart contract using Starton API in order to mint the NFTs and give it to a specific address.

If you feel stuck or have questions, feel free to get in touch in our [Discord](deploy-a-contract-from-code.md).
We’ll be glad to help you!

## Choose a smart contract template

Several standards of smart contracts have been developed for NFTs.

The most used ones are ERC721 and the ERC1155.
The main big difference between the two is that in an ERC721, every NFT is unique which means you will have to reference the content for each of them.
Meanwhile the ERC1155 enables you to create “collections” where there are several copies of the same NFT.

The ERC721, which is easier to use, still can be used to upload several copies of the same content, but is less optimised for this use case than the ERC1155.
If you want to see in more details the differences between the two standards, read [The Difference Between ERC721 vs ERC1155](https://www.altcoinbuzz.io/bitcoin-and-crypto-guide/the-difference-between-erc721-vs-erc1155/).

Today, we will use the ERC721 smart contract template.

We’ll also use IPFS to store the content that will be referenced in our deployed contract.
We do not store the content directly on blockchain as it is too heavy and would induce a very high cost.
The best solution is to store it somewhere else and only store a reference on-chain.

## Deploy the contract with Starton

We will deploy the contract only once, so we will do it directly from Starton’s dashboard.

We can access the list of templates in the [Smart contract section](/Smart-contract/understanding-smart-contracts.md).

1. Select the ERC721 template.
1. Enter:
    - a name for your contract,
    - a wallet to sign the transaction,
    - a blockchain / network on which to deploy,
    - the parameters of our contract.
      For more information on parameters, check out the [Deploying a Smart Contract](/Smart-contract/deploying-a-smart-contract.mdx).

For example, we can call our contract “My Super NFTs” and deploy it on the Polygon Mumbai network.

The following constructor parameters are:

-   Name: This name stored on blockchain, we will keep “My Super NFTs”.
-   Symbol: The symbol that will be displayed on blockchain explorers for example. We’ll use “MSNFT”.
-   Base Uri: This corresponds to the root of the url that will be used to find the content. We’ll use “ipfs://ipfs/“ as we store the content on IPFS.
-   Owner Or Multi Sig Contract: This is the address of the owner of the smart contract. We will put our Starton account address that can be found in the “Wallets” section and should be the one chosen at the top as well.
-   Contract Uri Suffix: This corresponds to the IPFS cid of your contract level metadata. This is needed for example if you want to perceive fees when your NFTs are sold on OpenSea.

To be able to fill this field we need to upload a JSON file that respects OpenSea’s specification for the contract level metadata on IPFS.

Lost on the Contract Uri Suffix part? Let’s see this in more details so we can finish the deployment of our contract.

## Upload the contract level metadata on IPFS

We need to implement [OpenSea’s specification](https://docs.opensea.io/docs/contract-level-metadatafor) the contract level metadata.

> This enables us to add a name on the OpenSea’s dashboard and perceive fees when someone sells one of our NFTs.

Here are the values we will use:

```jsx
{
  "name": “My Super NFTs“,
  "description": “You’ve never seen NFTs this beautiful.”,
  "image": "",
  "external_link": "",
  "seller_fee_basis_points": 100,
  "fee_recipient": “PUT YOUR ADDRESS HERE”
}
```

We now have the content of our metadata, we need to upload it on IPFS.

On IPFS, the content is not referred using an address like we are used to with urls, but by the content’s value.

Let’s see the difference:
When we query some content based on location we implicitly ask:
"Give me the content that is located at https://… no matter what it is that you find".

Whereas when we query some content based on its value on IPFS we implicitly ask:
"Find the content on the network with the hash XXXXXXX and give it to me, no matter who where it is".

With the location based approach we are specific on the “How” and not on the “What” while with the content based approach it is the opposite.
Using the content based approach, we are sure we get the content we want, without it being altered, replaced or infected as otherwise the hash would have changed as well.

And it is a game changer as we do no longer need to rely on trust!

The hash of the content is called a Content IDentifier (CID) on IPFS.

And when we will upload our contract-level metadata on IPFS we will get a CID back, which is the value we need to put as the Contract Suffix Uri in our smart contract.

To upload our files on IPFS we will now use the [Starton IPFS](/IPFS/understanding-IPFS.md) pinning service.

As the contract-level metadata only needs to be uploaded once, we can directly do it from [our dashboard](https://app.starton.com/ipfs)
Once done, a column “CID” appears with a value for our file that starts with “Qm”. This is the value we need for the Contract Uri Suffix of our smart contract!
We can finally deploy our contract!

With our smart contract deployed, we are redirected on the page to interact with our contract.

We won’t interact with it from the dashboard but you can click on the smart contract address at the top to see our contract in the blockchain explorer.
You will see next how to upload dynamically our images on IPFS from code with the API.

## Minting an NFT

The process of minting a new NFT and sending it to an address goes in three steps:

-   We upload the content on IPFS (as it is too heavy to be stored on-chain) and get the CID of the content.
-   We upload a metadata object as a JSON file on IPFS as we do not reference the content directly in the contract. Instead, we put the CID of the content in a metadata object that we upload on IPFS.
-   We call the function “mint” of our smart contract, giving the CID of our metadata object and the address that will receive the NFT.

### Prepare our connection to the Starton API

```jsx
const axios = require("axios")
const FormData = require("form-data")
const starton = axios.create({
	baseURL: "https://api.starton.com/v3",
	headers: {
		"x-api-key": "YOUR_STARTON_API_KEY",
	},
})
```

> Do not forget to replace the x-api-key value by your own! It is needed to authenticate yourself with our API.

You can find your API keys and generate new ones in [the API section](https://app.starton.com/api).

### Upload an image on IPFS with the Starton API

We can create a simple function like this one:

```jsx
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

Consult the [metadata standard format](https://docs.opensea.io/docs/metadata-standards) on OpenSea documentation.
We can define a new function using our image’s CID to upload the metadata on IPFS:

```jsx
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

```jsx
const SMART_CONTRACT_NETWORK = "polygon-mumbai"
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

Annnnnd it’s done! Congratulations!

Once all of this is executed, the content should be on IPFS, and associated to the given address in our ERC721 contract.
You should be able to see the NFTs on [OpenSea](https://testnets.opensea.io/assets/mumbai/0xA148a37fCC67CFFda10cB07d0b971a7941952300/0) or via this link [https://testnets.opensea.io/collection/my-super-nfts](https://testnets.opensea.io/collection/my-super-nfts).

## Conclusion

We have seen in this tutorial how to upload NFTs on a decentralised file system, how to deploy an ERC721 smart contract using Starton, how to make it compatible with OpenSea’s standards and how we can dynamicaly mint the NFTs from code to send them to people!

<div class="row-is-multiline">

<div class="col col--2" class="cards">
	<a class="button-card button-card--vertical" href="https://app.starton.com/projects">
		<h3>Check your Smart contract on the Dashboard</h3>
		<div class="button-card__inner">
			<p color="white">Go to <b>Starton Dashboard</b> and check all the transactions of your smart contract at one glance.</p>
		</div>
	</a>
</div>

</div>

We hope you liked this tutorial and that you will follow along in this epic journey of making Web3 the new standard for Internet!
We are very eager to see what you can build with NFTs.
Do not hesitate to share what you’ve done!
