# Programmatically Create Your First Transaction on Fireblocks - SPARK 2022 Lab

**Coding Level: Beginners; basic proficiency in working with a command prompt.**

## Overview

The objective of this lab is to allow you to experience basic workspace automation by executing basic API calls with the Fireblocks Software Development Kit (SDK).

**Exercise Outline:**
  1. Create vault account and asset wallets
  2. Create your first Vault to Vault transaction
  3. Getting a list of vault accounts, filtered

## Prerequisites Validation & Testnet Workspace Access:
  1. The Lab exercise will be performed on a designated shared testnet workspace for which you will receive access with an API user.
  2. Approach the Lab Partners and request an API user - that includes an API Key ID and an API Secret Key (Private key). In addition, validate with Lab Partner that you have satisfied all the prerequisites for your operating system.
  3. The function of the API Key ID is to identify the client responsible for a given API call. This API Key ID is not a Secret, and must be included in each request. The Secret Key is used for authentication, and should only be known to the client and to the API service.
  4. The API user is pre-configured for automated signing using the Fireblocks Communed Cosigner Machine for testing.
  5. The Secret must be saved on the computer from which the API calls will be made.


## Lab Workflow
### Step 1: Initialize project

**Project Directory:** Create a project directory by running the following set of commands in your mac Term, Windows Command Prompt or within a terminal window of your Integrated Development Environment (IDE):
```
cd ~/<someplace>
mkdir spark22-API-basics-lab
cd spark22-API-basisc-lab
```
**Note:** 'someplace' should be replaced with the directory of where you wish to save your project files:

**Project File:** 
In the terminal:
  1. Create a file and save it as: **APILABSPARK22.js** (.js extension)
  2. Copy the following SDK Initialization snippet into this file:
```
const FireblocksSDK = require("fireblocks-sdk").FireblocksSDK;
const fs = require('fs');
const path = require('path');
const apiKey = "Your API Key"
const apiSecret = fs.readFileSync(path.resolve(__dirname, "fireblocks_secret.key"), "utf8");;
const fireblocks = new FireblocksSDK(apiSecret, apiKey);
```
**In the code block above, insert the following values in each of the following variables, replacing the current placeholders:**
  1. In the variable 'apiKey', insert your API key.
  2. In the variable 'apiSecret', insert the full path to your Secret Key (ending with /fireblocks_secret.key). Alternatively, move the fireblocks_secret.key to the spark22-API-basisc-lab directory**

Next, you will be prompted to save the file. Save the file in the spark22-API-basics-lab directory.

### Step 2: Create a Vault Account and an ETH_TEST Asset Wallet

The following code utilizes the SDK functions that are relevant to creating a vault account and adding an ETH_TEST asset wallet. 

- Add the following code block to your **APILABSPARK22.js** file.
``` 
async function createVaultAccount(name){
  let vault = await fireblocks.createVaultAccount(name);
  console.log(vault);
}

async function createVaultAsset(vaultAsset, vaultId){
  let vaultasset = await fireblocks.createVaultAsset(vaultAsset, vaultId);
  console.log(vaultasset);
}

```
**Code Execution Workflow**
  1. Execution of each of these functions is performed by specifying the function's name within your code.
  2. Each function includes required parameters for which it is defined to recieve as inputs. These parameters are specified within the parentheses ().
  3. To apply the above explanation, note that the **createVaultAccount** function is defined with one parameter, **name**.
  4. For the purpose of this exercise, the **name** parameter of this vault account should be Spark22-[yourname] (replace the 'yourname' string with your name).
  5. Have your code execute the **createVaultAccount** function by adding the following line of code to your **APILABSPARK22.js** file, and pasting the name defined in item 4 in the parentheses:
```
createVaultAccount("Spark22-[yourname]-1");
```
  6. Save the file. 
  7. Open terminal and navigate to the directory where the file is saved (cd ~/folder name)
  8. Run the following command:

```
node APILABSPARK22.js
```
  9. The **createVaultAccount** function returns a JSON structured like this:
```
{
  "id": "string",
  "name": "string",
  "hiddenOnUI": false,
  "customerRefId": "string",
  "autoFuel": false,
  "assets": []
}
```

**Note:** "id": "string" is a key-value pair. The "string" value here is the vault account id that will be used as a parameter for the **createVaultAsset** function.

