---
title: Setting a transaction management strategy
---

# Setting a transaction management strategy

When sending a transaction, you can face issues with processing.
First, you got nonce issues. When sending a transaction without having the correct nonce, the next nonce available in database and the nonce of the blockchain might be desynchronized.

Then, if the gas speed set is low and there are a lot of transactions to be sent to the blockchain, minors won't be incentivized to pick yours.

With Starton, you can set your project strategy so that our Relayer increases the gas speed and handles stuck transactions.

## Setting your Relayer strategy

Setting the strategy of your Relayer allows Starton to manage stuck transactions until they reach a maximum amount. You need to set these values by using Starton's API. First, we will check the default Relayer strategy, then we will set your own custom strategy.

:::info

Transaction management strategy is set at a project level. You need to set it for each network at a time. As gas price is different for every network, please make sure you specify the right amount for the right network in your request.

:::

### Checking your current Relayer strategy

To check your current Relayer strategy, you can use the following snippet. Simply enter your api key
and select the network on which you want to set up your Relayer strategy.

```jsx showLineNumbers
const axios = require("axios")

const startonAPI = axios.create({
    baseURL: "https://api.starton.com",
    headers: {
        "x-api-key": "YOUR_API_KEY",
    },
})

startonAPI.get("/v3/setting/relayer/polygon-mumbai")
    .then(res=>console.log(res.data))
    .catch(e=>console.log(e))
```

Before you set your strategy, by default, this request will return:

```jsx showLineNumbers
{
"id": "string",
"projectId": "string",
"network": "{network}",
"unstuckMaxGasPrice": "string",
"unstuckMissingNonce": true,
"unstuckMissingNonceDelay": 0,
"unstuckCustomGasPrice": true,
"unstuckAutomaticGasPrice": true,
"unstuckGasPriceDelay": 0,
"createdAt": "2023-01-11T16:17:54.201Z",
"updatedAt": "2023-01-11T16:17:54.201Z"
}
```

The following parameters will be used to set your own gas strategy:

-   "network": the network on which strategy is set
-   "unstuckMaxGasPrice": the maximum gas price for which you allow Starton to reprocess stuck transactions
-   "unstuckMissingNonce": whether or not you allow Starton to help with stuck missing nonces
-   "unstuckMissingNonceDelay": number of seconds you wish to wait for Starton to act on missing nonce,
-   "unstuckCustomGasPrice": whether or not you allow Starton to handle issues related to a transaction with custom gas price,
-   "unstuckAutomaticGasPrice": whether or not you allow Starton to handle issues related to a transaction with chosen gas speed,
-   "unstuckGasPriceDelay": number of seconds you wish to wait for Starton to act on transactions with a gas price issue

### Setting your Relayer strategy

To set your current Relayer strategy, you can use the following snippet. Simply enter your api key
and select the network on which you want to set up your Relayer strategy. Then, you will set the parameters, seen above, according to your needs.

```jsx showLineNumbers
const axios = require("axios")

const startonAPI = axios.create({
    baseURL: "https://api.starton.com",
    headers: {
        "x-api-key": "YOUR_API_KEY",
    },
})

startonAPI.patch("/v3/setting/relayer/binance-testnet", {
    unstuckMaxGasPrice: "150000000000", // price in wei.
    unstuckMissingNonce: true,
    unstuckMissingNonceDelay: 300, // in seconds
    unstuckCustomGasPrice: true,
    unstuckAutomaticGasPrice: true,
    unstuckGasPriceDelay: 300, // in seconds
})
    .then(res=>console.log(res.data))
    .catch(e=>console.log(e))
```

If the request is successful, your settings have been changed. To understand any other return, you can see our [API reference](https://docs.starton.com/intro#tag/SettingRelayer).
