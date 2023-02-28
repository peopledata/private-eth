# private-eth
deploy a private ethereum.


## 1. Why should you create you own private blockchain ?
There are several reasons to that:
You’ll have a better understanding of the Ethereum ecosystem.
It may be useful to have you own infrastructure and to NOT depend on others.
To make private tests visible only by you (and not by others on testnet)

## 2. So, how can i create my own blockcahain ?
The first step is to download and install an Ethereum client.

```bash
$ sudo add-apt-repository -y ppa:ethereum/ethereum
$ sudo apt-get update
$ sudo apt-get install ethereum solc
```

Then, make a directory of a private blackchain.

```bash
$ mkdir private-eth-node<1>
$ cd private-node<1>
```

## 3. Create a genesis block
Once it’s done, you need to create the “genesis” block, this is the first block of a blockchain (block number 0) and specify his properties.

```bash
$ vi genesis.json
```

The `genesis.json` example is:
```json
{
  "config": {
    "chainId": 1202,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "difficulty": "200000000",
  "gasLimit": "8000000",
  "alloc": { }
}
```
There are several fields you need to know:
- `gasLimit`, this is the block gas limit. (On Ethereum this is set to 30 Millions gas). This means that in each block the sum of all consumed gas mustn't exceed 30 Millions.
- `Difficulty` , For validating a block each node need to calculate a hash, this hash need to fit certain “requirements”. A difficulty of 0x400 (1024 in decimal) means that, each time a node calculates a hash, he has 1 in 1024 chances to fit the requirements and to mine the block. The higher the difficulty is, the harder is to mine a new block. (on Ethereum it was 5–10 TRILLION before the merge).
`alloc` , you can choose the initial state by allocating ETH to certain addresses or by per-deploying smart contracts. 
`config` , you can configure other aspects of the blockchain. (like the chainId, the block when the blockchain will hard fork following the EIP150, 155 or 158 standard and others)


Then, create the genesis block.This will write the initial state of the blockchain in the directory of your choice.
```bash
$ geth --datadir data1 init genesis.json
```
The output should be like this:
```bash
INFO [02-27|18:27:48.674] Maximum peer count                       ETH=50 LES=0 total=50
INFO [02-27|18:27:48.676] Smartcard socket not found, disabling    err="stat /run/pcscd/pcscd.comm: no such file or directory"
INFO [02-27|18:27:48.679] Set global gas cap                       cap=50,000,000
INFO [02-27|18:27:48.680] Using leveldb as the backing database 
INFO [02-27|18:27:48.680] Allocated cache and file handles         database=/home/ubuntu/peopledata-private-eth/peopledata-private-eth/geth/chaindata cache=16.00MiB handles=16
INFO [02-27|18:27:48.686] Using LevelDB as the backing database 
INFO [02-27|18:27:48.703] Opened ancient database                  database=/home/ubuntu/peopledata-private-eth/peopledata-private-eth/geth/chaindata/ancient/chain readonly=false
INFO [02-27|18:27:48.703] Writing custom genesis block 
INFO [02-27|18:27:48.704] Persisted trie from memory database      nodes=3 size=407.00B time="29.382µs" gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [02-27|18:27:48.704] Successfully wrote genesis state         database=chaindata hash=e0dc41..15a1cb
INFO [02-27|18:27:48.704] Using leveldb as the backing database 
INFO [02-27|18:27:48.704] Allocated cache and file handles         database=/home/ubuntu/peopledata-private-eth/peopledata-private-eth/geth/lightchaindata cache=16.00MiB handles=16
INFO [02-27|18:27:48.716] Using LevelDB as the backing database 
INFO [02-27|18:27:48.733] Opened ancient database                  database=/home/ubuntu/peopledata-private-eth/peopledata-private-eth/geth/lightchaindata/ancient/chain readonly=false
INFO [02-27|18:27:48.733] Writing custom genesis block 
INFO [02-27|18:27:48.734] Persisted trie from memory database      nodes=3 size=407.00B time="28.654µs" gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [02-27|18:27:48.734] Successfully wrote genesis state         database=lightchaindata hash=e0dc41..15a1cb
```



## 4. Accessing the Blockchain on the network

```bash
geth --identity "peopledata-private-eth-<node #>"  \ 
--networkid <your assigment> --datadir <data1>  \ 
--ipcdisable --syncmode full --http --http.addr 0.0.0.0 \
--http.api admin,eth,miner,net,txpool,personal,web3 --allow-insecure-unlock \
--http.corsdomain "*" --http.vhosts "*"  console 
```


## 5. Adding your blockchain to metamask
To use the blockchain on metamask, you follow the standard way for adding a network (like any other).
Go to settings > network > Add network (to add a network).

## 6. Interact with the blockchain on the console
At this point, you can interact with the blockchain you’ve created by using the console in JavaScript.
Here is some commands you can try:
miner.start() // mine new blocks
miner.stop() // stop mining blocks
web3.fromWei(web3.eth.getBalance("0x...")) // Get the balance of an address

admin.nodeInfo() //list some informations about the node
eth.blockNumber() // print the current block number
You can send transactions from your metamask network too, but don’t forget to activate the miner for validating the transaction.

### 
You can find all the “commands” here: https://ethereum.stackexchange.com/questions/28703/full-list-of-geth-terminal-commands






