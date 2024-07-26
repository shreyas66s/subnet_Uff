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

## Code
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    uint public totalSupply;
    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;
    string public name = "shreyasss";
    string public symbol = "SHS";
    uint8 public decimals = 18;

		event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}



interface IERC20 {
    function totalSupply() external view returns (uint);

    function balanceOf(address account) external view returns (uint);

    function transfer(address recipient, uint amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}

contract Vault {
    IERC20 public immutable token;

    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function _mint(address _to, uint _shares) private {
        totalSupply += _shares;
        balanceOf[_to] += _shares;
    }

    function _burn(address _from, uint _shares) private {
        totalSupply -= _shares;
        balanceOf[_from] -= _shares;
    }

    function deposit(uint _amount) external {
        /*
        a = amount
        B = balance of token before deposit
        T = total supply
        s = shares to mint

        (T + s) / T = (a + B) / B 

        s = aT / B
        */
        uint shares;
        if (totalSupply == 0) {
            shares = _amount;
        } else {
            shares = (_amount * totalSupply) / token.balanceOf(address(this));
        }

        _mint(msg.sender, shares);
        token.transferFrom(msg.sender, address(this), _amount);
    }

    function withdraw(uint _shares) external {
        /*
        a = amount
        B = balance of token before withdraw
        T = total supply
        s = shares to burn

        (T - s) / T = (B - a) / B 

        a = sB / T
        */
        uint amount = (_shares * token.balanceOf(address(this))) / totalSupply;
        _burn(msg.sender, _shares);
        token.transfer(msg.sender, amount);
    }
}
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
