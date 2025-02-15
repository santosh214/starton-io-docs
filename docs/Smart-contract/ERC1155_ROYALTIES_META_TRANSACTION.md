---
title: ERC1155 Deployment of a Collection of NFTs (with royalties)
description: The smart contract standard to manage unique tokens.
keywords: [ERC-1155, ERC1155, Deploy, Starton, Smart contracts, parameters, functions]
---

# ERC1155 Deployment of a Collection of NFTs (with royalties)

This meta-transaction version enables you to send transactions on behalf of your users so they can use their NFT without having to pay for gas fees.
It also includes the blacklist feature. It allows the owner to block one or more addresses from transferring tokens on behalf of your users for example if you want to block the sales happening in a marketplace.
Notice that we do not store any content in the smart contract. Smart contract store only references. It is up to smart contract readers to find the content using references.
Starton provides an <a href="https://app.starton.com/ipfs">IPFS integration</a> in case you need to host your NFT content.

:::caution

To use this contract, you will need to import the etherjs library, create and sign a typeTransaction before you can use the function executeMetadata().

:::

## Parameters

-   **definitiveName:** The name of your smart contract which will be reflected on-chain.
-   **initialTokenUri:** The token URI of the collection
    -   Using IPFS: `ipfs://ipfs/QmW77ZQQ7Jm9q8WuLbH8YZg2K7T9Qnjbzm7jYVQQrJY5Y/{id}/`
    -   Using a centralised server: `https://yourapi.com/endpoint/{id}`
-   **initialContractUri:** The URI of the metadata that will be used to give more details about the description.
-   **initialOwnerOrMultisigContract:** The address that will own the NFT Collection
-   **definitiveRoyaltyFee:** The fraction of sale price representing the royalty fees
-   **definitiveFeeReceiver:** The address that will receive the royalty fees

## Functions

| Function               | Input Parameters                                                                              | Description                                                                                                                                                            |
| ---------------------- | --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PAUSER_ROLE            | None                                                                                          | Returns the value of the pauser role.                                                                                                                                  |
| MINTER_ROLE            | None                                                                                          | Returns the value of the minter role.                                                                                                                                  |
| LOCKER_ROLE            | None                                                                                          | Returns the value of the locker role.                                                                                                                                  |
| METADATA_ROLE          | None                                                                                          | Returns the value of the metadata role.                                                                                                                                |
| DEFAULT_ADMIN_ROLE     | None                                                                                          | Returns the address of the admin role.                                                                                                                                 |
| pause                  | None                                                                                          | Called to pause by address with a pauser role, disables every variable state changes of the contract.                                                                  |
| paused                 | None                                                                                          | Returns true when the contract is paused. Returns false otherwise.                                                                                                     |
| unpause                | None                                                                                          | Called to unpause by address with a pauser role.                                                                                                                       |
| executeMetaTransaction | (address userAddress, bytes memory functionSignature, bytes32 sigR, bytes32 sigS, uint8 sigV) | Executes a transaction on behalf of another user by providing their address, the function to call and the signature of the transaction.                                |
| getDomainSeparator     | None                                                                                          | Returns the domain separator according to the EIP712.                                                                                                                  |
| getChainId             | None                                                                                          | Returns the chain id according to the EIP712.                                                                                                                          |
| supportsInterface      | (bytes4 interfaceID)                                                                          | Returns true if the contract implements the specified interface.                                                                                                       |
| hasRole                | (bytes32 role, address account)                                                               | Returns true if the address specified has been granted the role.                                                                                                       |
| getRoleAdmin           | (bytes32 role)                                                                                | Returns the role that can control the specified role.                                                                                                                  |
| grantRole              | (bytes32 role, address account)                                                               | Grants a role to the address specified.                                                                                                                                |
| revokeRole             | (bytes32 role, address account)                                                               | Removes a role from an address.                                                                                                                                        |
| renounceRole           | (bytes32 role, address account)                                                               | Removes a role from an address. The address must be the caller address.                                                                                                |
| burn                   | (address account,uint256 id,uint256 value)                                                    | Erases a specified amount of token with specified id.                                                                                                                  |
| burnBatch              | (address account, uint256[] memory ids, uint256[] memory values)                              | Batch erases a specified amount of token with specified id.                                                                                                            |
| name                   | None                                                                                          | Returns the descriptive name of the smart contract.                                                                                                                    |
| safeTransferFrom       | (address from, address to, uint256 id, uint256 amount, bytes memory data)                     | Transfer a amount of token of specified id to another address. Notice that the spender must be either way the owner or have the approval to transfer this token.       |
| safeBatchTransferFrom  | (address from, address to, uint256 tokenId, bytes memory data)                                | Batch transfer a amount of token of specified id to another address. Notice that the spender must be either way the owner or have the approval to transfer this token. |
| balanceOf              | (address account, uint256 id)                                                                 | Returns the amount of tokens owned with the id owned by account.                                                                                                       |
| balanceOfBatch         | (uint256 tokenId)                                                                             | Returns the address that own the token.                                                                                                                                |
| tokenURI               | (uint256 tokenId)                                                                             | Returns the URI for a token.                                                                                                                                           |
| URI                    | None                                                                                          | Returns the URI of the token.                                                                                                                                          |
| setApprovalForAll      | (address operator, bool approved)                                                             | Set the approval of all tokens owned to a true or false that a operator can transfer.                                                                                  |
| isApprovedForAll       | (address account, address operator)                                                           | Returns true if the operator can transfer all the tokens owned by account                                                                                              |
| contractURI            | None                                                                                          | Returns a public URL that contains the metadata of the collection                                                                                                      |
| lockMetadata           | None                                                                                          | Revokes the ability to change metadata. Sender must have the LOCKER_ROLE.                                                                                              |
| lockMint               | None                                                                                          | Revokes the ability to mint. Sender must have the LOCKER_ROLE.                                                                                                         |
| mint                   | (address to, uint256 id, uint256 amount, bytes memory data)                                   | Mint nth new token to an address with a specified id.                                                                                                                  |
| mintBatch              | (address to, uint256[] memory ids, uint256[] memory amounts, bytes memory data)               | Batch mint nth new token to an address with a specified id.                                                                                                            |
| mint                   | (address to, uint256 id, uint256 amount)                                                      | Mint nth new token to an address with a specified id.                                                                                                                  |
| mintBatch              | (address to, uint256[] memory ids, uint256[] memory amounts)                                  | Batch mint nth new token to an address with a specified id.                                                                                                            |
| setContractURI         | (string memory newContractURI)                                                                | Change the contract URI to a new URI.                                                                                                                                  |
| setTokenURI            | (string memory newTokenURI)                                                                   | Change the token URI to a new URI.                                                                                                                                     |
