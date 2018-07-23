bchpusher
=========
Aims to be a comprehensive solution for:

1) broadcasting custom OP_RETURN pushdata onto the bitcoin cash blockchain

2) Easy interface with popular industry standards and protocols

current features:
-----------------

* Broadcasts custom OP_RETURN pushdata onto the bitcoin cash blockchain
* Interfaces easily with memo.cash
* Interfaces with blockpress.com (only a few functions at present)

aims
----

Lower barriers to entry for new developers interested in bitcoin cash e.g:

* Creating apps / tokens
* Interacting with existing apps / tokens
* Powerful searches / queries of block-chain metadata with an intuitive API.

example useage
--------------
>>> import bitcash
>>> my_key_main = bitcash.PrivateKey("Kymh9idW5bdM7n7dHvL1DPW6od9J8tFR7kgHMpGLhUcxU8Q1UGQY")
>>> bitcash.bitpusher.bchpush(my_key_main, [('6d01', 'hex'), ('new_name', 'utf-8')])

as per memo.cash protocol @ https://memo.cash/protocol this results in a "Set name" action to "new_name"

raw OP_RETURN will be:

    0e 6a 02 6d01 08 6e65775f6e616d65

Currently (this module) only allows up to 220 bytes maximum - as multiple OP_RETURNS in one transaction are considered non-standard.

TODO
====

bchpusher
---------

Add easy interface with popular industry standards and protocols such as:

* Tokenisation standards: e.g. SLP (Simple Ledger Protocol - see https://docs.google.com/document/d/1GcDGiVUEa87SIEjrvM9QcCINfoBw-R7EPWzNVR4M8EI)

* Standard registration of Token ID / Issuer ID

* Broadcasting the SHA256 hash of important data / contracts onto the blockchain with your preference of intellectual property archive / industry standard protocol

* Support for a few special cases such as _memo.cash_ and _blockpress.org_ to allow easy access for beginner python developers to gain experience playing around with OP_RETURN-based apps.

bitpuller
---------
* easy-to-use bitDB query API for python synergising well with bitpusher
* open source, google-like, block-chain search engine
* complementary counterpart to bitpusher

Manual method (see below)
-------------------------

1) Import private key with bitcash

2) Update unspent transactions

3) Create lst_of_pushdata

4) Output OP_RETURN pushdata as bytes and store in variable

5) Create rawtx ready for broadcast (fee = 1 sat/byte; sending 0.0001 BCH back to own address)

6) Broadcast rawtx

>>> my_key = bitcash.PrivateKey('WIF Compressed (base58) here')
>>> my_key.get_unspents() #necessary step --> updates my_key.unspents object variable
>>> lst_of_pushdata =  [('6d01', 'hex'), ('bitPUSHER', 'utf-8')]
>>> pushdata = bitcash.bchpusher.create_pushdata(lst_of_pushdata)
>>> rawtx = my_key.create_transaction([(my_key.address, 0.0001, 'bch')], fee=1, message=pushdata, bytes=True)
>>> bitcash.network.services.NetworkAPI.broadcast_tx(rawtx)

look at block explorer or wallet to see new transaction!