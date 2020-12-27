# quorumDeploy

A repository to quickly configure and deploy quorum networks (with the Istanbul consensus algorithm)

Essentially the same instructions as in <https://docs.goquorum.consensys.net/en/stable/Tutorials/Create-IBFT-Network/>

# Pre-requisites

Golang should be installed in the system, and ```$GOPATH/bin``` should be in the ```$PATH```

Then install goQuorum build it:

```
go get github.com/ConsenSys/quorum
cd $GOPATH/src/github.com/ConsenSys/quorum
make
foreach i in build/bin/*; do ln -s $(pwd)/$i $GOPATH/bin; done
go get github.com/ConsenSys/istanbul-tools
cd $GOPATH/src/github.com/ConsenSys/istanbul-tools
make
foreach i in build/bin/*; do ln -s $(pwd)/$i $GOPATH/bin; done
cd
```

# Generating and initializing the network

First, create a directory where the nodes are going to be generated, e.g. in ```~/Ethereum```

```
mkdir -p ~/Ethereum
cd !$
```

Then, clone this repository to get the script files

```
git clone https://github.com/juliofaura/quorumDeploy
```

Then, run the ```netgenerate``` script to generate the node directories. The command accepts one parameter, which is the number of nodes that the network is going to have (all validators in the IBFT algorithm). For example, for a network with 4 nodes you can use:

```
quorumDeploy/netgenerate 4
```

After the directories have been generated then the nodes can be initialized:

```
quorumDeploy/netinit
```

# Launching the network

To launch all the nodes in the network, you can simply call the ```netlaunch``` script:

```
quorumDeploy/netlaunch
```

You can check that all the geth nodes are up and running calling:

```
pgrep geth
```
, which should show a number of nodes running equals to the number of nodes used in the ```netinit``` command

And now you can attach a geth console to start interacting with the network:

```
quorumDeploy/attach
```

(You can check that the network is mining blocks for example using the ```eth.BlockNumber``` command, which should show a growing number of blocks as they get mined)

```
Welcome to the Geth JavaScript console!

instance: Geth/v1.9.7-stable-7b726385(quorum-v20.10.0)/darwin-amd64/go1.15.6
coinbase: 0xc29e6a16fc78ac641cb3ed8fae5ad4cc31503674
at block: 447 (Sun, 27 Dec 2020 20:45:09 CET)
 datadir: /Users/julio/Ethereum/4-node-net/Node-0/data
 modules: admin:1.0 debug:1.0 eth:1.0 istanbul:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0

> eth.blockNumber
7
```

# Stopping the network

You can stop the network with a simple ```pkill``` command:

```
pkill geth
```

The network can be restarted by using again the ```netlaunch``` command. It will pick it up from the last block it was mined

```
quorumDeploy/netlaunch
```


# Resetting the network

You can reset the network with the ```netreset``` script

```
quorumDeploy/netreset
```

Now you can re-initilize the network and launch it again as desired

```
quorumDeploy/netinit
quorumDeploy/netlaunch
```
