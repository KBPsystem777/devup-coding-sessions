# ➡️ Deployment

## Compiling the Smart Contract



From your terminal, enter the following command to compile your smart contract:

{% code overflow="wrap" %}
```bash
npx hardhat compile
```
{% endcode %}

If you compiled it succefully, you should see the same output below

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

You will also notice that after compiling, an `artifact` folder has been created on our repo

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption><p>The artifacts folder contains the compilation and metadata of our contract including ABI</p></figcaption></figure>

## Deployment Script

By default, Hardhat provided us already a ready build deployment script for Lock.sol which can be located in `ignition/modules/Lock.js` folder

{% embed url="https://gist.github.com/KBPsystem777/7db9769c0c65f093f9488c156586ac4a" %}
Deployment script for Lock.sol
{% endembed %}

### Deployment Script Explanation

This script is part of Hardhat's boilerplate and is used to define and configure a module for deploying a smart contract. The script utilizes the `@nomicfoundation/hardhat-ignition` library, which simplifies the deployment process for Ethereum smart contracts.

Let's break down the script step-by-step:

**Importing Required Modules**

```javascript
const { buildModule } = require("@nomicfoundation/hardhat-ignition/modules");
```

Here, the script imports the `buildModule` function from the `@nomicfoundation/hardhat-ignition/modules` package. This function is used to define a deployment module.

**Defining Constants**

```javascript
const JAN_1ST_2030 = 1893456000;
const ONE_GWEI = 1_000_000_000n;
```

Two constants are defined:

* `JAN_1ST_2030` is a timestamp representing January 1st, 2030.
* `ONE_GWEI` is a value representing one gwei in wei (1 gwei = 1,000,000,000 wei).

**Exporting the Deployment Module**

```javascript
module.exports = buildModule("LockModule", (m) => {
  const unlockTime = m.getParameter("unlockTime", JAN_1ST_2030);
  const lockedAmount = m.getParameter("lockedAmount", ONE_GWEI);

  const lock = m.contract("Lock", [unlockTime], {
    value: lockedAmount,
  });

  return { lock };
});
```

This part of the script defines and exports the deployment module named "LockModule". Here's what happens inside the `buildModule` function:

1.  **Parameters Configuration**

    ```javascript
    const unlockTime = m.getParameter("unlockTime", JAN_1ST_2030);
    const lockedAmount = m.getParameter("lockedAmount", ONE_GWEI);
    ```

    The script retrieves parameters for `unlockTime` and `lockedAmount`. If these parameters are not provided, it defaults to the predefined constants `JAN_1ST_2030` and `ONE_GWEI` respectively.
2.  **Contract Deployment**

    ```javascript
    const lock = m.contract("Lock", [unlockTime], {
      value: lockedAmount,
    });
    ```

    This line deploys the `Lock` contract with the `unlockTime` parameter and sends `lockedAmount` of Ether to the contract upon deployment. The `m.contract` function is used to specify the contract to be deployed and its constructor parameters.
3.  **Return Deployed Contract Instance**

    ```javascript
    return { lock };
    ```

    The deployed contract instance is returned as part of an object. This allows other modules or scripts to access the deployed contract.

Once everything are all set, from your terminal, enter the command: `npx hardhat ignition deploy ./ignition/modules/Lock.js --network sepolia`

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption><p>Successful deployment of Lock.sol in Sepolia Network</p></figcaption></figure>

And that's it! You may verify the deployed contract in [https://sepolia.etherscan.io/address/0x0c28cade9467a8dac6ef14ab3ddf6f1b79f3f092](https://sepolia.etherscan.io/address/0x0c28cade9467a8dac6ef14ab3ddf6f1b79f3f092)

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>
