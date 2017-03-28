# 3.3: Setting Up Your Wallet

> **NOTE:** This is a draft in progress, so that I can get some feedback from early reviewers. It is not yet ready for learning.

You're now ready to start working with Bitcoin. To begin with, you'll need to create an address for receiving funds.

## Create an Address

The first thing you need to do is create an address for receiving payments. This is done with the `bitcoin-cli getnewaddress` command. Remember that if you want more information on this command, you should type `bitcoin-cli help getnewaddress`.
```
$ bitcoin-cli getnewaddress
n4cqjJE6fqcmeWpftygwPoKMMDva6BpyHf
```
Note that this address begins with an "n" (or sometimes an "m"). This signifies that this is a testnet address. 

> **TESTNET vs MAINNET:** The equivalent mainnet address would start with a 1.

Take careful note of the address. You'll need to give it to whomever will be sending you funds.

_What is a Bitcoin address?_ A Bitcoin address is literally where you receive money. It's like an email address, but for funds. However unlike an email address, a Bitcoin address should be considered single use: use it to receive funds just _once_. When you want to receive funds from someone else or at some other time, generate a new address. This is suggested in large part to improve your privacy. The whole blockchain is immutable, which means that explorers can look at long chains of transactions over time, making it possible to statistically determine who you and your contacts are, no matter how careful you are. However, if you keep reusing the same address, then this becomes even easier.

A Bitcoin address is also something else: a public key (or more precisely, the 160-bit hash of a public key). The public key (or address) allows you to receive money, while an associated private key lets you spend that money.

_What is a P2PKH address?_ You may see this sort of address referred to as a Pay to PubKey Hash (P2PKH) address. That's the standard type of Bitcoin address that allows funds to be immediately sent to one person. It's largely used in contrast to more complex payment mechanisms like Pay to Script Hash (P2SH).

_What is a Bitcoin wallet?_ By creating your first Bitcoin address, you've also begun to fill in your Bitcoin wallet. More precisely, you've begun to fill the `wallet.dat` file in your ~/.bitcoin/testnet3 directory. The `wallet.dat` file contains data about preferences and transactions, but more importantly it contains all of the keypairs that you create: both the public key (which is to say the address that you give to people so that you can receive funds) and the private key (which is what you use to spend those coins). For the most part, you won't have to worry about that private key: `bitcoind` will use it when it's needed. However, this makes the `wallet.dat` file extremely important: if you lose it, you lose your private keys, and if you lose your private keys, you lose your funds!

With a single address in hand, you could jump straight to the next section and begin receiving funds. However, before we get there, we're going to talk about a few other wallet commands that you might want to use in the future.

## Optional: Sign a Message

Sometimes you'll need to prove that you control a Bitcoin address (or rather, that you control its private key). This is important because it lets people know that they're sending funds to the right persons. This can be done by creating a signature with the `bitcoin-cli signmessage` command, in the form `bitcoin-cli signmessage [address] [message]`. For example:
```
$ bitcoin-cli signmessage "n4cqjJE6fqcmeWpftygwPoKMMDva6BpyHf" "Hello, World"
H3yMBZaFeSmG2HgnH38dImzZAwAQADcOiMKTC1fryoV6Y93BelqzDMTCqNcFoik86E8qHa6o3FCmTsxWD7Wa5YY=
```
You'll get the signature as a return.

Another person can then use the `bitcoin-cli verifymessage` command to verify the signature. He inputs the address in question, the signature, and the message:
```
$ bitcoin-cli verifymessage "n4cqjJE6fqcmeWpftygwPoKMMDva6BpyHf" "H3yMBZaFeSmG2HgnH38dImzZAwAQADcOiMKTC1fryoV6Y93BelqzDMTCqNcFoik86E8qHa6o3FCmTsxWD7Wa5YY=" "Hello, World"
true
```
If they all match up, then the other person knows that he can safely transfer funds to the person who signed the message by sending to the address.

If some black hat was making up signatures, this would instead produce a negative result:
```
$ bitcoin-cli verifymessage "n4cqjJE6fqcmeWpftygwPoKMMDva6BpyHf" "FAKEBZaFeSmG2HgnH38dImzZAwAQADcOiMKTC1fryoV6Y93BelqzDMTCqNcFoik86E8qHa6o3FCmTsxWD7Wa5YY=" "Hello, World"
false
```

## Optional: Dump Your Wallet

It might seem dangerous having all of your irreplacable private keys in a single file. That's what `bitcoin-cli backupwallet` is for. It lets you make a copy of your wallet.dat:
```
$ bitcoin-cli backupwallet backup.dat
```
You can then recover it with `bitcoin-cli importwallet`.
```
$ bitcoin-cli importwallet backup.dat
```

## Optional: View Your Private Keys

Sometimes, you might want to actually look at the private keys associated with your addresses. Perhaps you want to be able to sign a message or spend a coin from a different machine. Perhaps you just want to back up certain important private keys.

To look at _all_ the keys in your wallet, type `bitcoin-cli dumpwallet mywallet.txt`. This will create a mywallet.txt file in ~/.bitcoin/testnet3 with a long list of private keys, addresses, and other information. Mind you, you'd never want to put this data out in a plain text file on a Bitcoin setup with real funds!

More likely, you just want to look at the private key associated with a specific address. This can be done with the `bitcoin-cli dumpprivkey` command.
```
$ bitcoin-cli dumpprivkey "n4cqjJE6fqcmeWpftygwPoKMMDva6BpyHf"
cW4s4MdW7BkUmqiKgYzSJdmvnzq8QDrf6gszPMC7eLmfcdoRHtHh
```
You can then save that key somewhere safe.

You've been typing that Bitcoin address you generated a _lot_, while you were signing messages and now dumping keys. If you think it's a pain, we agree. It's also prone to errors, a topic that we'll address in the very next section.

## Summary: Setting Up Your Wallet

You need to create an address to receive funds. Your address is stored in a wallet, which you can back up. You can also do lots more with an address, like dumping its private key or using it to sign messages. But really, creating that address is _all_ you need to do in order to receive Bitcoin funds.