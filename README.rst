Status Update
==============
1) Please be advised that this project is in the very early stages. (not even at v0.1 yet!)
2) Bitcash https://github.com/sporestack/bitcash is now satisfactory for the backend to bchpusher to move forward but until the next release of bitcash, you will need to git clone / copy and paste the latest copy of bitcash into your Lib/site-packages folder (or copy it overtop of the old one - not the whole repo just the bitcash folder!). New release coming soon though. 
3) Bchpusher now works for the main basic functions provided you have performed step 2 (get latest bitcash from repo). To get bchpusher working (in its current state) you will need to clone / copy and paste the bchpusher folder directly into your Lib/site-packages folder. It will probably be a little while (i.e. a few weeks or more) before I put together a pypi // pip3 installer for ease of access.
4) In the meantime. If you are using this project. Feel free to report any issues / bugs / suggestions / questions / pull requests etc. This is all valuable feedback. I will generally reply within about 6 hours.

------------------------------------------------------------------------------------------------------------------------------

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

Lower barriers to entry for new developers and pythonistas with a mild curiosity about crypto:

* Creating apps / tokens
* Interacting with existing apps / tokens
* Powerful searches / queries of block-chain metadata with an intuitive API for data analysis etc.

bchpush method example useage
-----------------------------
>>> import bitcash, bchpusher
>>> my_key = bitcash.PrivateKey("Kymh9idW5bdM7n7dHvL1DPW6od9J8tFR7kgHMpGLhUcxU8Q1UGQY")
>>> bchpusher.bchpush(my_key, [('6d01', 'hex'), ('new_name', 'utf-8')])

as per memo.cash protocol @ https://memo.cash/protocol performs "Set name" action to "new_name"

raw OP_RETURN 02 6d01 08 6e65775f6e616d65 

Currently only allows up to 220 bytes maximum - as multiple OP_RETURNS in one transaction are considered non-standard.

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
>>> lst_of_pushdata =  [('6d01', 'hex'), ('bchpusher', 'utf-8')]
>>> pushdata = bitcash.bchpusher.create_pushdata(lst_of_pushdata) # returns raw byte array
>>> rawtx = my_key.create_transaction([(my_key.address, 0.0001, 'bch')], fee=1, message=pushdata, custom_pushdata=True)
>>> bitcash.network.services.NetworkAPI.broadcast_tx(rawtx)

look at block explorer or wallet to see new transaction!

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
* easy-to-use bitDB query API for python synergising well with bchpusher
* open source, google-like, block-chain search engine
* complementary counterpart to bchpusher
