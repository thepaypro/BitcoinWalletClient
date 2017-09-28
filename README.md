# PayPro Bitcore wallet CLI implementation

Node based project with a cli integration for the bitcore wallet client service.

## Requirements

* [Docker engine](https://www.docker.com/)
* [Docker compose](https://docs.docker.com/compose/)

## Usage

Simply run `docker-compose up` in the root directory of the project to install the required dependencies.
Once it has finished you can run any BWS CLI command through the docker API via `docker-compose exec node -c "[CLI command]"`.

For more info about the CLI commands available check the quick guide section and/or the Bitcore wallet documentation.

## bitcore-wallet

[![NPM Package](https://img.shields.io/npm/v/bitcore-wallet.svg?style=flat-square)](https://www.npmjs.org/package/bitcore-wallet)

A simple Command Line Interface Wallet using [Bitcore Wallet Service](https://github.com/bitpay/bitcore-wallet-service) and its *official* client lib, [bitcore-wallet-client](https://github.com/bitpay/bitcore-wallet-client)


## Quick Guide

``` shell
# Use -h or BWS_HOST to setup the BWS URL (defaults to localhost:3001)
# 
# Start a local BWS instance be doing:
# git clone https://github.com/bitpay/bitcore-wallet-service.git bws
# cd bws; npm install; npm start

cd bin
 
# Create a 2-of-2 wallet (~/.wallet.dat is the default filename where the wallet critical data will be stored)
#
# TIP: add -t for testnet, and -p to encrypt the credentials file
wallet create 'my wallet' 2-2 
  * Secret to share:
    JevjEwaaxW6gdAZjqgWcimL525DR8zQsAXf4cscWDa8u1qKTN5eFGSFssuSvT1WySu4YYLYMUPT

# Check the status of your wallet 
wallet status
 
  * Wallet my wallet [livenet]: 2-of-2 pending
    Missing copayers: 1

# Use -f or WALLET_FILE to setup the wallet data file
 
# Join the wallet as another copayer (add -p to encrypt credentials file)
wallet -f pete.dat join JevjEwaaxW6gdAZjqgWcimL525DR8zQsAXf4cscWDa8u1qKTN5eFGSFssuSvT1WySu4YYLYMUPT
   
export WALLET_FILE=pete.dat
wallet status

# Generate addresses to receive money
wallet address
  * New Address 3xxxxxx

# Check your balance
wallet balance
   
# Spend coins. Amount can be specified in btc, bit or sat (default)
wallet send 1xxxxx 1000bit "1000 bits to mother"
  * Tx created: ID 01425517364314b9ac6017-e97d-46d5-a12a-9d4e5550abef [pending]
    RequiredSignatures: 2

# You can use 1000bit or 0.0001btc or 100000sat. (Set BIT_UNIT to btc/sat/bit to select output unit).

# List pending TX Proposals
wallet txproposals
  * TX Proposals:
    abef ["1000 bits to mother" by pete] 1,000 bit => 1xxxxx
      Missing signatures: 2
   
# Sign or reject TXs from other copayers
wallet -f pete.dat reject <id>
wallet -f pete.dat sign <id>

# List transaction history
wallet history
  a few minutes ago: => sent 1,000 bit ["1000 bits to mother" by pete] (1 confirmations)
  a day ago: <= received 1,400 bit (48 confirmations)
  a day ago: <= received 300 bit (62 confirmations)
   
# List all commands:
wallet --help
 
    
  ```
  
  
## Password protection 

It is possible (and recommeded) to encrypt the wallet's credentials (.dat file). this is done 
be adding the `-p` parameter to `join` or `create` or `genkey`. The password will be asked 
interactively. Following commands that use the crendetials will require the password to work.

Password-based key derication function 2 (http://en.wikipedia.org/wiki/PBKDF2) is used to derive
the key to encrypt the data. AES is used to do the actual encryption, using the implementation
of SJCL (https://bitwiseshiftleft.github.io/sjcl/).



