# Setting up a Test-Net Node

### nela installation script instructions

This script is installed on ubuntu 20.04.

Recommended configuration:

| Environment    | sector-size | min-requirement                                              | recommended-requirement                                      |
| -------------- | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Dev/TeamServer | 2K          | CPU:4C,<br/>MEM:4GB,<br/>SSD:800GB                           | CPU:4C,<br/>MEM:8GB,<br/>SSD:1TB                             |
| Dev/TeamServer | 8M          | CPU:4C,<br/>MEM:4GB,<br/>SSD:800GB                           | CPU:4C,<br/>MEM:8G,<br/>SSD:1T                               |
| Test           | 512M        | CPU:4C,<br/>MEM:6GB,<br/>SSD:2TB                             | CPU:8C,<br/>MEM:16G,<br/>SSD:4T                              |
| Product        | 32G         | CPU:64C,<br/>MEM:256GB,<br/>SSD:4TB or more,<br/>GPU:2080Ti × 1 | CPU:128C,<br/>MEM:512G,<br/>SSD:10TB or more,<br/>GPU:3090 × 1 |
| Product        | 64G         | CPU:128C,<br/>MEM:512GB,<br/>SSD:10TB or more,<br/>GPU:2080Ti × 1 | CPU:256C,<br/>MEM:1T,<br/>SSD:10TB or more,<br/>GPU:3090 × 1 |



```
#New system implementation update
sudo apt-get update && apt-get upgrade -y

#If the system itself does not have git service, you need to install it in advance, if you have, please ignore
git --help
sudo apt-get -y install

#Do the following configuration and then perform a git pull or git push without entering a password prompt
git config --global credential.helper store

#Clone deployment project to local
git clone https://github.com/edshareview/node-bash.git
```

Script file description.

```
.
├── add_node_common.sh          #Join the node's public script
├── add_node_config.properties  #Join the node's configuration file
├── add_node_sentry.sh          #Sentinel Installation Master Program
├── add_node_validator.sh       #Verifier installs the main program
├── common.sh                   #nela installs the public scripts corresponding to the main program
├── config.properties           #nela installs the configuration file corresponding to the main program
├── js                          #Get related operations via js
│   ├── keystore.js             #Generate UTC file by private key
│   ├── package.json 
│   ├── package-lock.json
│   └── stake_pubkey.js         #Obtain public key information
├── nela-install.sh             #nela installation main program
└── README.md                   #Installation instructions file
```

#### How to use nela single node installation (without Sentinel)：

Corresponding profiles：config.properties

Script Description：

```
#########################################################################################
        Nela Installing the deployment script 
         Please follow the order of execution!
Usage: bash nela-install.sh [Number]
—————————————— Configure replacement source ————————————————————————————
        0.     If you are in China you need to configure a replacement source
        1.     Update source
—————————————— Installation Wizard —————————————————————————————————————
        2.     Install related software and dependency packages
        3.     Installing Rustup
        4.     Installing Golang 1.17
        5.     Installing RabbitMQ
        6.     Install binary released files
        7.     Porep-proof file download
        8.     download the genesis file and produce the genesis file
        9.     Set up the node
        10.    start up the ETH chain
—————————————— Install the main program ———————————————————————————————
        11.    Deploy Layer1 smart contracts on Private ETH Network
        12.    Pledged Tokens Become Validators
        13.    The configuration of validator node
        14.    start the heimdall service(heimdall/heimdall_rest_server/heimdall_bridge)
        15.    start the nela service
        16.    Deploy Nela POS Smart Contracts
        17.    Deploy the Storage and Staking Smart Contracts
        18.    Register the Minner information
—————————————— Deploy the DAPP contracts ———————————————————————————————
        19.    Deploy DAPP Smart Contracts
—————————— One-Click Configuration and Installation of Nela ————————————
        20.    One-click configuration and installation
—————————————— Other Options ———————————————————————————————————————————
        21.    Exit 

Please enter your choice【0-21】:
```

The script supports one-click deployment and also supports step-by-step execution in the following way：

1、The step-by-step execution is as follows：

