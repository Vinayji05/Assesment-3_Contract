# Transaction Contract

A Smart Contract for Token Transfers and Ether Management. TransactionContract is a Solidity smart contract that enables users to deposit Ether, transfer tokens, and withdraw Ether securely. It also provides an emergency withdrawal feature for the owner.

## Description

TransactionContract allows users to interact with a smart contract by depositing Ether, transferring tokens between accounts, and withdrawing Ether. The contract includes safety checks to prevent unauthorized actions and logs important events for transparency. And can be used for financial services.

## Getting Started

### Installing

* Make sure you have the necessary development tools installed, such as Node.js.

### Executing program

1.Open In Remix

2.Compile

3.Deploy

## Code

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TransactionContract {
    address public ownerOnly;
    mapping(address => uint256) public balanceofAccount;

    //Events
    event TokensSent(address indexed from, address indexed to, uint256 amount);
    event AmountDeposited(address indexed from, uint256 amount);
    event AmountWithdrawn(address indexed to, uint256 amount);
    event EmergencyWithdrawal(address indexed to, uint256 amount);
 
    //Initialization
    constructor() {
        ownerOnly = msg.sender;
    }

    //Token Transfer
    function sendingtheTokens(address _TO, uint256 _AMOUNT) public {
        require(balanceofAccount[msg.sender] >= _AMOUNT, "Insufficient balance.");
        assert(balanceofAccount[_TO] + _AMOUNT >= balanceofAccount[_TO]);
        balanceofAccount[msg.sender] -= _AMOUNT;
        balanceofAccount[_TO] += _AMOUNT;
        emit TokensSent(msg.sender, _TO, _AMOUNT);
    }

    //Depositting
    function depositingtheAmount() public payable {
        balanceofAccount[msg.sender] += msg.value;
        emit AmountDeposited(msg.sender, msg.value);
    }

    //Withdrawing
    function withdrawingofAmount(uint256 _AMOUNT) public {
        require(balanceofAccount[msg.sender] >= _AMOUNT, "Insufficient balance to withdraw.");

        (bool sent, ) = msg.sender.call{value: _AMOUNT}("");
        require(sent, "Failed to send Ether");

        balanceofAccount[msg.sender] -= _AMOUNT;
        emit AmountWithdrawn(msg.sender, _AMOUNT);
    }

    //Emergency Withdrawal
    function emergencyWithdrawingtheAmount() public {
        if (msg.sender != ownerOnly) {
            revert("Only the owner can perform emergency withdrawal.");
        }

        uint256 contractBalance = address(this).balance;
        (bool sent, ) = ownerOnly.call{value: contractBalance}("");
        require(sent, "Failed to send Ether");

        balanceofAccount[ownerOnly] = 0;
        emit EmergencyWithdrawal(ownerOnly, contractBalance);
    }

    receive() external payable {
        depositingtheAmount();
    }

    fallback() external payable {}
}
```

## Help

Advise for common problems or issues are

1.Ensure Sufficient Balance: Make sure the sender has enough balance for token transfers and Ether withdrawals.

2.Check Contract Ownership: Only the owner can perform certain actions, such as emergency withdrawals.


```
truffle help

```

## Authors

VINAY

vinayseh244@gmail.com


## License

This project is licensed under the [Transaction Contract] License - see the LICENSE.md file for details
