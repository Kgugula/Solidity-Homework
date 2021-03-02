# Solidity-Homework

## Overview:
    Inro
    Contract 1
    Contract 2
    Testnet Addresses
    Appendix
    Documentation

## Intro

For this homework assignment I built smart contracts using the Solidity and the Remix IDE. A quick primer on Solidity - it is a high-level programming language that can interact with the Ethereum Virtual Machine (EVM), which in itself is a blockchain-based software platform. Solidity (according to the documentation) was influenced by C++, Python, JavaScript. I would have liked to see some more Python in there but I am heavily biased (and somewhat inexperienced in the other languages). All jokes aside it was fun going through the documentation to pick up all the new syntactic challenges for this new language! Moreover, Remix is a very user-friendly, web-based development environment where you can practice writing/administering Solidity smart contracts (emphasis on user friendly - all you have to do is go to: remix.ethereum.org). 

## Contract 1: TieredProfitSpliter

This contract splits profits between employees based on percentages for different tiers of employees (i.e. CEO, CTO, foot soldier named Bob). Using this contract, the owner, which could be personnel in HR, can deposit funds (in wei or ether) which will then be distribituted to three employees, each at different "levels," with the remainder going back to the CEO (employee_one). 

When the contract is deployed a few important things happen with the help of a constructor. A constructor function can be thought of as similiar to an instantiation of key variables in the contract. The constructor function is only executed upon contract creation. In other words, external function calls or any interaction with your contract on the blockchain will not change what you have initialized in your constructor. You do not need a constructor function in your contract. For TieredProfitSplitter the purpose of the constructor function is twofold. First, it assigns the employee addresses to new variables so that they are not hardcoded into the contract (and visible by anyone on the blockchain). Second, it designates the current 'msg.sender' at time of contract deployment as the owner of the contract. This can be thought of as someone in HR or Payroll, but the point is that after contract deployment only this person, or more specifically, only this address, can call the 'deposit' function of the contract and distribute funds to the employees. 

To deploy the contract, simply connect your Metamask account to a viable ETH network (I used Kovan for this demonstration), and enter the three addresses representing the employees for whom the profits are split. Once deployed, there are two functions that can be called. The 'balance' function is a 'view' function and does not change state variables. It simply returns the balance of ether (or wei) that the contract holds. Since the purpose of this contract is to distribute all profits to employees, it also serves as a check to ensure the correct functionality of the contract i.e. the contract is not meant to store ether itself, so that balance should always return '0'. The 'deposit' function allows the designated owner to send funds into the contract, which are then split among the employees based on predetermined percentages. For example, the CEO receives 60% of the funds, the CTO receives 25%, and Bob, dear Bob, receives 10%. Note how these figures do not add up to 100%. Not to worry, the remainder is calculated by subtracting the running total from the 'msg.value' and is set back to the CEO. There is a third function as well, designated using the 'fallback' keyword. A fallback function is executed on a call to the contract if none of the other functions match the given function signature, or if no data was supplied at all and there is no receive Ether function (docs.soliditylang.org). Like the name implies, it serves as a 'fallback' or a safeguard of sorts. In this case we call the 'deposit' function from within the fallback to ensure that the logic in 'deposit' executes if ether is sent directly to the contract. This is important to prevent ether from being locked in the contract, since we don't have a 'withdraw' function for this particular contract. Please see Appendix A for screenshots of this contract, and the accompanying gif showcasing the functionality. 

## Contract 2: DeferredEquityPlan

In this contract, I modelled the functionality of a deferred equity incentive plan, in which an 'active' employee at the firm receives shares based a schedule with a prescribed vesting period in place. In this example, the total number of shares to be distributed to the employee is 1000 with a four-year vesting i.e. the employee receives 250 shares every year (365 days). If the employee leaves within the first four years, he or she would forfeit ownership of any remaining ("unvested") shares. Although the idea of deferred equity incentive plans is not new, the rationale as some interesting implications when used in the context of smart contracts and DeFi applications. For example, when going after a crowdsale or an ICO, there can be functionality built into the sale (written in the contract itself) such that the founders or other senior executives receiving a large amount of shares cannot sell their stakes for a given time period. Because this information could be made available on the blockchain, and be visible to all, it could serve to temper investor fears of buying into a "pump and dump" scheme or signify the leaders faith and commitment to the company/product. 