```
cd node-bash
bash nela-install.sh 0
...
bash nela-install.sh 19
```

2、One-click installation and deployment

```
cd node-bash
bash nela-install.sh 20

#If you make an error at one of the steps in the one-click installation, you can continue from the error step by using the sequential installation method
```

Note: There are interdependencies if there is an error related to the deployment contract.

- Steps 0-10, if the execution is successful, there is no need to repeat the execution, after step 11 there is an error in the execution, you can continue from step 11 to continue the execution；
- Steps 14 and 15 involve services, which cannot be repeated, and you need to refer to the last relevant help command to stop the service operation；



Final deployed contract address information：

```
#Bridging Related Contracts
code/nela-bridge-contracts/contractAddresses.json
#dapp Related Contracts
code/dap-contracts/contractAddresses.json
#Storage Related Contracts
code/genesis-contracts/contractAddresses.json
```

You can use the following command to check whether the checkpoint function is normal, if you can query the data, it means normal：

```
curl http://localhost:1317/bor/span/1
```

```
root@ubuntu:~# curl http://localhost:1317/bor/span/1
{"height":"1453","result":{"span_id":1,"start_block":256,"end_block":6655,"validator_set":{"validators":[{"ID":1,"startEpoch":0,"endEpoch":0,"nonce":1,"power":10000,"pubKey":"0x04914873c8d5935837ade39cbdabd6efb3d3d4064c5918da11e555bba0ab2c58fee95974a3222830cf73d257bdc18cfcd01765482108a48e68bc0b657618acb40e","signer":"0x9fb29aac15b9a4b7f17c3385939b007540f4d791","last_updated":"","storagePower":0,"storageSize":0,"storagePromise":1000000000,"storageNonce":0,"jailed":false,"accum":0}],"proposer":{"ID":1,"startEpoch":0,"endEpoch":0,"nonce":1,"power":10000,"pubKey":"0x04914873c8d5935837ade39cbdabd6efb3d3d4064c5918da11e555bba0ab2c58fee95974a3222830cf73d257bdc18cfcd01765482108a48e68bc0b657618acb40e","signer":"0x9fb29aac15b9a4b7f17c3385939b007540f4d791","last_updated":"","storagePower":0,"storageSize":0,"storagePromise":1000000000,"storageNonce":0,"jailed":false,"accum":0}},"selected_producers":[{"ID":1,"startEpoch":0,"endEpoch":0,"nonce":1,"power":10000,"pubKey":"0x04914873c8d5935837ade39cbdabd6efb3d3d4064c5918da11e555bba0ab2c58fee95974a3222830cf73d257bdc18cfcd01765482108a48e68bc0b657618acb40e","signer":"0x9fb29aac15b9a4b7f17c3385939b007540f4d791","last_updated":"","storagePower":0,"storageSize":0,"storagePromise":1000000000,"storageNonce":0,"jailed":false,"accum":0}],"bor_chain_id":"15678"}}
```



#### How to use nela Join Node (with Sentinel Mode)：

Corresponding profiles：add_node_config.properties

##### 一、The sentry node operates using the following script：

Script Description：

```
########################################################################################
         Nela Installing the deployment script 
         Please follow the order of execution!
Usage: bash add_node_sentry.sh [Number]
—————————————— Configure replacement source ———————————————————————————————————————————
        0.     If you are in China you need to configure a replacement source
        1.     Update source
—————————————— Installation Wizard ————————————————————————————————————————————————————
        2.     Install related software and dependency packages
        3.     Installing Rustup
        4.     Installing Golang 1.17
—————————————— Install/compile the main program ———————————————————————————————————————
        5.     Compile the heimdall binary
        6.     Compile the nera binary
        7.     Set up the node
        8.     Setting up the heimdall configuration file
        9.     Configuring Nela Services
        10.    start the heimdall service(heimdall/heimdall_rest_server)
—————————————— The heimdall must be fully synchronized before you can continue with the following operations ———————
        11.    Check if heimdall synchronization is complete(Repeatable execution to view synchronization status)
        12.    start the nela service
—————————— One-Click Configuration and Installation of Nela ———————————————————————————
        13.   One-click configuration and installation
—————————————— Other Options ——————————————————————————————————————————————————————————
        14.    Exit 

Please enter your choice【0-14】: 
```

