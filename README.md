# Solidity-Homework

## Overview:
    Inro
    Contract 1
    Contract 2
    Testnet Addresses
    Appendix

## Intro

For this homework assignment I built smart contracts using the Solidity and the Remix IDE. A quick primer on Solidity - it is a high-level programming language that can interact with the Ethereum Virtual Machine (EVM), which in itself is a blockchain-based software platform. Solidity (according to the documentation) was influenced by C++, Python, JavaScript. I would have liked to see some more Python in there but I am heavily biased (and somewhat inexperienced in the other languages). All jokes aside it was fun going through the documentation to pick up all the new syntactic challenges for this new language! Moreover, Remix is a very user-friendly, web-based development environment where you can practice writing/administering Solidity smart contracts (emphasis on user friendly - all you have to do is go to: remix.ethereum.org). 

## Contract 1: TieredProfitSpliter

This contract splits profits between employees based on percentages for different tiers of employees (i.e. CEO, CTO, foot soldier named Bob). Using this contract, the owner, which could be personnel in HR, can deposit funds (in wei or ether) which will then be distribituted to three employees, each at different "levels," with the remainder going back to the CEO (employee_one). 

When the contract is deployed a few important things happen with the help of a constructor. A constructor function can be thought of as similiar to an instantiation of key variables in the contract. The constructor function is only executed upon contract creation. In other words, external function calls or any interaction with your contract on the blockchain will not change what you have initialized in your constructor. You do not need a constructor function in your contract. For TieredProfitSplitter the purpose of the constructor function is twofold. First, it assigns the employee addresses to new variables so that they are not hardcoded into the contract (and visible by anyone on the blockchain). Second, it designates the current 'msg.sender' at time of contract deployment as the owner of the contract. This can be thought of as someone in HR or Payroll, but the point is that after contract deployment only this person, or more specifically, only this address, can call the 'deposit' function of the contract and distribute funds to the employees. 

To deploy the contract, simply connect your Metamask account to a viable ETH network (I used Kovan for this demonstration), and enter the three addresses representing the employees for whom the profits are split. Once deployed, there are two functions that can be called. The 'balance' function is a 'view' function and does not change state variables. It simply returns the balance of ether (or wei) that the contract holds. Since the purpose of this contract is to distribute all profits to employees, it also serves as a check to ensure the correct functionality of the contract i.e. the contract is not meant to store ether itself, so that balance should always return '0'. The 'deposit' function allows the designated owner to send funds into the contract, which are then split among the employees based on predetermined percentages. For example, the CEO receives 60% of the funds, the CTO receives 25%, and Bob, dear Bob, receives 10%. Note how these figures do not add up to 100%. Not to worry, the remainder is calculated by subtracting the running total from the 'msg.value' and is set back to the CEO. There is a third function as well, designated using the 'fallback' keyword. A fallback function is executed on a call to the contract if none of the other functions match the given function signature, or if no data was supplied at all and there is no receive Ether function (docs.soliditylang.org). Like the name implies, it serves as a 'fallback' or a safeguard of sorts. In this case we call the 'deposit' function from within the fallback to ensure that the logic in 'deposit' executes if ether is sent directly to the contract. This is important to prevent ether from being locked in the contract, since we don't have a 'withdraw' function for this particular contract. Please see Appendix A for screenshots of this contract, and the accompanying gif showcasing the functionality. 

## Testnet Addresses

Employee 1: 0xd11B6223050D958C88FFdD6cB93948B4580CA43c
Employee 2: 0x44E4866576c761eF1dF82dF13cCFAA9854B2e586
Employee 3: 0x44E4866576c761eF1dF82dF13cCFAA9854B2e586

Contract 1:

    Transaction hash (deployment): 0x2ebd9b9a155a215eb33c0e93a4ab441194cd71e0f0c3909e3f702c53be5f3eef

    Transaction hash (depositing funds):
    0x8333f66a8762a8bf45d91ff927bbaa02a3df8116039c54a098417fa3ad35f74b

## Appendix

### Appendix A

Contract Overview:

![Screen Shot 2021-03-02 at 10 31 20 AM](https://user-images.githubusercontent.com/70069238/109671845-6fe0e600-7b42-11eb-8300-6714860d288b.png)

Functionality Gif:

https://user-images.githubusercontent.com/70069238/109672026-9bfc6700-7b42-11eb-819b-81a06cdc079e.mp4

Transaction Hash (Depositing):

![Screen Shot 2021-03-02 at 10 31 20 AM](https://user-images.githubusercontent.com/70069238/109672406-feedfe00-7b42-11eb-9cce-4bd03f7ffc6f.png)









