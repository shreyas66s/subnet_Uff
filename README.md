# Project Title
ERC20 Token and Vault Contract

## Description
*This project includes an implementation of a basic ERC20 token contract and a Vault contract. The ERC20 token contract allows for the creation and management of a fungible token named "shreyasss" with the symbol "SHS". The Vault contract allows users to deposit and withdraw these tokens, managing shares corresponding to the deposited amount.

# Getting Started

## Prerequisites

* Solidity compiler (solc) version 0.8.17 or later
Ethereum development environment such as Remix, Truffle, or Hardhat

# Installing

* Clone the repository to your local machine.
* Navigate to your preferred development environment and create a new project.
* Copy the provided ERC20 and Vault contract code into the respective Solidity files within your project.
Executing Program
* Compile the Contracts:
Open your Solidity development environment and compile the contracts to ensure there are no syntax errors.

* Deploy the ERC20 Token Contract:
Deploy the ERC20 contract first. Note the deployed contract address as it will be used in the Vault contract deployment.

* Deploy the Vault Contract:
Deploy the Vault contract using the address of the deployed ERC20 token contract.

* Interact with the Contracts:
* Mint Tokens:
Call the mint function on the ERC20 contract to create new tokens.

```
erc20Contract.mint(amount);
```
* Approve Vault:
Approve the Vault contract to spend your tokens.
```
erc20Contract.approve(vaultContractAddress, amount);
```
* Deposit Tokens:
Deposit tokens into the Vault contract to receive shares.

```
vaultContract.deposit(amount);
```
* Withdraw Tokens:
Withdraw tokens from the Vault contract by burning shares.
```
vaultContract.withdraw(shares);
```

## Help
If you encounter any issues or have questions, consider the following:

* Ensure the ERC20 token address used in the Vault contract constructor is correct.
* Verify that the token balance and allowances are properly set before depositing into the Vault.
* Double-check the Solidity compiler version to match pragma solidity ^0.8.17;.

## Authors
[S Shreyas]
[shreyas1665@gmail.com]

## License
This project is licensed under the MIT License - see the LICENSE.md file for details.
