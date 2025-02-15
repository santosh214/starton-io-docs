---
title: How did we build a Meta-Transaction Site with Web3Auth and Starton
---

# How did we build a Meta-Transaction Site with Web3Auth and Starton

![Starton Web3auth](img/2.png)

Use Case: We wanted to build a DApp that allows users to transfer Starton tokens to other users. He wants to make the transfer process as easy and cost-effective as possible by using meta transactions.

> **What is a meta transaction?**
Meta-transactions are a type of transaction on the blockchain that allow users to initiate transactions without needing to pay gas fees or have Ether in their wallets. Instead, the gas fees are paid by a relayer or a third-party that submits the transaction to the network.
>

To achieve this, we decided to use Web3Auth, a library that allows users to sign transactions using their Web3 wallets without needing to pay for gas fees. He also chooses to use Starton, a token built on the blockchain, for his DApp.

To build the solution, we followed three steps:

## Instanciating Web3Auth using the integration builder

   We started by installing the Web3Auth library and instantiating it in his front-end code. We then created a button that will trigger the Web3Auth authentication modal when clicked. The modal will prompt the user to connect with their social login.

  
   >
   >To integrate Web3Auth, you can use the Integration builder by making sure you select the [W3A modal](https://web3auth.io/docs/integration-builder?lang=NEXT&chain=ETH&evmFramework=WEB3&customAuth=NONE&mfa=DEFAULT&whitelabel=NO&useModal=YES&web3AuthNetwork=TESTNET&rnMode=EXPO&stepIndex=0).
   >


   Once the user is connected, we use the Web3Auth wallet to sign the transaction wrapped in the meta-transaction later, to ensure the transaction is valid and authorized by the user's wallet.

   To retrieve the Starton token balance of the user, we also created a function that calls the Starton smart contract and returns the user's token balance.

## Building a server-side to protect your API key 

   On the server side, we created two routes to handle the request for funds and the execution of meta transactions.

   The `requestFunds` function will accept the user's address and the number of funds they wish to receive. We used a route that calls this function and returns the requested funds to the user.

   The `executeMetaTransaction` function will accept the signed meta transaction from the user's Web3 wallet and execute it on the network. We then used a route that calls this function and executes the meta transaction on the network.

## Creating a Front-End Integration

   To integrate the functionality into the front end of his DApp, we created buttons and models that connect to the functions on the server side.

   When the user clicks the **Request Funds** button, the front end will call the `requestFunds` route on the server side, which will return the requested funds to the user.

   When the user clicks the "Send MetaTransaction" button, a modal window will open, prompting the user to enter the recipient address and amount to transfer. The front end will then sign the transaction using Web3Auth and send it to the server side for execution using the `executeMetatransaction` route.

   By following these steps, we were able to build a Meta-Transaction site using Web3Auth and Starton. His users can now transfer Starton tokens to other users using meta transactions, making the process more efficient and cost-effective.


Want to use our project? Go to our tutorial section, [CREATING METATRANSACTIONS WITH WEB3AUTH AND STARTON](https://docs.starton.com/docs/Tutorials/starton-web3auth)
