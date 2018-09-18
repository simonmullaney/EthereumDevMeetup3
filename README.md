# EthereumDevMeetup3


## Setting up Ethereum private chain using Geth and Mist -Tuesday, September 18, 2018


Mist: It’s a browser for decentralized web apps. It seeks to be the equivalent of Chrome or Firefox, but for Dapps. It’s still insecure and you shouldn’t use it with untrusted dapps as of yet.


Geth: Is the core application on your computer that will connect you to a blockchain. It can also start a new one (in our case we will create a local test blockchain), create contract, mine ether etc.


Ethereum wallet: It’s a version of Mist, but only opens one single dapp, the Ethereum Wallet. Mist and Ethereum Wallet are just UI fronts. And we need a core that will connect us to an Ethereum blockchain(It could be the real Ethereum blockchain, or a test one).




Steps:

1) Download Geth ->  https://geth.ethereum.org/downloads/

2) Download Mist or Ethereum Wallet -> https://github.com/ethereum/mist/releases


In the root folder create a new file, and name it “genesis.json”. Then past the following content in it.



{
  "difficulty" : "0x20000",
  "extraData"  : "",
  "gasLimit"   : "0x8000000",
  "alloc": {},
  "config": {
        "chainId": 16,
        "homesteadBlock": 0,
        "eip155Block": 0,
        "eip158Block": 0
    }
}


The file “genesis.json” is simply the configuration file that geth needs to create a new blockchain.



If you run geth without specifying any parameter, it will try to connect to the mainnet. The mainnet is the main network of Ethereum, the real blockchain of Ethereum.


Then run the following command in the root of your project directory:

`
geth --datadir=./chaindata/ init ./genesis.json
`


We launch geth and specify where our blockchain will be stored, here in the “chaindata” folder (it will be automatically generated), and we initialize it with our “genesis.json” configuration file.



We then start geth using the following command:

`
geth --datadir=./chaindata/ --rpc --ipcpath "~/geth.ipc"
`


Then we start Mist by using:


`/Applications/Mist.app/Contents/MacOS/Ethereum\ Wallet --rpc "/Users/simonmullaney/geth.ipc"`


We then run:


` geth attach ipc:"/Users/simonmullaney/geth.ipc"`



From the geth console you can unlock your account by:

`personal.unlockAccount('account_address', 'password')`


Then we can deploy the first ethereum developers meetup:

1) clone the repo - `git clone https://github.com/simonmullaney/EthereumDevMeetup.git`

2) then cd into it - `cd EthereumDevMeetup`

3) Change truffle.js file to the following:

`
module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // for more about customizing your Truffle configuration!
  networks: {
    development: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "*"
    },
    ourTestNet: {
      host: "127.0.0.1",
      port: 8545,
      network_id: "*"
    }
  }
};
`

4) migrate the contract to the blockchain that geth is running (The miner must be started)


`truffle migrate --network ourTestNet`


then in the truffle console:

`truffle console --network ourTestNet`

in the console we can see the contract by running:

`ipfs`


Get the address by running:

`ipfs.address`

and the abi by runnning:

`JSON.stringify(ipfs.abi)`
