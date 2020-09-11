---
layout: post
title:  "Azure Key Vault Lifecycle"
date:   2016-08-01 16:20:59 +0000
categories: Azure KeyVault
---

In the previous post of Introduction to Azure Key Vault, we learnt about the basics of Azure Key Vault, keys and Authentication mechanism. In this blog we will be continuing with Key Vault lifecycle i.e., go through the process of creating and managing Azure Key Vault with PowerShell.

![Azure Key-Vault Lifecycle](/assets/2016-08-01-01.png)


 - To use the Key Vault service we have to create the Key Vault which can be done by using the cmdlet **New-AzureRmKeyVault**.
 - It is more than likely that this key vault would have been created for one or more applications to use. You must register those applications in Azure Active Directory. Also one must authorize them to use your vault with the cmdlet **Set-AzureRmKeyVaultAccessPolicy**. Optionally one can use the cmdlet: **Set-AzureRmKeyVaultAccessPolicy** to allow other users (say your team members) to add/remove keys to this key vault, the simple way is to authorize those users for those specific operations on your key vault.
- An authorized user, can then add secrets (e.g. passwords) and cryptographic keys to the key vault. This can be done with the cmdlet: **Set-AzureKeyVaultSecret** and **Add-AzureKeyVaultKey** respectively. For each secret or key that we add, **we get a unique URI**.
- The application operator is the one who configures applications to use the URI of your key vault (or of specific secrets or keys). The specific configuration steps are unique to each application; generally the application will provide you a place in it’s configuration to paste the URI of your key or secret.
- Subsequently, the application we authorized can use the key vault programatically using the Key Vault REST API or Key Vault Client classes. The application can do the following:
  - It can **read or write secrets** into your key vault, in case it is authorized for those operations.
  - It can **use your keys** by calling methods of the service such as **decryptor sign**. The application cannot read (extract) your keys. This is because the Key Vault service performs the cryptographic operation on the application’s behalf.
  - The application may also **add**, **remove**, or **update keys** in your key vault, if authorized for those specific operations, but this is less common.
- The key vault owner, or a delegate such as an auditor, will be able to monitor operations performed on your keys and secrets by retrieving a usage log from the Key Vault service link.
- At any point, we can update values of existing keys or secrets by using the cmdlet **Add-AzureKeyVaultKey** with the existing key name. **Set-AzureKeyVaultKey** can be used with the existing secret name.
- When you are done using your keys and secrets, delete them using the cmdlets **Remove-AzureKeyVaultKey** and **Remove-AzureKeyVaultSecret** respectively.
- At any point, you can revoke users and applications from accessing your key vault, or authorize other users and applications by using the cmdlets **Remove-AzureRmKeyVaultAccessPolicy** and **Set-AzureRmKeyVaultAccessPolicy**.
- When you are done using the key vault, delete the entire key vault using the cmdlet **Remove-AzureRmKeyvault**.


For step-by-step instructions to create your first key vault and authorize an application using PowerShell and REST APIs stay tuned for _**next part**_.