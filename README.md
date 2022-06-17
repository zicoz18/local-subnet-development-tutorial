# Set up your development environment for local subnet development
// TODO: Table ıof contents ekle
If you are not familiar with subnets, virtual machines or similar terminology you can refer to [Subnet Overview](https://docs.avax.network/subnets).

TODO: Cheatsheet, genel adımlar vs diye bölebilirsin. Common Errors diye bir section oluşturulabilir ve belli command'lere göre common error çözümleri sunulabilir

## Introduction

This tutorial's goal is to deploy and start a basic subnet in your local machine. So that you can interact with the subnet using [Remix](https://remix.ethereum.org/) and [Hardhat](https://hardhat.org/). In this tutorial we will be using [avalanche-cli](https://github.com/ava-labs/avalanche-cli), to create and deploy the subnet. If you ever encounter an error refer to [Troubleshoot Common Issues](##Troubleshoot-Common-Issues) section.
> avalanche-cli is in beta version. So it might get updated fairly frequently. It is best to refer to latest version from it's [github page](https://github.com/ava-labs/avalanche-cli).

If you want to customize your subnet you can refer to the optional [Customize the Subnet](##Customize-the-Subnet) section, while creating your subnet.

Steps to follow: 
1. Install avalanche-cli 
2. Create the subnet 
3. Deploy the subnet
4. Interact with the subnet

## Requirements

- Mac or Linux environment

## Step 1: Install avalanche-cli
To build Avalanche-cli you have to first install golang because Avalanche-cli is written in golang. Follow the instructions here: [https://go.dev/doc/install](https://go.dev/doc/install).

After downloading golang, to download avalanche-cli's latest version, run:
````
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
````
>This command will download the bin to the `./` (relative  to where the command has run).

>To download in a custom location go to [Installing in Custom Location](###Installing-in-custom-location).



To add avalanche to PATH, run:
````
cd bin
export PATH=$PWD:$PATH
````
> This command will add avalanche-cli to the PATH temporarily, which means that, when you reopen your terminal you would not be able run 'avalanche' command. So, to add it permanantly refer to [Add Avalanche Command Permanently](###Add-Avalanche-Command-Permanently) section.

#### Installing in Custom Location
To download binary to a specific directory, run:
```
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s -- -b <relative directory>
```

#### Add Avalanche Command Permanently
To add avalanche command to your path:

##### MacOs
1. Open shell config file.
    * If you are using `zsh` shell, open `${HOME}/.zprofile` in a text editor.
    * If you are suing `bash` shell, open `${HOME}/.bash_profile` in a text editor.
2. Add this line, ` export PATH="<path-of-avalanche-bin-directory>:${PATH}"` to the file.
3. Restart the terminal and run `avalanche`.

##### Linux
1. Open shell config file, `${HOME}/.bashrc` in a text editor.
2. Add this line, `export PATH="<path-of-avalanche-bin-directory>:${PATH}"` to the file.
3. Restart the terminal and run `avalanche`.

>If you have ran the installation command at `$HOME` directory. `<path-of-avalanche-bin-directory>` would be `${HOME}/bin`

//TODO: Step 1 için karşılaşılan error'leri çözebilmesi için step1 trouble shooting diye section'a linkle
## Step 2: Create the subnet 

To create the subnet, run:
```
avalanche subnet create <subnetName>
```
// TODO: Bunun içindeki command'leri daha görünebilir hale gelitr
>If you are getting an error, that might be because you have already created a subnet with the same name. To check if that is the case, you can run `avalanche subnet list` which would list the subnets you have. If you have a subnet with the same name, you can try to create with a different name, delete the existing subnet by running `avalanche subnet delete <subnetName>` or overwrite the existing one by running `avalanche subnet create <subnetName> --force`

When you run this command you will be walked through the customization of the subnet. You can learn more about the configuration details at [Subnet Customization Options](###Subnet-Custimization-Options) section.

//TODO: Parametreleri highlight2la
Example walk through: 
* Choose your VM: SubnetEVM
* ChainId: 676767
* How would you like to set the fees: Low disk use...
* How would you like to distribute the funds: Airdrop 1 million tokens to the default address
* Would you like to add a precompile to modify the EVM: No

You have successfully created the [genesis](https://docs.avax.network/subnets/customize-a-subnet#genesis) file of your subnet. 
To see details run:
````
avalanche subnet describe <subnetName>
```` 
To see the genesis file directly, run:
````
avalanche subnet describe <subnetName> --genesis
````

## Step 3: Deploy the subnet
To deploy the subnet locally, run:
````
avalanche subnet deploy <subnetName> -l
````

>If you get an error, "Error: failed to query network health: ..."
You can check to logs which are located at `$HOME/.avalanche-cli/logs` or try to run the command once more.

>If you get an error, "Error: failed to start network : ..."
It means that you have deployed a subnet before and trying to deploy another one without stopping the initial one. To solve this issue, run:
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

Make sure to save `Metamask connection details`. You will need them to interact with your subnet

## Step 4: Interact With the Subnet
This tutorial will cover interacting with the subnet through [Remix](https://remix.ethereum.org/) and [Hardhat](https://hardhat.org/).

### Using Remix
Firstly, we will be adding our subnet to [metamask](https://metamask.io/). To add the subnet, refer to [Deploy a Smart Contract on Your Subnet EVM Using Remix and Metamask](https://docs.avax.network/subnets/deploy-a-smart-contract-on-your-evm#step-1-setting-up-metamask) you should replace the values with your subnet values that are printed out after you have created it.
Example Values:
````
Network Name: <subnetName>
New RPC URL: http://127.0.0.1:37868/ext/bc/2ALrMJ74YHrq6gRXzZkmYaAx6tJhshybkWr8m71r56E7Cv25Qf/rpc
ChainID: 676767
Symbol: TEST
````

:::note
If you followed the exact steps in this tutorial, you would see that your balance on metamask is zero. That is because we have only airdropped to the default account which is `0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC`. Therefore, your account on metamask has zero tokens and cannot send any transactions. So, to interact with the chain we have to use the address that is airdropped. If your balance is zero, refer to [Access Funded Accounts](####Access-Founded-Accounts).
:::


#### Access Funded Accounts
Make sure that metamask is configured to use your subnet. 
> If you have customized the subnet and have tokens on your account you can skip the "Import the airdropped account" part

* Import the airdropped account
  1. Open your metamask extension
  2. Click on the account image
  3. Click "Import Account"
  4. Select type: Private Key
  5. Enter private key of the default airdrop account, which is `56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027`
  6. Click "Import"


### Using Hardhat
To interact with the subnet using Hardhat, refer to [Using Hardhat with the Avalanche C-Chain](https://docs.avax.network/dapps/smart-contracts/using-hardhat-with-the-avalanche-c-chain). It is very similar to interacting with C-Chain. You only have change `hardhat.config.ts` file. Inside that file, find the exported js object and inside of it find `networks`. Add a new network which will be your subnet.
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

Now you can run any commands ran in the tutorial with `--network subnet` parameter
Example command: 
```
yarn deploy --network subnet
```

//TODO: 
add explanations for 
avalanche network stop and avalanche network start

## Customize the Subnet
  * `ChainId`: You want your `ChainId` parameter to be unique. To make sure that your subnet is secure against replay attacks. To see registered `ChainIds` you can check [chainlist.org](https://chainlist.org/). At the top right of the site make sure to turn on the button to include Testnets.
  * `Gas Parameters`: Ava Labs recommends the low-low option and C-Chain currently uses this option. But, if you know what you are doing you are free to customize. Note that higher disk usage has some trade offs, it would require more processing power and cause it to be more expensive to maintain.
  * `Airdrop Address`: You would not like to use the default address in production, that is recieving the 1 million tokens. Because it is a compromised wallet, which means that it's private key is well known by others. If you add a custom address to recieve airdrop. Avalanche-cli will ask you to give amount in AVAX, in that case do not enter the value thinking as in ethers but in gwei to correctly airdrop the amount you want. Example: To airdrop 1 whole token, as in one ether, you would enter the value 1000000000.
  * `Precompiles`: You can learn what precompiles are by refering to [this](https://docs.avax.network/subnets/customize-a-subnet#precompiles).


## Troubleshoot Common Issues

### Step 1: Install avalanche-cli
* `avalanche`, `avalanche subnet`
  * `"command not found: avalanche"` 
  It means that `avalanche` command is not added to the PATH environment variable. It could be caused by following reasons; 
    * You have added the wrong `bin` directory to your environment variables. 
      * Make sure to find where `bin` directory is and run `export PATH=$PWD:$PATH` inside the `bin` directory.
    * You have added it to PATH environment variable temporarily and restarted your terminal.
      * You can either add the `bin` directory to the PATH environment variable again by running `export PATH=$PWD:$PATH` inside the downloaded `bin` directory or you can refer to [Add Avalanche Command Permanently](###Add-Avalanche-Command-Permanently)

### Step 2: Create the Subnet
* `avalanche subnet create <subnetName>`

  * `"Error: Configuration already exists..."`
It means that you have already created a subnet with the same name. To check if that is the case, you can run `avalanche subnet list` which would list the subnets you have. If you have a subnet with the same name, you can try to create with a different name, delete the existing subnet by running `avalanche subnet delete <subnetName>` or overwrite the existing one by running `avalanche subnet create <subnetName> --force`


### Step 3: Deploy the subnet
* `avalanche subnet deploy <subnetName> -l`

  * `"Error: failed to query network health: ..."`
You can check to logs which are located at `$HOME/.avalanche-cli/logs` or try to run the command once more.

  * `"Error: failed to start network : ..."`
It means that you have deployed a subnet before and trying to deploy another one without stopping the initial one. To solve this issue, run:
`avalanche network stop` and try to deploy your subnet again.


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