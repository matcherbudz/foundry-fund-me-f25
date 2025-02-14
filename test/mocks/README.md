# Foundry Fund Me

This solidity code is a simple crowd sourcing project. The person who deploys the FundMe contract is assigned the owner. Users can deposit Ethereum and the contract keeps track of the users and what they fund. If anyone who is not the owner tries to withdraw the funds it will not work. When the owner withdraws it resets all the users balances to zero and using call withdraws the funds to the owners wallet.

# Getting started

## Requirements
[git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- type `git --version` this will tell you if you have git installed or not if not install git
[foundry](https://getfoundry.sh/)
same with foundry type `foundry --version` if not installed follow instructions to install foundry

### Quickstart