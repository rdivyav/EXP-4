# Experiment 4: DeFi Lending and Borrowing Protocol

# NAME : Divya R V
# Reg no: 212223100005
# Department: CSE(cyber security)

# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Setup Lending and Borrowing Mechanism
Users deposit ETH into the contract as liquidity.


Depositors receive interest based on their deposits.


Borrowers can borrow ETH but must provide collateral (e.g., 150% of the borrowed amount).


Interest on borrowed funds is calculated dynamically based on utilization rate.


Step 2: Implement Overcollateralization
If a borrower’s collateral value drops below a certain liquidation threshold, their collateral is liquidated to repay the debt.


Step 3: Allow Liquidation
If collateral < liquidation threshold, liquidators can repay the borrower's debt and claim their collateral at a discount.



Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {

    uint256 public interestRate = 20;   // 20% interest per cycle
    uint256 public liquidationThreshold = 150; 

    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Borrowed(address user, uint256 amount, uint256 collateral);
    event InterestAdded(address user, uint256 newDebt);
    event Liquidated(address user, uint256 collateralSeized);

    // Borrow function (Collateral must be given here)
    function borrow(uint256 amount) public payable {

        require(msg.value >= (amount * liquidationThreshold)/100, 
        "Not enough collateral");

        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;

        payable(msg.sender).transfer(amount);

        emit Borrowed(msg.sender, amount, msg.value);
    }

    // Interest added to increase debt
    function addInterest() public {

        uint256 interest = (borrowed[msg.sender] * interestRate)/100;
        borrowed[msg.sender] += interest;

        emit InterestAdded(msg.sender, borrowed[msg.sender]);
    }

    // Liquidation
    function liquidate(address borrower) public {

        require(
        collateral[borrower] < (borrowed[borrower] * liquidationThreshold)/100,
        "Not eligible for liquidation"
        );

        uint seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;

        payable(msg.sender).transfer(seizedCollateral);

        emit Liquidated(borrower, seizedCollateral);
    }

    // Deposit ETH into contract so it can lend
    receive() external payable {}
}
 
```
# Expected Output:
Users can deposit ETH and earn interest.


Users can borrow ETH by providing collateral.


If collateral < 150% of borrowed amount, liquidators can seize the collateral.



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# OUTPUT:
<img width="1920" height="1080" alt="Screenshot 2026-08-19 111112" src="https://github.com/user-attachments/assets/a19ddb08-2c83-4be1-abdd-0ce1e43e037d" />

<img width="1920" height="1080" alt="Screenshot 2026-08-19 111130" src="https://github.com/user-attachments/assets/5a2a4d3f-5d66-4d5c-86b1-32c8d38d1418" />

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/47b5fed5-9915-4cf9-8a77-476df3a3b151" />

<img width="1920" height="1080" alt="Screenshot 2026-08-19 111149" src="https://github.com/user-attachments/assets/0bd91106-de6a-41a4-abcd-d20720e48118" />

# RESULT : 
Thus decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral is executed successfully.
