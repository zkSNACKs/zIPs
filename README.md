# Technical Plans

## Abstract

Wasabi Wallet is the state of the art Bitcoin privacy wallet that is based on the [ZeroLink Wallet Fungibility Framework](https://github.com/nopara73/ZeroLink/). While statistical privacy can be achieved today with it, the cost, convenience, intuitiveness and strength of this privacy can be greatly improved. Wasabi also needs to improve its accessibility and its general Bitcoin wallet features. Furtheremore Wasabi should look into ways of extending the scope of its privacy protection to other, not closely Bitcoin related fields, like end to end encrypted messaging. Finally, Wasabi also needs to concentrate on its stability, performance, UX and code quality. This document aims to outline a starting plan to progress towards those objectives.

## Table Of Contents

I. [Introduction](#i-introduction)  
II. [Stability, Performance, UX, Code Quality](#ii-stability-performance-ux-code-quality)  
III. [Education](#iii-education)  
IV. [Bitcoin Privacy Improvements](#iv-bitcoin-privacy-improvements)  
V. [General Wallet Features](#v-general-wallet-features)  
VI. [Accessibility](#vi-accessibility)  
VII. [Extending the Scope of Privacy](#vii-extending-the-scope-of-privacy)  
VIII. [Unique Wallet Features](#viii-unique-wallet-features)  

## I. Introduction

Wasabi's main focuses are Bitcoin and privacy, thus section IV. [Bitcoin Privacy Improvements](#iv-bitcoin-privacy-improvements)  . However a a loss of privacy in fields, those are traditionally considered to be out of the scope of a Bitcoin wallet, like sharing addresses through unsecure chat clients or checking transactions in a block explorer through the clearnet also poses privacy threats, thus Wasabi cannot consider them out of their scope, thus section VII. [Extending the Scope of Privacy](#vii-extending-the-scope-of-privacy).  
In the paper [Anonymity Loves Company: Usability and the Network Effect](https://www.freehaven.net/anonbib/cache/usability:weis2006.pdf) the authors note that: 

> We show that in anonymizing networks, even if you were smart enough and had enough time to use every system perfectly, you would nevertheless be right to choose your system based in part on its usability for other users.

Therefore Wasabi should also pay attention to fields those help to increase the number of Wasabi users, with that bringing greater privacy for everyone, thus sections III. [Education](#iii-education) and VI. [Accessibility](#vi-accessibility).  

Furtheremore Wasabi is a software, therefore it must not neglect general software quality issues, thus section II. [Stability, Performance, UX, Code Quality](#ii-stability-performance-ux-code-quality). The better software Wasabi is, the more users it will retain.

Wasabi is also a Bitcoin wallet, therefore it must also improve general Bitcoin wallet related features, thus section V. [General Wallet Features](#v-general-wallet-features).

And finally, there are development opportunities, where the developers of Wasabi recognize they could esily add some unique wallet features, no other wallets do, like in-wallet multi-wallet support, thus section VIII. [Unique Wallet Features](#viii-unique-wallet-features).

Note that, the developers of Wasabi are currently occupied by section II. [Stability, Performance, UX, Code Quality](#ii-stability-performance-ux-code-quality). This enjoys the highest priority. New issues will constantly come up as new users try to use the software. At this point it is unclear if Wasabi will ever have the resources to tackle other sections in this document.

### Wasabi Wallet Under the Hood

Wasabi is an [open-source](https://github.com/zkSNACKs/WalletWasabi/), desktop Bitcoin wallet, working on Windows, Linux and OSX, written in [.NET Core](https://en.wikipedia.org/wiki/.NET_Core) (C#), which is cross platform and open source .NET. Wasabi uses [NBitcoin](https://github.com/MetacoSA/NBitcoin/) as its Bitcoin library, to which Wasabi developers are frequent contributors: [@lontivero](https://github.com/lontivero), [@nopara73](https://github.com/nopara73). Wasabi uses [Avalonia](https://github.com/AvaloniaUI/Avalonia/) library as its UI framework, where Wasabi developer [@danwalmsley](https://github.com/danwalmsley) is a maintainer.  
Wasabi does not support and does not plan to support other currencies in the future.

Now, let's look at what is going on under the hood for Wasabi, what tradeoffs it made, so we can later understand where it can be improved.

After setting up Wasabi and generating a wallet, Wasabi welcomes the user with a load wallet screen. Unlike other wallets, Wasabi have a way to use multiple wallets. This is beneficial for privacy, since many users may prefer or used to achieve coin separation this way. However Wasabi provides a convenient in-wallet coin separation interface, too, more on that later. It is interesting to note, that since coin separation can be achieved in the wallet easily, initially we did not plan for such a wallet management system, our UX design choices naturally lead us down this road.
![](https://i.imgur.com/139bjDH.png)

Wasabi has a status bar that shows meta information about the state of the wallet. To better understand the architecture of the wallet it is helpful to go through them.

The "Tor" label shows the status of the Tor daemon. Tor is an anonymity network, which Wasabi ships with by default and runs on the background. While user can opt to use its own Tor instance if it wishes to. All communication with Wasabi's backend server goes through Tor. Wasabi also utilizes multiple Tor identities, where applicable. For example registering coinjoin inputs and outputs happens through different Tor identities to avoid linking.

![](https://i.imgur.com/054zvbY.png)

Wasabi's backend server is used to facilitate [Chaumian CoinJoin](https://github.com/nopara73/ZeroLink#ii-chaumian-coinjoin) coordination between the mixing participants and to serve Golomb-Rice filters to the clients, similarly to in [BIP157](https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki)-[BIP158](https://github.com/bitcoin/bips/blob/master/bip-0158.mediawiki). More on the difference soon.

![](https://i.imgur.com/lSXrOpJ.png)

After loading the wallet, the user can generate a receive address. Some important design choices were made here. First, Wasabi had to be a Segregated Witness only wallet, so registering unconfirmed coinjoin outputs into new coinjoin round is not to be vulnerable to malleability attacks. However the developers of Wasabi decided to make the wallet native segwit (bech32) only and to not support wrapped segwit. This way the backend server can leverage this and only generate filters regarding bech32 addresses. This makes Wasabi's filter size a few megabytes today, instead of >1GB (insert source here.) On the first glance this may be seen as hazardous to privacy, however Wasabi user utxos, can be identified as Wasabi utxos by the huge coinjoins, that only Wasabi do anyway, so no additional privacy loss happens there. In the future, as more and more wallets adopts bech32, Wasabi developers have to look at how to scale the performance of the wallet. Failing that Wasabi initial sync will slow down.

![](https://i.imgur.com/aZb6TbZ.png)

Wasabi also maintains connection to the Bitcoin P2P network. After Wasabi receives the filters from the backend, it can download the required blocks (there are false positives, too) one block at a peer. This does not currently happen over Tor, since the NBitcoin library we use to do the job does not support Tor yet. Sensitive information leak is already unlikely here, since the only information a node can get is that "one wallet may (false positives) have a transaction in one block someone fetched from me." Wasabi then stores the block in its entirety on disk, so it won't fetch again. A possible privacy leak can happen if wallet is being recovered, thus the work of Tor support here is desired in the future. Furthermore, storing blocks on the disk may take up too much space when the wallet is used extensively. There is room for improvement there, too.

![](https://i.imgur.com/GBP8mSH.png)

Wasabi receives incoming transactions from the nodes it is connected to. It is, while privacy preserving a more insecure way to handle this and should be improved in the future. Generally unconfirmed transactions are considered to be insecure regardless.

![](https://i.imgur.com/mI4lQDt.png)

Unlike in other Bitcoin wallets, to generate a Bitcoin address, a label is not optional, but required. This is, because Wasabi has an intra-wallet blockchain analysis tool built into it, which tries to cluster utxos (Wasabi calls them coins) and based on these clusters the user can make an educated decision which coins to merge and which to not later at spending, if it's necessary at all.

![](https://i.imgur.com/dkdlOZN.png)

Wasabi also has a History tab, like any other Bitcoin wallet.

![](https://i.imgur.com/baDnln2.png)

Unlike other Bitcoin wallets, the user cannot spend from Wasabi without selecting coins. The label field of send tab is also cumpolsory, because of the above mentioned reasons.

![](https://i.imgur.com/wUKTzE6.png)

By clicking on the Max button, one can spend all secelted coins. Spending whole coins is beneficial to privacy. The Bitcoin fee rates are fetched from the backend server, the source of these fees are Bitcoin Core's estimatesmartfee's conservative feerates. Every fee query happens over Tor with new Tor identity. When clicking send, the wallet will broadcast the transaction to the Backend over Tor. This is sub-optimal, but because there's no Tor support for NBitcoin yet, broadcasting transaction P2P over the clearnet would be more dangerous. While, NBitcoin P2P Tor support should be an interest of future work, it is a smarter long-term objective to implement [Dandelion](https://github.com/gfanti/bips/blob/master/bip-dandelion.mediawiki) from a broadcasting point of view when the Bitcoin network adopted it.

![](https://i.imgur.com/gQ1I7DZ.png)

Coins in Wasabi have Privacy and History properties. The anonymity set is just an estimation at the moment, however by examining the mixes and other people's transactions we will be able to show accurate values. The History is a calculated clusters from the labels. Blockchain analysis may identify these clusters. For example, if the user joins together a "foo" labeled coins with a "bar" labeled coin at sending, then the change coin history will show "change of (foo), change of (bar)". From this users are able to make educated decisions which coins not to join together at all cost. Human input is invaluable.

![](https://i.imgur.com/4vUiSWr.png)

Wasabi also has a CoinJoin tab, its use is straightforward. The user can simply queue its coin for coinjoin and wait for others to join the mix.

![](https://i.imgur.com/8wFd88C.png)

If the user does not wish to proceed, it can dequeue its coins.

![](https://i.imgur.com/lxmdxIG.png)

After a mix successfully executed, the resulting CoinJoin transaction will look like the following: https://www.smartbit.com.au/tx/a0855875fd3d19522568ad673e4b52e11691d837021d74eef0d177f9e0950bf2

![](https://i.imgur.com/rcVcPOM.png)

Wasabi also has a Tor website, where one can see real time statistics about the mixes: http://wasabiukrxmkdgve5kynjztuovbg43uxcbcxn6y2okcrsg7gb6jdmbad.onion/

![](https://i.imgur.com/4vE1IiD.png)

The above number means, Wasabi's coinjoins created >102BTC outputs with equal value.

## II. Stability, Performance, UX, Code Quality
https://github.com/zkSNACKs/WalletWasabi/issues/407
https://github.com/zkSNACKs/WalletWasabi/issues/386
https://github.com/zkSNACKs/WalletWasabi/issues/416
https://github.com/zkSNACKs/WalletWasabi/issues/408
https://github.com/zkSNACKs/WalletWasabi/issues/430
https://github.com/zkSNACKs/WalletWasabi/issues/436
https://github.com/zkSNACKs/WalletWasabi/issues/462
https://github.com/zkSNACKs/WalletWasabi/issues/465
https://github.com/zkSNACKs/WalletWasabi/issues/474
https://github.com/zkSNACKs/WalletWasabi/issues/445
https://github.com/zkSNACKs/WalletWasabi/issues/553
https://github.com/zkSNACKs/WalletWasabi/issues/420
https://github.com/zkSNACKs/WalletWasabi/issues/414
https://github.com/zkSNACKs/WalletWasabi/issues/409
https://github.com/zkSNACKs/WalletWasabi/issues/570
https://github.com/zkSNACKs/WalletWasabi/issues/573
https://github.com/zkSNACKs/WalletWasabi/issues/427
https://github.com/zkSNACKs/WalletWasabi/issues/572
https://github.com/zkSNACKs/WalletWasabi/issues/587
https://github.com/zkSNACKs/WalletWasabi/issues/588
https://github.com/zkSNACKs/WalletWasabi/issues/432
https://github.com/zkSNACKs/WalletWasabi/issues/597
https://github.com/zkSNACKs/WalletWasabi/issues/596
https://github.com/zkSNACKs/WalletWasabi/issues/610
https://github.com/zkSNACKs/WalletWasabi/issues/580
https://github.com/zkSNACKs/WalletWasabi/issues/449
https://github.com/zkSNACKs/WalletWasabi/issues/611
https://github.com/zkSNACKs/WalletWasabi/issues/533
https://github.com/zkSNACKs/WalletWasabi/issues/618
https://github.com/zkSNACKs/WalletWasabi/issues/440
https://github.com/zkSNACKs/WalletWasabi/issues/622
https://github.com/zkSNACKs/WalletWasabi/issues/642
https://github.com/zkSNACKs/WalletWasabi/issues/655
https://github.com/zkSNACKs/WalletWasabi/issues/675
https://github.com/zkSNACKs/WalletWasabi/issues/688
https://github.com/zkSNACKs/WalletWasabi/issues/689
https://github.com/zkSNACKs/WalletWasabi/issues/691
https://github.com/zkSNACKs/WalletWasabi/issues/692
https://github.com/zkSNACKs/WalletWasabi/issues/564
https://github.com/zkSNACKs/WalletWasabi/issues/444
https://github.com/zkSNACKs/WalletWasabi/issues/714
https://github.com/zkSNACKs/WalletWasabi/issues/716
https://github.com/zkSNACKs/WalletWasabi/issues/720
https://github.com/zkSNACKs/WalletWasabi/issues/501
https://github.com/zkSNACKs/WalletWasabi/issues/719
https://github.com/zkSNACKs/WalletWasabi/issues/722
https://github.com/zkSNACKs/WalletWasabi/issues/723
https://github.com/zkSNACKs/WalletWasabi/issues/724
https://github.com/zkSNACKs/WalletWasabi/issues/725
https://github.com/zkSNACKs/WalletWasabi/issues/726
https://github.com/zkSNACKs/WalletWasabi/issues/730
https://github.com/zkSNACKs/WalletWasabi/issues/718

## III. Education
https://github.com/nopara73/ZeroLink/issues/11
https://github.com/nopara73/ZeroLink/issues/43
https://github.com/nopara73/ZeroLink/issues/40
https://github.com/nopara73/ZeroLink/issues/44
https://github.com/nopara73/ZeroLink/issues/46
https://github.com/nopara73/ZeroLink/issues/48
https://github.com/nopara73/ZeroLink/issues/49
https://github.com/nopara73/ZeroLink/issues/54
https://github.com/nopara73/ZeroLink/issues/55
https://github.com/nopara73/ZeroLink/issues/66
https://github.com/nopara73/ZeroLink/issues/59
https://github.com/nopara73/ZeroLink/issues/69
https://github.com/nopara73/ZeroLink/issues/70
https://github.com/nopara73/ZeroLink/issues/71

## IV. Bitcoin Privacy Improvements
https://github.com/zkSNACKs/Meta/issues/18
https://github.com/zkSNACKs/Meta/issues/17
https://github.com/zkSNACKs/Meta/issues/10
https://github.com/zkSNACKs/Meta/issues/11
https://github.com/zkSNACKs/Meta/issues/8
https://github.com/zkSNACKs/Meta/issues/6
https://github.com/zkSNACKs/Meta/issues/5
https://github.com/zkSNACKs/Meta/issues/4
https://github.com/zkSNACKs/Meta/issues/3
https://github.com/nopara73/ZeroLink/issues/37
https://github.com/nopara73/ZeroLink/issues/41
https://github.com/nopara73/ZeroLink/issues/56
https://github.com/nopara73/ZeroLink/issues/58
https://github.com/nopara73/ZeroLink/issues/32
https://github.com/nopara73/ZeroLink/issues/60
https://github.com/nopara73/ZeroLink/issues/64
https://github.com/nopara73/ZeroLink/issues/65
https://github.com/nopara73/ZeroLink/issues/42
https://github.com/nopara73/ZeroLink/issues/6
https://github.com/nopara73/ZeroLink/issues/75
https://github.com/nopara73/ZeroLink/issues/74
https://github.com/zkSNACKs/WalletWasabi/issues/612
https://github.com/zkSNACKs/WalletWasabi/issues/728
https://github.com/zkSNACKs/WalletWasabi/issues/729

## V. General Wallet Features
https://github.com/zkSNACKs/Meta/issues/15
https://github.com/zkSNACKs/Meta/issues/12
https://github.com/zkSNACKs/Meta/issues/2
https://github.com/zkSNACKs/WalletWasabi/issues/486
https://github.com/zkSNACKs/WalletWasabi/issues/727
https://github.com/zkSNACKs/WalletWasabi/issues/731
https://github.com/zkSNACKs/WalletWasabi/issues/732

## VI. Accessibility
https://github.com/zkSNACKs/Meta/issues/9
https://github.com/zkSNACKs/Meta/issues/20
https://github.com/zkSNACKs/Meta/issues/7

## VII. Extending the Scope of Privacy
https://github.com/zkSNACKs/Meta/issues/16
https://github.com/zkSNACKs/Meta/issues/13
https://github.com/zkSNACKs/Meta/issues/14

## VIII. Unique Wallet Features
https://github.com/zkSNACKs/WalletWasabi/issues/496
https://github.com/zkSNACKs/WalletWasabi/pull/697
https://github.com/zkSNACKs/WalletWasabi/issues/608
https://github.com/zkSNACKs/WalletWasabi/issues/439
