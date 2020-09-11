---
layout: post
title:  "Azure Key Vault Overview"
date:   2016-07-06 16:20:59 +0000
categories: Azure KeyVault
---

**Azure Key Vault** is a service that enables us to store & manage cryptographic keys and secrets in one central secure vault. All the sensitive data is stored on physical hardware security modules (HSM) – FIPS 140-2 Level 2 certified – inside the datacenter where the data will be encrypted by VMs or directly on the HSM, more on this later. Pricing of Azure Key Vault can be found here.

A **Vault Owner** can create a Key Vault gaining full access & control over the vault.

A **Vault Consumer** can perform actions on the assets inside the Key Vault when the vault owner grants him/her access and depending on the permissions granted.

This enables us to give the customers full control on their sensitive data – They can decide how their key lifecycle looks and who has access to it. Based on the audit logs they are aware of what the consumers are doing and if they are still trustworthy.

On the other hand, developers are now no longer responsible for storing sensitive data such as API tokens, certificates & encryption keys. Operators also will no longer be able to see sensitive data in the database, web.config, etc.

### Secrets

A secret is a sequence of bytes limited to 10 kB to which we can assign any value, this can be a certificate, string or whatever we want.

The consumer can save or read back values based on the name of the secret, if they have the required permissions. It basically is a Key-Value store that encrypts your data and stores it in the HSM.

It’s important to know that consumers will receive value of the secret as plain-text.

### Keys

A key is a cryptographic RSA 2048 key that consumers can use for typical key operations such as encrypt, decrypt, sign, verify, etc. Key Vault will handle all these operations for the consumers because they can’t read back the value.

All keys are encrypted and stored in physical HSMs but come in two flavours :

 - **Software Keys** are using Azure VMs to handle operations on the keys. They are pretty cheap but less secure. These keys are typically used for dev/test scenarios.

 - **HSM Keys** are performing key operations directly on the HSM and thus more secure. However, these keys are more expensive and require us to use a Premium-tier vault.

A key has a higher latency than a secret, if we need to frequently use the key it is recommended to store it as a secret.

### Authentication

Azure Key Vault leverages enterprise-grade authentication & authorization by integrating with **Azure Active Directory** where we grant a person or application in your directory access to the vault with a specific set of permissions.

Here is a nice overview of how the authentication process works:

![Azure Key-Vault Overview](/assets/2016-07-06-01.png)

We can authenticate with Azure Active Directory by using our Account Id & Secret or Account Id & Certificate. We then use the granted token and give it to Key Vault along with the operation which we want to perform.

Anybody with an Azure subscription and Administrator Privileges can sign in with an Azure subscription, create a vault for the organization in which to store keys, and then be responsible for operational tasks, such as:

 - Create or import a key or secret
 - Revoke or delete a key or secret
 - Authorize users or applications to access the key vault, so they can then manage or use its keys and secrets
 - Configure key usage (for example, sign or encrypt)
 - Monitor key usage

We can create or manage azure key vault using PowerShell, REST API, .Net libaries on NuGet or the preview SDK for Node.

### Summary

The Azure Key Vault is a secure service that allows you to have control over keys and passwords using Hardware Security Modules. Also, it allows you to assign permissions to applications depending on your need. Further it allows users to manage or create key vaults using PowerShell, REST API’s etc. 