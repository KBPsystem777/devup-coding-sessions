---
description: From your VSCode, checkout Lock.sol smart contract
---

# ➡️ Building the Smart Contract

This page explains the default contract provided in the Hardhat boilerplate. This simple contract, named **`Lock`**, allows an owner to lock up some Ether until a specified unlock time, after which the owner can withdraw the funds.

Head over to your VSCode, open the contracts folder and checkout **Lock.sol**:

{% embed url="https://gist.github.com/KBPsystem777/a978883afdc0cc0230710ad47760d0a5" %}
Lock.sol
{% endembed %}

## Code Explanation

### Detailed Breakdown

#### **SPDX License Identifier**

{% code overflow="wrap" fullWidth="true" %}
```solidity
// SPDX-License-Identifier: UNLICENSED
```
{% endcode %}

* Indicates that the contract is unlicensed. It is important for open-source projects to include a license identifier.



#### **Pragma Directive**

{% code overflow="wrap" %}
```solidity
pragma solidity ^0.8.24;
```
{% endcode %}

* Specifies the version of Solidity compiler to use.



#### **Contract Definition**

{% code overflow="wrap" fullWidth="true" %}
```solidity
contract Lock {
    uint public unlockTime;
    address payable public owner;

    event Withdrawal(uint amount, uint when);
```
{% endcode %}

* Defines a contract named `Lock`.
* Declares state variables `unlockTime` and `owner`.
* Declares an event `Withdrawal` that logs withdrawals.



#### **Constructor**

{% code overflow="wrap" fullWidth="true" %}
```solidity
constructor(uint _unlockTime) payable {
    require(
        block.timestamp < _unlockTime,
        "Unlock time should be in the future"
    );

    unlockTime = _unlockTime;
    owner = payable(msg.sender);
}
```
{% endcode %}

* The constructor is executed once during deployment.
* It takes an `unlockTime` parameter and ensures it is in the future.
* Sets the `unlockTime` and the `owner` of the contract to the deployer.



#### **Withdraw Function**

{% code overflow="wrap" fullWidth="true" %}
```solidity
function withdraw() public {

    require(block.timestamp >= unlockTime, "You can't withdraw yet");
    require(msg.sender == owner, "You aren't the owner");

    emit Withdrawal(address(this).balance, block.timestamp);

    owner.transfer(address(this).balance);
}
```
{% endcode %}

* Allows the owner to withdraw funds after the `unlockTime`.
* Requires the current time to be at least the `unlockTime`.
* Ensures that only the owner can withdraw the funds.
* Emits a `Withdrawal` event and transfers the contract's balance to the owner.