Listed in the screenshots are two separate contracts - DeferredEquityPlan and DeferredEquityPlan_TEST. The latter was used to test the functionality of the "unlock" function, whereby the 'distribute' can only be called every 365 days. In the test script I added an additional variable, "fakenow", and a function "fastforward", so that I can "skip" ahead in 100 day increments to test that only 250 shares where being distributed every year, and that more than 1000 shares was not distributed after the four-year period had passed. Please see Appendix B for the corresponding screenshots Contract Overview (Test) and Contract Overview for more information.

In the contract a constructor function is utilized in much the same way as the previous one i.e. 'msg.sender' upon contract deployment is designated as human resources. There are two functions of interest, as well as a fallback function. The first function distributes shares to the employee. If it passes the 'require' statements (which are pretty self-explanatory), the function increments the 'unlock' time by 365 days, and distributes shares equal to (now - start_time) / 365 days * annual distribution. For example, if (now - start_time) is 400 days, the equation will be set to 1 (remainders are discarded) and 1 * 250 will be the amount of shares awarded. This process continues until the vesting time-period has passed. Moreover, the next function allow either human_resources or the employee to "deactivate" their status. Upon deactivation, the "distribute" function will no longer call (because of the require statement in place). 




## Testnet Addresses

Employee 1: 0xd11B6223050D958C88FFdD6cB93948B4580CA43c
Employee 2: 0x44E4866576c761eF1dF82dF13cCFAA9854B2e586
Employee 3: 0x44E4866576c761eF1dF82dF13cCFAA9854B2e586

Contract 1:

    Transaction hash (deployment): 0x2ebd9b9a155a215eb33c0e93a4ab441194cd71e0f0c3909e3f702c53be5f3eef

    Transaction hash (depositing funds):
    0x8333f66a8762a8bf45d91ff927bbaa02a3df8116039c54a098417fa3ad35f74b

Contract 2:
    
    Transaction hash (deployment of test):
    0x06695005f23c3e0a265f3438f088a6995e8929b735c0d242b98cbc5171f9d30b

    Transaction hash (deployment of production): 0x2493c7c55df850d7f196cb5346cc7f676997d20ef082aba340c3ce8c2ec157ed
    
    Transaction hash (distributing shares):
    0xf6e7d1d38dd236d1baaa212e8fe2eaf7070f533b77f2fd51d3de68a31f774e35


## Appendix

### Appendix A

Contract Overview:

![Screen Shot 2021-03-02 at 10 31 20 AM](https://user-images.githubusercontent.com/70069238/109671845-6fe0e600-7b42-11eb-8300-6714860d288b.png)

Functionality Gif:

https://user-images.githubusercontent.com/70069238/109672026-9bfc6700-7b42-11eb-819b-81a06cdc079e.mp4

Transaction Hash (Depositing):

![Screen Shot 2021-03-02 at 10 35 28 AM](https://user-images.githubusercontent.com/70069238/109672653-39579b00-7b43-11eb-9762-7632f8af9a30.png)

### Appendix B

Contract Overview (Test):

![Screen Shot 2021-03-02 at 11 37 09 AM](https://user-images.githubusercontent.com/70069238/109681600-a0794d80-7b4b-11eb-8e72-2f40a5ab301d.png)

Contract Overview:

![Screen Shot 2021-03-02 at 11 38 50 AM](https://user-images.githubusercontent.com/70069238/109681814-d9192700-7b4b-11eb-9e8d-cb11ce684c60.png)

Functionality Gif (Fastforward function):

** Fastforward was called multiple times prior

https://user-images.githubusercontent.com/70069238/109683539-7032ae80-7b4d-11eb-8428-088913391263.mp4

Transaction Hash (Distributing w/ Test):

![Screen Shot 2021-03-02 at 11 57 05 AM](https://user-images.githubusercontent.com/70069238/109684569-62c9f400-7b4e-11eb-80e1-66221887add2.png)

## Documentation

Solidity Documentation - https://docs.soliditylang.org/en/v0.8.2/index.html

Solidity by Example - https://github.com/raineorshine/solidity-by-example

Awesome Solidity - https://github.com/bkrem/awesome-solidity