**Code Execution Workflow**
  1. The **createVaultAsset** function is defined with two parameters: **vaultAsset** and **vaultId**.
  2. Have your code execute the **createVaultAsset** function by performing the following two steps: 
      - Comment out the execution of the prior functions you've called in the above steps by adding // (two forward slashes) in the beginning of the row
      - Add the following line of code to your **APILABSPARK22.js** file and replace the **vaultAsset** string with "ETH_TEST" and the vaultId with the id string from the response received by the **createVaultAccount** function above.

```
createVaultAsset("ETH_TEST", "vaultId");
```
  3. Save the file. 
  4. In your terminal, run the following command:

```
node APILABSPARK22.js
```
  5. The **createVaultAsset** function returns a JSON structured like this:
```
{
  "id": "string",
  "address": "string",
  "legacyAddress": "string",
  "tag": "string",
  "eosAccountName": "string"
}
```

### Fund your Vault:
**Ask your Fireblocks Lab Partner to send 0.1 ETH_TEST to your new vault account, providing the ID for that vault.**

### Step 3: Vault to Vault Transaction
  1. Repeat step #2.#5 to create one additional vault account: createVaultAccount(Spark22-[yourname]-2);
  2. Document the Id of the new vault account.
  3. Add the following code block to your **APILABSPARK22.js** file, that will transfer 0.001 ETH_TEST between the two vault accounts.
```

sync function createTransaction(){
    const payload = {
      assetId: "ETH_TEST",
        source: {
          type: "VAULT_ACCOUNT",
          id: "replace with vault account Id" // **Id of Spark22-[yourname]-1**
        },
        destination: {
          type: "VAULT_ACCOUNT",
          id: "replace with vault account Id" // **Id of Spark22-[yourname]-2**        },
      amount: "0.001",
      note: "Spark22-[yourname]: My first Sweep transaction"
    };
    const result = await fireblocks.createTransaction(payload);
    console.log(JSON.stringify(result, null, 2));
  }

```
**Code Execution Workflow**
  1. In the code block above, replace the parameters of source and destination vault account IDs with the id's of the vault accounts you have created.
  2. Add your name to the transaction note.
  3. Have your code execute the **createTransaction** function by performing the following two steps: 
      - Comment out the execution of the prior functions you've called in the above steps by adding // (two forward slashes) in the beginning of the row
      - Add the following line of code to your **APILABSPARK22.js**

```
createTransaction();
```
  4. Save the file.
  5. In your terminal run the following command:

```
node APILABSPARK22.js
```
  6. The **createTransaction** function returns a JSON structured like this:
```
{
  "id": "string",
  "status": "string"
}
```
### Step 4: Get a list of vault accounts, filtered
  1. The function included in the following code block below will retrieve a list of vault accounts matching your selected **criteria**.
  2. Add the following code block to your **APILABSPARK22.js** file. 

```
async function getVaultAccounts(){
  getVaultAccounts = await fireblocks.getVaultAccountsWithPageInfo({namePrefix: "Spark22-[yourname]", assetId: "ETH_TEST"});
  console.log(getVaultAccounts);
}
```
  3. In our case we will filter the names of the vault account created as the namePrefix argument, as used when you created the vault accounts above. 
  4. Have your code execute the **getVaultAccounts** function by performing the following two steps:
      - Comment out the execution of the prior functions you've called in the above steps by adding // (two forward slashes) in the beginning of the row
      - Add the following line of code to your **APILABSPARK22.js** file:
```
getVaultAccounts();
```
  5. Save the file.
  6. In your terminal run the following command:
```
node APILABSPARK22.js
```
  7. The **getVaultAccounts** function returns a JSON structured like this:
```
[
  {
    "id": "string",
    "name": "string",
    "hiddenOnUI": false,
    "customerRefId": "string",
    "autoFuel": false,
    "assets": [
      {
        "id": "string",
        "total": "string",
        "available": "string",
        "pending": "string",
        "lockedAmount": "string",
        "totalStakedCPU": "string",
        "totalStakedNetwork": "string",
        "selfStakedCPU": "string",
        "selfStakedNetwork": "string",
        "pendingRefundCPU": "string",
        "pendingRefundNetwork": "string"
      }
    ]
  }
]
```
# Success!
