---
title: ERC20 Event transfer
---

# ERC20 Event transfer (EVENT_TRANSFER)

Triggers when an ERC20 contract emits a transfer event, to track when tokens are moved.  
The associated address should be the address of the smart contract on which the event is emitted.
Here is an example of the payload sent to the webhook as a POST request when the event is triggered.
You can find more details about the [transaction object](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionResponse) (please notice that it inherits the [Transaction](https://docs.ethers.io/v5/api/utils/transactions/#Transaction).
The receipt object is described [here](https://docs.ethers.io/v5/api/providers/types/#providers-TransactionReceipt).

```jsx

{
  "projectId": "string",
  "event": "EVENT_TRANSFER",
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
    "transfer": {
      "to": "string", // Address of the receiver
      "from": "string", // Address of the sender
      "value": { // Amount transfered
        "type": "string", // Which is most of the time "BigNumber"
        "hex": "string" // Hex value for the value expressed as the "type" variable
      },
      "contractAddress": "string"
    }
  }
}
```