The script supports one-click deployment and also supports step-by-step execution in the following manner:

1、Step-by-step installation：

```
cd node-bash
bash add_node_sentry.sh 0
...
bash add_node_sentry.sh 12
```

2、One Click Installation：

```
bash add_node_sentry.sh 13
```

You need to wait for heimdall synchronization to complete before starting nela service, the synchronization process is more time-consuming.

```
#Check if heimdall synchronization is complete
bash add_node_sentry.sh 11

bash add_node_sentry.sh 12
```

##### 二、The verification node is operated using the following script：

Script Description：

```
########################################################################################
         Nela Installing the deployment script 
         Please follow the order of execution!
Usage: bash add_node_validator.sh [Number]
—————————————— Configure replacement source ———————————————————————————————————————————
        0.     If you are in China you need to configure a replacement source
        1.     Update source
—————————————— Installation Wizard ————————————————————————————————————————————————————
        2.     Install related software and dependency packages
        3.     Installing Rustup
        4.     Installing Golang 1.17
        5.     Installing RabbitMQ
—————————————— Install/compile the main program ———————————————————————————————————————
        6.     Compile the heimdall binary
        7.     Compile the nera binary
        8.     Porep-proof file download
        9.     Set up the node
        10.    Setting up the heimdall configuration file
        11.    Configuring Nela Services(bootnode/keystore/passwd)
        12.    start the heimdall service(heimdall/heimdall_rest_server/heimdall_bridge)
—————————————— The heimdall must be fully synchronized before you can continue with the following operations ———————
        13.    Check if heimdall synchronization is complete
        14.    start the nela service
—————————— One-Click Configuration and Installation of Nela ———————————————————————————
        15.    One-click configuration and installation
—————————————— Other Options ——————————————————————————————————————————————————————————
        16.    Register the Minner information
        17.    Exit 

Please enter your choice【0-17】:
```

The script supports one-click deployment and also supports step-by-step execution in the following way：

1、Step-by-step installation：

```
cd node-bash
bash add_node_validator.sh 0
...
bash add_node_validator.sh 14
```

2、One Click Installation：

```
bash add_node_sentry.sh 15
```

Check if the heimdall synchronization is complete, the "Synchronization completed!" message will be given when the synchronization is complete.

```
bash add_node_sentry.sh 13
```

Need to wait for heimdall sync to complete before starting nela service.

```
bash add_node_sentry.sh 14
```

After waiting for the heimdall synchronization to complete, start nela and then continue with the following steps.

Pledges become validators

```
Please make sure that the account you want to become a verifier has enough ETH (at least 1 ETH) and Nela coins (more than 2000 Nela coins).

You need to make sure that there is at least 1 ETH and Nela tokens greater than 2000 on the main chain of Ether, if you don't have Nela tokens, please transfer the corresponding number of tokens to the contract.

ETH corresponds to Nela token contract address: 0x5aF1473c5984b6469b0671A71048296Ff78f6463

Then through "http://test.wallet.nela.com/staking" Page for pledge operation (pledge Nela coin operation on ETH chain)
```

Registered Miner Address Information

```
bash add_node_sentry.sh 16
```

Waiting to become a validator。



#### Related help commands

```
#Restart related services
sudo service nela restart
sudo service heimdalld restart
sudo service heimdalld-rest-server restart
sudo service heimdalld-bridge restart

#View Log
journalctl -u heimdalld.service -f
journalctl -u heimdalld-rest-server.service -f
journalctl -u heimdalld-bridge.service -f
journalctl -u nela.service -f

#Check heimdall status
curl localhost:26657/status

#View Process
ps -ef|grep -v grep |grep -E "heimdall|bridge"
ps -ef|grep -v grep |grep nela

#Enter the nela chain
nela attach http://localhost:8545

#Enter the ETH chain
nela attach http://localhost:9545
```
