---
title: ERC1155 Event transfer single
---

# ERC1155 Event transfer single (ERC1155_EVENT_TRANSFER_SINGLE)

Triggers when an ERC1155 contract emits a transfer event, to track when one NFT is moved.
The associated address should be the address of the smart contract on which the event is emitted.
Here is an example of the payload sent to the webhook as a POST request when the event is triggered.
You can find more details about the [transaction object](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse) (please notice that it inherits the [Transaction](https://docs.ethers.io/v5/api/utils/transactions/#Transaction).
The receipt object is described [here](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionReceipt).

```jsx
{
  "projectId": "string",
  "event": "ERC1155_EVENT_TRANSFER_SINGLE",
  "data": {
    "transaction": {
      "hash": "string",
      "type": "number",
      "accessList": [],
      "blockHash": "string",
      "blockNumber": "number",
      "transactionIndex": "number",
      "confirmations": "number",
      "from": "string",
      "gasPrice": {
        "hex": "string",
        "raw": "string"
      },
      "maxPriorityFeePerGas": {
        "hex": "string",
        "raw": "string"
      },
      "maxFeePerGas": {
        "hex": "string",
        "raw": "string"
      },
      "gasLimit": {
        "hex": "string",
        "raw": "string"
      },
      "to": "string",
      "value": {
        "hex": "string",
        "raw": "string"
      },
      "nonce": "number",
      "data": "string",
      "r": "string",
      "s": "string",
      "v": "number",
      "creates": null,
      "chainId": "number"
    },
    "receipt": {
      "to": "string",
      "from": "string",
      "contractAddress": null,
      "transactionIndex": "number",
      "gasUsed": {
        "hex": "string",
        "raw": "string"
      },
      "logsBloom": "string",
      "blockHash": "string",
      "transactionHash": "string",
      "logs": [
        {
          "transactionIndex": "number",
          "blockNumber": "number",
          "transactionHash": "string",
          "address": "string",
          "topics": [
            "string",
            "string",
            "string",
            "string"
          ],
          "data": "string",
          "logIndex": "number",
          "blockHash": "string"
        }
      ],
      "blockNumber": "number",
      "confirmations": "number",
      "cumulativeGasUsed": {
        "hex": "string",
        "raw": "string"
      },
      "effectiveGasPrice": {
        "hex": "string",
        "raw": "string"
      },
      "status": "number",
      "type": "number",
      "byzantium": "boolean"
    },
    "network": "string",
    "blockchain": "string",
    "transferSingle": {
      "operator": "string",
      "to": "string",
      "from": "string",
      "id": {
        "hex": "string",
        "raw": "string"
      },
      "value": {
        "hex": "string",
        "raw": "string"
      },
      "contractAddress": "string"
    }
  }
}
```
