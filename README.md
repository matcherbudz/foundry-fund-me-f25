# Foundry Fund Me

This solidity code is my first project I am pushing to github!!
Shout out to my awesome teacher Patrick Collins and his team at Cyfrin Updraft. 

This is a simple crowd source funding project. It had a minimum deposit of $5 in ethereum and converts the price of eth to usd using chainlink pricefeeds. Whoever deploys the FundMe contract is assigned the owner. Users can deposit Ethereum and the contract keeps track of the users and the amount they fund. If anyone who is not the owner tries to withdraw the funds it will not work. When the owner withdraws it resets all the users balances to zero and uses a call function to withdraw all the funds to the owners wallet.

# Getting Started

## Requirements

You will need git, foundry, and make 

Before starting, make sure you have them installed:

```bash
git --version

forge --version

make --version
```

Expected outputs: 

```
git version x.x.x 

forge x.x.x(5a8bd89 2024-12-20T08:46:21.555250780Z)

GNU Make 4.3
Built for ...
```
If git or foundry is not installed follow these links and instructions to install them

[git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

[foundry](https://getfoundry.sh/)

make `sudo apt install make`

# Important 

Friendly reminder to never put your private keys in plain text.
## To save your private keys in a encrypted way directly on forge:

Open a seperate terminal from your coding enviroment and type `cast wallet import defaultKey --interactive` defaultKey is the name of the key you're saving you can name this what you like it will prompt you to enter your private key and then give you the public address. 

## To use encrypted private keys in deployment:

Replace `--private-key 0x00` with `--account defaultKey 0x00` and replace the 0x00 with the public address associated with the private key

documentation - https://book.getfoundry.sh/reference/cast/cast-wallet-import

### Optional Gitpod

If you can't or don't want to run and install locally, you can work with this repo in Gitpod. If you do this, you can skip the `clone this repo` part.

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#github.com/matcherbuds/foundry-fund-me-f25)

## Quickstart

```
git clone https://github.com/matcherbudz/foundry-fund-me-f25

cd foundry-fund-me-f25

make
```

# How to use

## Deployment

Check the Makefile to see what the make commands will do. In this case it will spin up and deploy to a local development node. Or you can manually type `anvil` and in another terminal `forge script script/DeployFundMe.s.sol --rpc-url http://127.0.0.1:8545 --account defaultKey 0x00`

To use the Makefile to deploy simply type `make anvil` then in another terminal `make deploy`

## Scripts and cast funding and withdraw

To test our FundMe contract functionality we can use cast to directly interact with the contract.
Also if we want a programatic way to interact with our contract we can use the Interactions script.

(alternative encrypted private key 0x00 being the public address of the private key)`--account defaultKey 0x00`
```
cast send <FUNDME_CONTRACT_ADDRESS> "fund()" --value 0.1ether --private-key <PRIVATE_KEY> 
cast send <FUNDME_CONTRACT_ADDRESS> "withdraw()"  --private-key <PRIVATE_KEY>
```

or


```
forge script script/Interactions.s.sol:FundFundMe --rpc-url sepolia  --private-key $PRIVATE_KEY  --broadcast
forge script script/Interactions.s.sol:WithdrawFundMe --rpc-url sepolia  --private-key $PRIVATE_KEY  --broadcast
```

or we can use make to test the scripts if we do this we need to add this to the foundry.toml to grant filesystem permissions this is default off to protect unauthorized access to your systems files
```
fs_permissions = [
    { access = "read", path = "./broadcast" },
    { access = "read", path = "./reports" }
]
```

```
make fund
make withdraw
```

## Testing

```
forge test
```

or

```
forge test --mt testFunctionName
```

or

```
forge test --fork-url $SEPOLIA_RPC_URL
```


## Test Coverage

```
forge coverage
```

## Esitmate gas and saving money on gas

```
forge snapshot
```
outputs a file called `.gas-snapshot` Also we can check the storage layout by typing `forge inspect FundMe storageLayout` 
If we want to see what is in each individual storage slot either deploy the contract using the makefile or 
type `forge script script/DeployFundMe.s.sol --rpc-url http://127.0.0.1:8545 --private-key 0x00 --broadcast`
(alternative encrypted private key 0x00 being the public address of the private key)`--account defaultKey 0x00`
then copy the contract address and paste it in with `cast storage 0x00 X` replacing the 0x00 with the contract address and X 
being the storage slot for example if we put 2 we get whats at storage slot 3 which is s_priceFeed

# Testing with ZkSync
First we need to install ZkSync foundry `curl -L https://raw.githubusercontent.com/matter-labs/foundry-zksync/main/install-foundry-zksync | bash` documentation https://foundry-book.zksync.io/getting-started/installation

In this lesson I learned a bit about deploying to ZkSync and how with testing some things work with vanilla foundry and some things work with ZkSync in the ZkSyncDevOps.t.sol is a contract that tests this. 

Forgive me for my lack of information here for I am still learning about ZkSync but its cool to know things are improving.

# Additional Info:
Some users were having a confusion that whether Chainlink-brownie-contracts is an official Chainlink repository or not. Here is the info.
Chainlink-brownie-contracts is an official repo. The repository is owned and maintained by the chainlink team for this very purpose, and gets releases from the proper chainlink release process. You can see it's still the `smartcontractkit` org as well.

https://github.com/smartcontractkit/chainlink-brownie-contracts

## Let's talk about what "Official" means
The "official" release process is that chainlink deploys it's packages to [npm](https://www.npmjs.com/package/@chainlink/contracts). So technically, even downloading directly from `smartcontractkit/chainlink` is wrong, because it could be using unreleased code.

So, then you have two options:

1. Download from NPM and have your codebase have dependencies foreign to foundry
2. Download from the chainlink-brownie-contracts repo which already downloads from npm and then packages it nicely for you to use in foundry.
## Summary
1. That is an official repo maintained by the same org
2. It downloads from the official release cycle `chainlink/contracts` use (npm) and packages it nicely for digestion from foundry.

## It is important to note that things are always changing and its possible some of the information here will be outdated so if something doesnt work right look at the websites for documentation and up to date information.