---
title: Granting Full Access on your KMS to Starton
description: Learn how to connect AWS Key Management Systems
keywords: [Wallet, Starton, Smart contracts, AWS, Key Management System, Transaction]
---
# Granting Full Access on your KMS to Starton

> Granting full access to **Starton** enables you to dynamically create new wallets with the Starton API.

## Creating a new policy before the IAM creation

Before granting access to your KMS, you need to create a Policy to define permissions associated to the IAM we will create.

1. On AWS, go to the **Identity and Access Management (IAM)** dashboard.
1. In **Access Management**, go to **Policies**. (img)
1. Click **Create Policy**.
1. Go to **JSON**.
1. Copy the following .json and paste it in the tab.

```jsx
  {
      "Version": "2012-10-17",
      "Statement": [
          {
              "Effect": "Allow",
              "Action": "iam:CreateServiceLinkedRole",
              "Resource": "arn:aws:iam::*:role/aws-service-role/*"
          },
          {
              "Effect": "Allow",
              "Action": "kms:*",
              "Resource": "*"
          }
      ]
  }

```

1. Click on **Next:Tags**.
1. Click **Review**.
1. Enter a **Name** for the policy.
1. Click **Create policy**.

## Create a new IAM user for Starton

1. Access [AWS Users](https://console.aws.amazon.com/iamv2/home?#/users).
1. Click **Add users**.
1. Set username to `kms`.

> WARNING
>
> Setting username to kms is mandatory. Do not enter another username.

1. In **Select AWS access type**, check **Access key - Programmatic access**.

![Configure key](assets/add-user.png)

1. Select **Attach existing policies directly**.
1. Select the kms policy name.
1. Click **Next:Tags**.

> Adding tags is optional.

1. Click **Next:Review**.
1. Review the kms user:

> AWS access type must be set to Programmatic access - with an access key.

1. Click **Create user** to get the **Access Key Id** and **Secret Access Key** for your KMS.

## Importing a Key Management System  on Starton

1. From the Dashboard, click **Settings**.
1. In **KMS**, click **+ KMS**.
1. Enter your KMS information:

| Parameter             | Description                                                                                                               |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **KMS name**          | The name of your Key Management System on the **Dashboard**.                                                              |
| **Account id**        | The 12 digit number you can find it in the top-right corner of your AWS Dashboard.                                        |
| **Access key id**     | The **Access Key ID** of the new IAM user available after completing [this step](#create-a-new-iam-user-for-starton).     |
| **Secret access key** | The **Secret access key** of the new IAM user available after completing [this step](#create-a-new-iam-user-for-starton). |
| **Region**            | The **Region** on which you want to create the wallet. For example `eu-west-3`.                                           |

1. Click **Create**.

You can now dynamically create new wallets from your code or from the interface.


:::info CREATING A WALLET FROM A KMS 

1. To create a wallet from your KMS, go to **Wallet**. 
1. Click **+Wallet** and click **CONNECT YOUR KMS AND GRAND FULL ACCESS TO STARTON**. 
1. From there, you can select the KMS imported. 
1. Click Next. 

::: 

**Related topics**

-   More on [Transactions](/Transactions/creating-a-transaction.mdx)
-   More on [Smart Contracts](/Smart-contract/understanding-smart-contracts.md)
-   More on [Developer mode](/Developer/Discovering-coding-interface.md)
