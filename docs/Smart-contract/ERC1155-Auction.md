---
title: ERC1155 NFT collection Sale in an Auction
description: The smart contract standard to manage unique tokens.
keywords: [ERC-1155, ERC1155, Deploy, Starton, Smart contracts, parameters, functions]
---

# ERC1155 NFT collection Sale in an Auction

<p>The smart contract template to help you sell ERC1155 tokens in the form of an auction. In a video game, you can sell a piece of equipment to the player placing the highest bid.</p>
	<p>Notice you can start a new auction by simply calling the startNewAuction function without having to deploy a new contract.</p>

## Parameters

| Parameter                     | Description                                                                                     |
|-------------------------------| ----------------------------------------------------------------------------------------------- |
| **definitiveTokenAddress**    | The token address of the ERC721 that you want to sell.                                          |
| **definitiveFeeReceiver**     | The address that will receive the amount paid for the NFTs.                                     |
| **initialStartingPrice**      | The initial price offered for the NFT.                                                          |
| **initialMinPriceDifference** | The minimum price increase to place a bid on top of the current auction winner.                 |
| **initialStartTime**          | The time at which the sale will begin and users can bid. Timestamp in seconds.                  |
| **initialEndTime**            | The time when the sale will end and users couldn't bid anymore. Timestamp in seconds.           |
| **initialTokenId**            | The token id of the token that will be minted for the auction. |
| **initialTokenAmount**        | The amount of tokens that will be minted for the auction. |

## Functions

| Function             | Input Parameters                                                                                                                                | Description                                                                                                                                                         |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| token                | None                                                                                                                                            | Returns the NFT contract where the new tokens will be minted at.                                                                                                    |
| currentPrice         | None                                                                                                                                            | Returns the current price of the auction.                                                                                                                           |
| minPriceDifference   | None                                                                                                                                            | Returns the minimum difference between the currentPrice and the bided amount that there should be.                                                                  |
| currentAuctionWinner | None                                                                                                                                            | Returns the current winner of the auction. Notice that if it’s equal to 0 then there isn’t any winner yet.                                                          |
| startTime            | None                                                                                                                                            | Returns the start time of the sale.                                                                                                                                 |
| endTime              | None                                                                                                                                            | Returns the end time of the sale.                                                                                                                                   |
| tokenId              | None                                                                                                                                            | Returns the id that the token being auctioned will have.                                                                                                            |
| tokenAmount          | None                                                                                                                                            | Returns the amount of tokens being auctioned.                                                                                                                       |
| bid                  | None                                                                                                                                            | Bid for the auction and set the price of the bid by the amount you send to the contract.                                                                            |
| claim                | None                                                                                                                                            | Mint a new token with the tokenURI to the winner of the auction.                                                                                                    |
| startNewAuction      | (uint256 newStartingPrice, uint256 newMinPriceDifference, uint256 newStartTime, uint256 newEndTime, uint256 newTokenId, uint256 newTokenAmount) | Start a whole new auction with new parameters you can define. Notice that no current auction can be ongoing and the caller must be the owner of the smart contract. |
| withdraw             | None                                                                                                                                            | Withdraw the price paid for the mints.                                                                                                                              |
| owner                | None                                                                                                                                            | Returns the owner of the smart contract.                                                                                                                            |
| renounceOwnership    | None                                                                                                                                            | Renounce the ownership of the smart contract.                                                                                                                       |
| transferOwnership    | (address newOwner)                                                                                                                              | Transfer the ownership of the contract to a specified address.                                                                                                      |
