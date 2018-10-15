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

### The Structure of Wasabi Wallet



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
