# tendereum

[![CircleCI](https://circleci.com/gh/cosmos/tendereum/tree/master.svg?style=svg)](https://circleci.com/gh/adrianbrink/tendereum/tree/master)

## Overview

### Goals
The goal of Tendereum is to be an implementation of Ethereum that uses Tendermint Core like it is
meant to be used. Tendereum only handles the application logic. It does not contain any code for
mining, transaction pools, consensus or networking. It only contains code to implement EVM state
transitions. This means that a user can submit an Ethereum transaction and have the EVM execute it.
On top of this implementation there is an RPC server, which implements all Web3 endpoints to
provide full API compatibility with existing tooling such as Truffle. Tendereum also aims to
provide a light client which exposes all Web3 endpoints. The light client sits beneath the RPC
server and facilitates secure communication with the network.


### Purpose
The purpose of Tendereum is initially a learning exercise. It forces me to fully understand the
Ethereum application structure and figure out how to map that neatly onto Tendermint Core.
Furthermore it is a testbed for the cosmos-sdk and how to use the sdk in order to implement an
already existing system. Moreover Tendereum provides a testbed for adding staking to existing
systems. Here we can try different approaches such as staking contracts or native staking through
the SDK. Furthermore, it should provide a way for users to submit IBC transactions. Implementing
staking and ibc inside of smart contracts has the advantage that every existing web3 client would
work, since it is just a normal Ethereum transaction. 

### Design

```
                =============================================
============    =  ===============         ===============  =       
=          =    =  = RPC Server  =         = Tendereum   =  =
=  Web3    <---------->          =         = Application =  = app package
=  RPC     =    =  = - Web3 Imp. =         = - EVM       =  =
=  Client  =    =  =             =         = - State     =  =
= console  =    =  = rpc package =         = - UX        =  =
= package  =    =  =             =         =             =  =
============    =  =             =         =             =  =
                =  =             =         =             =  =
                =  ===============         ===============  =
                =             |                   ^         =
                ==============|===================|==========
                              | Txs               | Txs
                ==============|===================|==========
                =             |    =   full       |         =
                =   light     |    =   tendereum  |         =
                =   tenderum  |    =       ===============  =                                             
                =             |    =       = ABCI Server =  = 
                =             |    =       ===============  =
cmd package     =             |    =       ^      ^      ^  =
                =             |    =       |      |      |  =
                =             |    =  ABCI |      |      |  =
                =             |    =       |      |      |  =
                =             v    =       v      v      v  =
                = ========================================= =  
                = =          Tendermint Core              = =
                = ========================================= =  
                =                   ^                       =
                ====================|========================  
                                    |
                                    |
                                    v
                                Consensus
```

### CLI

### RPC Server
The RPC server does not talk to the application directly. It only connects to Tendermint Core.
Even when it runs against a local Tendermint Core node it does full light-client verification.

### Console
The console sits on top of the RPC server and is just another client.

### App package
* maintains two separate states
  * EVM state
  * Cosmos state
    * IBC
    * Staking

### Staking
* currently gas gets credited to the coinbase
* block rewards should be distributed equally across all validators

### IBC

### Web3
* extend the Web3 api in a backwards compatible manner to include staking and IBC
transactions


