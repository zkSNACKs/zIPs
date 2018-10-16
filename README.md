# Technical Plans

## Abstract

Wasabi Wallet is a Bitcoin privacy wallet that is based on the [ZeroLink Wallet Fungibility Framework](https://github.com/nopara73/ZeroLink/). While statistical privacy can be achieved today with it, the cost, convenience, intuitiveness and strength of this privacy can be greatly improved. Wasabi also needs to improve its accessibility and its general Bitcoin wallet features. Furthermore Wasabi should look into ways of extending the scope of its privacy protection to other, not closely Bitcoin related fields, like end to end encrypted messaging. Finally, Wasabi also needs to concentrate on its stability, performance, UX and code quality. This document aims to outline a starting plan to progress towards those objectives.

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

Wasabi's main focuses are Bitcoin and privacy, thus section [IV. Bitcoin Privacy Improvements](#iv-bitcoin-privacy-improvements)  . However a loss of privacy in fields those are traditionally considered to be out of the scope of a Bitcoin wallet, like sharing addresses through unsecure chat clients or checking transactions in a block explorer through the clearnet also pose privacy threats, ergo Wasabi cannot consider them entirely out of their scope, thus section [VII. Extending the Scope of Privacy](#vii-extending-the-scope-of-privacy).  
In the paper [Anonymity Loves Company: Usability and the Network Effect](https://www.freehaven.net/anonbib/cache/usability:weis2006.pdf) the authors note: 

> We show that in anonymizing networks, even if you were smart enough and had enough time to use every system perfectly, you would nevertheless be right to choose your system based in part on its usability for other users.

Therefore Wasabi should also pay attention to fields that help to increase the number of Wasabi users, with that bringing greater privacy for everyone, thus sections [III. Education](#iii-education) and [VI. Accessibility](#vi-accessibility).  

Furthermore Wasabi is a software, therefore it must not neglect general software quality issues, thus section [II. Stability, Performance, UX, Code Quality](#ii-stability-performance-ux-code-quality). The better software Wasabi is, the more users it will retain.

Wasabi is also a Bitcoin wallet, therefore it must improve general Bitcoin wallet related features as well, thus section [V. General Wallet Features](#v-general-wallet-features).

Finally, there are development opportunities, where the developers of Wasabi recognize that they could easily add some unique wallet features that no other wallets have, like in-wallet multi-wallet support or copypaste malware defense, thus section [VIII. Unique Wallet Features](#viii-unique-wallet-features).

Note that the developers of Wasabi are currently occupied by section [II. Stability, Performance, UX, Code Quality](#ii-stability-performance-ux-code-quality). This enjoys the highest priority. New issues will constantly come up as new users try to use the software. At this point it is unclear if Wasabi will ever have the resources to tackle other sections in this document.

### Wasabi Wallet Under the Hood

Wasabi is an [open-source](https://github.com/zkSNACKs/WalletWasabi/), desktop Bitcoin wallet, working on Windows, Linux and OSX, written in [.NET Core](https://en.wikipedia.org/wiki/.NET_Core) (C#), which is cross platform and open source .NET. Wasabi uses [NBitcoin](https://github.com/MetacoSA/NBitcoin/) as its Bitcoin library, to which Wasabi developers are frequent contributors: [@lontivero](https://github.com/lontivero), [@nopara73](https://github.com/nopara73). Wasabi uses [Avalonia](https://github.com/AvaloniaUI/Avalonia/) library as its UI framework where Wasabi developer [@danwalmsley](https://github.com/danwalmsley) is a maintainer.  
Wasabi does not support and does not plan to support other currencies in the future.

Let's look at what is going on under the hood for Wasabi, what design decisions and tradeoffs the developers made, so we can later understand where it can be improved.

After setting up Wasabi and generating a wallet, Wasabi welcomes the user with a load wallet screen. Unlike other wallets, Wasabi has a convenient way to use multiple wallets. Privacy centric users may be already used to achieve coin separation this way. However Wasabi provides a convenient in-wallet coin separation interface too, more information will be provided about that later on. Since coin separation can be easily achieved without multiple wallet files, initially the developers did not plan for such a wallet management system, our UX design choices naturally lead us down this road.
![](https://i.imgur.com/139bjDH.png)

Wasabi has a status bar that shows meta information about the state of the wallet. To better understand the architecture of the wallet it is helpful to go through them.

The "Tor" label shows the status of the Tor daemon. Tor is an anonymity network which Wasabi ships with by default and runs on the background. Although the user can opt to use its own Tor instance if it wishes to. All communication with Wasabi's backend server goes through Tor. Wasabi also utilizes multiple Tor identities where applicable. For example registering coinjoin inputs and outputs happen through different Tor identities to avoid linking.

![](https://i.imgur.com/054zvbY.png)

Wasabi's backend is used to facilitate [Chaumian CoinJoin](https://github.com/nopara73/ZeroLink#ii-chaumian-coinjoin) coordination between the mixing participants and to serve Golomb-Rice filters to the clients, similarly to [BIP157](https://github.com/bitcoin/bips/blob/master/bip-0157.mediawiki)-[BIP158](https://github.com/bitcoin/bips/blob/master/bip-0158.mediawiki). More information will be provided about the difference soon. Before that, it is worth pointing out that the design choice of building a light wallet was made because such wallet can attract orders of magnitude larger number of users compared to a a wallet on top of a full node, and more users means larger and faster coinjoins. Historically all light wallets were vulnerable against some kind of network observer due to unprivate utxo fetching. A few years ago the only type of wallet that wasn't vulnerable was a full node, like Bitcoin Core. The first iteration of Wasabi was [HiddenWallet](https://github.com/zkSNACKs/WalletWasabi/tree/hiddenwallet-v0.6), which was a full-block SPV wallet that aimed to leverage useability without compromising on privacy through the omission of initial blockchain downloading compared to a full node. In theory it was a light wallet, in practice it was hard to compete with Bitcoin Core's micro-optimizations and it was still painful to wait for wallet synchronization every time the wallet was opened.

![](https://i.imgur.com/lSXrOpJ.png)

Back to Wasabi. After loading the wallet the user can generate a receive address. Some important design choices were made here. Firstly, Wasabi had to be a Segregated Witness only wallet, so the registration of unconfirmed coinjoin outputs into a new coinjoin round is not to be a victim of malleability attacks. However, the developers of Wasabi decided to make the wallet native segwit (bech32) only, not supporting wrapped segwit. This way the backend server can leverage this and only generate filters regarding bech32 addresses. This makes Wasabi's filter size a few megabytes today, instead of >1GB (ToDo: insert source here, I guess Blummer Tamas came up with this number an posted to the dev mailing list.) On the first glance this may be seen as hazardous to privacy, however Wasabi user utxos can be identified as Wasabi utxos by the huge coinjoins that only Wasabi do anyway, so no additional privacy loss happens there. In the future, as more and more wallets adopts bech32, Wasabi developers have to look at how to scale the performance and network usage of the wallet. Failing that Wasabi's initial sync will slow down.

![](https://i.imgur.com/aZb6TbZ.png)

Wasabi also maintains connection to the Bitcoin P2P network. After Wasabi receives the filters from the backend, it can download the required blocks (there are false positives, too) one block from one peer. This does not currently happen over Tor since the NBitcoin library we use to do the job does not support Tor yet. Sensitive information leak is already unlikely here since the only information a node can learn is that "one wallet may (false positives) have a transaction in one block someone fetched from me." Wasabi then stores the block in its entirety on disk so it won't fetch again. A possible privacy leak can happen if a wallet is being recovered, thus the work of Tor support here is desired in the future. Furthermore, storing blocks on the disk may take up too much space when the wallet is used extensively. There is room for improvement there as well.

![](https://i.imgur.com/GBP8mSH.png)

Wasabi receives incoming transactions from the nodes it is connected to. It is, while privacy preserving a more insecure way to handle this and should be improved in the future. Generally unconfirmed transactions are considered to be insecure regardless.

![](https://i.imgur.com/mI4lQDt.png)

Unlike in other Bitcoin wallets, to generate a Bitcoin address a label is not optional but required. That is because Wasabi has an intra-wallet blockchain analysis tool built into it, which tries to cluster utxos (Wasabi calls them coins) and based on these clusters the user can make an educated decision which coins to merge.

![](https://i.imgur.com/dkdlOZN.png)

Wasabi also has a History tab like any other Bitcoin wallet.

![](https://i.imgur.com/baDnln2.png)

Unlike other Bitcoin wallets, the user cannot spend from Wasabi without selecting coins, since ["Coin Control Is Must Learn If You Care About Your Privacy In Bitcoin"](https://medium.com/@nopara73/coin-control-is-must-learn-if-you-care-about-your-privacy-in-bitcoin-33b9a5f224a2) at least for today. The label field of the Send tab is also compulsory.

![](https://i.imgur.com/wUKTzE6.png)

By clicking on the Max button one can spend all selected coins. Spending whole coins is beneficial to privacy.  
The Bitcoin fee rates are fetched from the backend server, the source of these fees are Bitcoin Core's `estimatesmartfee`'s `CONSERVATIVE` output. Every fee query happens over Tor with new Tor identity. When clicking send, the wallet will broadcast the transaction to the Backend over Tor. This is sub-optimal, but because there's no Tor support for NBitcoin yet, broadcasting transaction P2P over the clearnet would be more dangerous. While NBitcoin P2P Tor support should be an interest of future work, it is a smarter long-term objective to implement [Dandelion](https://github.com/gfanti/bips/blob/master/bip-dandelion.mediawiki) from a broadcasting point of view when the Bitcoin network adopted it.

![](https://i.imgur.com/gQ1I7DZ.png)

Coins in Wasabi have Privacy and History properties. The anonymity set is just an estimation at the moment, however, by examining the mixes and other people's transactions we will be able to show accurate values. The History is the calculated clusters from the labels based on typical Blockchain analysis heuristics. For example, if the user joins together a "foo" labeled coins with a "bar" labeled coin at sending, then the change coin history will show "change of (foo), change of (bar)". From this users are able to make educated decisions which coins not to join together at all cost. Human input is invaluable.

![](https://i.imgur.com/4vUiSWr.png)

Wasabi has a CoinJoin tab as well, its use is straightforward. The user queues its coins for coinjoin and wait for others to join the mix.

![](https://i.imgur.com/8wFd88C.png)

If the user does not wish to proceed then the user can dequeue its coins.

![](https://i.imgur.com/lxmdxIG.png)

After a mix successfully executed, the resulting CoinJoin transaction will look like the following (real example): https://www.smartbit.com.au/tx/a0855875fd3d19522568ad673e4b52e11691d837021d74eef0d177f9e0950bf2

![](https://i.imgur.com/rcVcPOM.png)

Wasabi also has a Tor website where one can see real time statistics about the mixes: http://wasabiukrxmkdgve5kynjztuovbg43uxcbcxn6y2okcrsg7gb6jdmbad.onion/

![](https://i.imgur.com/4vE1IiD.png)

The above number means, Wasabi's coinjoins created >102BTC outputs with equal value.

## II. Stability, Performance, UX, Code Quality

As it was discussed above, the main priority of Wasabi developers is currently the stability, performance, code quality and user experience of Wasabi. Great care must be taken here because the more users a network reliant privacy software has, the better privacy it offers. The more users there are, the better the software is. In Wasabi it's the most apparent during CoinJoins. The more users participating in the round, the higher anonymity set a CoinJoin achieves and the more frequent the rounds are. In our calculations if Wasabi would acquire the volume of the most popular Bitcoin mixers, Wasabi could provide a 100 anonymity set round with the denomination of 0.1BTC every 3 to 5 minutes.  
This section consists of many small issues, waiting to be solved one by one. Since solving these issues are often more effective than discussing them, these won't be extensively discussed in this document. Related issues as of 2018 October:

https://github.com/zkSNACKs/WalletWasabi/issues/407, 
https://github.com/zkSNACKs/WalletWasabi/issues/386, 
https://github.com/zkSNACKs/WalletWasabi/issues/416, 
https://github.com/zkSNACKs/WalletWasabi/issues/408, 
https://github.com/zkSNACKs/WalletWasabi/issues/430, 
https://github.com/zkSNACKs/WalletWasabi/issues/436, 
https://github.com/zkSNACKs/WalletWasabi/issues/462, 
https://github.com/zkSNACKs/WalletWasabi/issues/465, 
https://github.com/zkSNACKs/WalletWasabi/issues/474, 
https://github.com/zkSNACKs/WalletWasabi/issues/445, 
https://github.com/zkSNACKs/WalletWasabi/issues/553, 
https://github.com/zkSNACKs/WalletWasabi/issues/420, 
https://github.com/zkSNACKs/WalletWasabi/issues/414, 
https://github.com/zkSNACKs/WalletWasabi/issues/409, 
https://github.com/zkSNACKs/WalletWasabi/issues/570, 
https://github.com/zkSNACKs/WalletWasabi/issues/573, 
https://github.com/zkSNACKs/WalletWasabi/issues/427, 
https://github.com/zkSNACKs/WalletWasabi/issues/572, 
https://github.com/zkSNACKs/WalletWasabi/issues/587, 
https://github.com/zkSNACKs/WalletWasabi/issues/588, 
https://github.com/zkSNACKs/WalletWasabi/issues/432, 
https://github.com/zkSNACKs/WalletWasabi/issues/597, 
https://github.com/zkSNACKs/WalletWasabi/issues/596, 
https://github.com/zkSNACKs/WalletWasabi/issues/610, 
https://github.com/zkSNACKs/WalletWasabi/issues/580, 
https://github.com/zkSNACKs/WalletWasabi/issues/449, 
https://github.com/zkSNACKs/WalletWasabi/issues/611, 
https://github.com/zkSNACKs/WalletWasabi/issues/533, 
https://github.com/zkSNACKs/WalletWasabi/issues/618, 
https://github.com/zkSNACKs/WalletWasabi/issues/440, 
https://github.com/zkSNACKs/WalletWasabi/issues/622, 
https://github.com/zkSNACKs/WalletWasabi/issues/642, 
https://github.com/zkSNACKs/WalletWasabi/issues/655, 
https://github.com/zkSNACKs/WalletWasabi/issues/675, 
https://github.com/zkSNACKs/WalletWasabi/issues/688, 
https://github.com/zkSNACKs/WalletWasabi/issues/689, 
https://github.com/zkSNACKs/WalletWasabi/issues/691, 
https://github.com/zkSNACKs/WalletWasabi/issues/692, 
https://github.com/zkSNACKs/WalletWasabi/issues/564, 
https://github.com/zkSNACKs/WalletWasabi/issues/444, 
https://github.com/zkSNACKs/WalletWasabi/issues/714, 
https://github.com/zkSNACKs/WalletWasabi/issues/716, 
https://github.com/zkSNACKs/WalletWasabi/issues/720, 
https://github.com/zkSNACKs/WalletWasabi/issues/501, 
https://github.com/zkSNACKs/WalletWasabi/issues/719, 
https://github.com/zkSNACKs/WalletWasabi/issues/722, 
https://github.com/zkSNACKs/WalletWasabi/issues/723, 
https://github.com/zkSNACKs/WalletWasabi/issues/724, 
https://github.com/zkSNACKs/WalletWasabi/issues/725, 
https://github.com/zkSNACKs/WalletWasabi/issues/726, 
https://github.com/zkSNACKs/WalletWasabi/issues/730, 
https://github.com/zkSNACKs/WalletWasabi/issues/718

## III. Education

While education, content creation and marketing are things those havae little place in a technical document, it is still important part of the big picture. Through education Wasabi can obtain new users. The more Wasabi users there are, the better their privacy is. Advancing this issue can take various, often opportunistic forms.

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
