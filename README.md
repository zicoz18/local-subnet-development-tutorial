# Set up your development environment for local subnet development

If you are not familiar with subnets, virtual machines or similar terminology you can refer to [Subnet Overview](https://docs.avax.network/subnets).

## Introduction

This tutorial's goal is to deploy and start a basic subnet without much configuration in your local machine so that you can interact with the subnet by [remix](https://remix.ethereum.org/) and [hardhat](https://hardhat.org/). In this tutorial we will be using [avalanche-cli](https://github.com/ava-labs/avalanche-cli).
> avalanche-cli is in beta version. So it might get updated fairly frequently. It is best to refer to latest version by clicking [this](https://github.com/ava-labs/avalanche-cli)

If you want to customize your subnet you can refer to the optional [Customize your subnet](###Customie-Your-Subnet) section.

Steps to follow: 
1. Install avalanche-cli 
2. Create the subnet 
3. Deploy the subnet
4. Interact with the subnet

## Requirements

- Mac or Linux environment
- Golang (//TODO: https://github.com/ava-labs/avalanche-cli burdan açıkla)

## Installation
To download avalanche-cli's latest version, run:
````
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
````
To add avalanche to PATH, run:
````
cd bin
export PATH=$PWD:$PATH
````
> This command will add avalanche-cli to the PATH temporarily, which means that, when you reopen your terminal you would not be able run 'avalanche' command. So, to add it permanantly refer to [Add Avalanche Command Permanently](###Add-Avalanche-Command-Permanently) section.

## Create the Subnet

To create the subnet, run:
```
avalanche subnet create <subnetName>
```

>If you are getting an error, that might be because you have already created a subnet with the same name. To check if that is the case, you can run `avalanche subnet list` which would list the subnets you have. If you have a subnet with the same name, you can try to create with a different name, delete the existing subnet by running `avalanche subnet delete <subnetName>` or overwrite the existing one by running `avalanche subnet create <subnetName> --force`

When you run this command you will be walked through the customization of the subnet. You can learn more about the configuration details [here](https://docs.avax.network/subnets/customize-a-subnet).

Example walk through: 
* Choose your VM: SubnetEVM
* ChainId: 676767
* How would you like to set the fees: Low disk use...
* How would you like to distribute the funds: Airdrop 1 million tokens to the default address
* Would you like to add a precompile to modify the EVM: No

This is a walk through of a basic subnet. If you want to customize the airdrop, add precompiles or do some more customization refer to the [Subnet Customization Options](###Subnet-Custimization-Options) section.

You have created the [genesis](https://docs.avax.network/subnets/customize-a-subnet#genesis) file of your subnet. 
To see details run:
````
avalanche subnet describe <subnetName>
```` 
To see the genesis file directly, run:
````
avalanche subnet describe <subnetName> --genesis
````

## Deploy the Subnet
To deploy the subnet locally, run:
````
avalanche subnet deploy <subnetName> -l
````


>If you get an error,
"Error: failed to query network health: the health check failed to complete. The server might be down or have crashed, check the logs! rpc error: code = Unavailable desc = connection error: desc = "transport: Error while dialing dial tcp :8097: connect: connection refused"
Try to run the command once more or you can check to logs which are located at `$HOME/.avalanche-cli/logs`

>If you get an error,
"Error: failed to start network :rpc error: code = Unknown desc = already bootstrapped"
It means that you have deployed a subnet before and trying without stopping it. To solve this issue, run:
`avalanche network stop` and try to deploy your subnet again.


After a successfull deployment, an example of what you would see:
```
Network ready to use. Local network node endpoints:
Endpoint at node node4 for blockchain "2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf" with VM ID "srEXiWaHq58RK6uZMmUNaMF2FzG7vPzREsiXsptAHk9gsZNvN": http://127.0.0.1:37868/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
Endpoint at node node5 for blockchain "2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf" with VM ID "srEXiWaHq58RK6uZMmUNaMF2FzG7vPzREsiXsptAHk9gsZNvN": http://127.0.0.1:61314/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
Endpoint at node node1 for blockchain "2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf" with VM ID "srEXiWaHq58RK6uZMmUNaMF2FzG7vPzREsiXsptAHk9gsZNvN": http://127.0.0.1:29281/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
Endpoint at node node2 for blockchain "2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf" with VM ID "srEXiWaHq58RK6uZMmUNaMF2FzG7vPzREsiXsptAHk9gsZNvN": http://127.0.0.1:56438/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
Endpoint at node node3 for blockchain "2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf" with VM ID "srEXiWaHq58RK6uZMmUNaMF2FzG7vPzREsiXsptAHk9gsZNvN": http://127.0.0.1:51473/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc

Metamask connection details (any node URL from above works):
RPC URL:          http://127.0.0.1:37868/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
Funded address:   0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC with 1000000 (10^18) - private key: 56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027
Network name:     subnetName
Chain ID:         676767
Currency Symbol:  TEST
```

## Interact With the Subnet
This tutorial will cover interacting with the subnet through [remix](https://remix.ethereum.org/) and [hardhat](https://hardhat.org/).

### Using Remix
Firstly, we will be adding our subnet to [metamask](https://metamask.io/). To add the subnet, refer to [this](https://docs.avax.network/dapps/smart-contracts/create-erc-20-token-on-avalanche-c-chain#set-up-metamask) and replace the values with your subnet's values.
Example Values:
````
Network Name: <subnetName>
New RPC URL: http://127.0.0.1:37868/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
ChainID: 676767
Symbol: TEST
````
Make sure that metamask is configured to use your subnet. If you followed the exact steps in this tutorial, you would see that your balance on metamask is zero. That is because we have only airdropped to the default account which is `0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC`. Therefore, your account on metamask has zero tokens and cannot send any transactions. So, to interact with the chain we have to use the address that is airdropped. 
> If you have customized the subnet and have tokens on your account you can skip the "Import the airdropped account" part

* Import the airdropped account
  1. Click on the account image
  2. Click "Import Account"
  3. Select type: Private Key
  4. Enter private key of the default airdrop account, which is `56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027`
  5. Click "Import"

Now you should see that you have a balance of `1000000` TEST tokens. We have our tokens and we are ready to deploy a smart contract on our subnet. To deploy a contract using remix you could refer to [this](https://docs.avax.network/dapps/smart-contracts/deploy-a-smart-contract-on-avalanche-using-remix-and-metamask#step-3-connect-metamask-and-deploy-a-smart-contract-using-remix). Interacting and deploying smart contracts to your subnet will be same as interacting with  C-chain just make sure that your metamask extension is configured to use your subnet.


### Using Hardhat
To interact with the subnet using Hardhat, refer to [this](https://docs.avax.network/dapps/smart-contracts/using-hardhat-with-the-avalanche-c-chain). It is very similar to interacting with C-Chain. You only have change `hardhat.config.ts` file. Inside that file, find the exported JSON and inside of it find `networks`. Add a new network which will be your subnet.
```
subnet: {
  url: "<yourRpcUrl>",
  chainId: <yourChainId>,
  accounts: ["<accounts-private-key>"]
}
```

Example Values: 
```
subnet: {
  url: "http://127.0.0.1:37868/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc",
  chainId: 676767,
  accounts: ["56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027"]
}
```

Now you can run and commands ran in the tutorial with `--network subnet` parameter



//TODO: Maybe rename as customization details
### Production level details
  * `ChainId`: You want your `ChainId` parameter to be unique. To make sure that your subnet is secure against replay attacks. To see registered `ChainIds` you can check [chainlist.org](https://chainlist.org/). At the top right make sure to turn on the button to include Testnets.
  * `Gas Parameters`: Ava Labs recommends the low-low option and C-Chain currently uses this option. But, if you know what you are doing you are free to customize. Note that higher disk usege has some trade offs, it would require more processing power and cause it to be more expensive to maintain.
  * `Airdrop Address`: You would not like to use the default address in production, that is recieving the 1 million tokens. Because it is a compromised wallet, which means that it's private key is well known by others. If you add a custom address to recieve airdrop. Avalanche-cli will ask you to give amount in AVAX, in that case do not enter the value thinking as in ethers but in Gwei to correctly airdrop the amount you want. Example: To airdrop 1 whole token, as in one ether, you would enter the value 1000000000.
  * `Precompiles`: You can learn what precompiles are by refering to [this](https://docs.avax.network/subnets/customize-a-subnet#precompiles).

### Optional Parts

#### Add Avalanche Command Permanently

//TODO: Fill this part from https://zwbetz.com/how-to-add-a-binary-to-your-path-on-macos-linux-windows/ and https://zwbetz.com/shell-config-file-on-mac/


#### Subnet Custimization Options
#### Currently supported precompiles

- Native Contract Minter
  // TODO: explain
- Transaction Allow List
  // TODO: explain
- Contract Deploy Allow List
  // TODO: explain

#### Stuff
Starting out with developing on subnets might be confusing at first.
So we will be trying to start out by following the simplest path.
Which will be creating a subnet on our local machine.
The perfect tool for this purpose is `avalanche-cli` [ //TODO: Insert link to the avalanche-cli https://github.com/ava-labs/avalanche-cli]
You can refer to `avalanche-cli` for more information about the tool.



This command would print out general information about the tool and explain some usefull commands as well. If you would like to dig deeper refer to `this` //TODO: insert link of the avalanche-cli here

Now it is the exciting time, let's create our subnet run:
`avalanche subnet create <yourSubnetName>`