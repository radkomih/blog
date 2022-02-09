---
title: "How to host a blog on IPFS & Filecoin"
date: 2022-02-09T00:51:36+02:00
draft: true
---

## Why
<!-- First and formost, I try to respect the privacy of others and expect the same to me.  -->
I alwasy try to be authonomus and not raly and depend on somebody else. So when I decide to create something that I want to last and want to be in controll and not depend on somebody else parties that might decide what would hapen with the thing that i've created.


## What
[IPFS or Inter Planetary Files System Protocol](https://ipfs.io/) by Protocol Labs
> A peer-to-peer hypermedia protocol
> designed to preserve and grow humanity's knowledge
> by making the web upgradeable, resilient, and more open

- single distributed network of peer-to-peer nodes that route, store and share git objects (the data)
- it aims to replace HTTP by addressing file content (with hashes) rather than file locations
- content addressing (files split into smaller chunks and cryptographically hashed outputing a unique verifiable and reliable fingerprint `CID` resistent to tampering)

- no central point of failure, resistent to censorship, resilient, durable

---

[Filecoin](https://filecoin.io/) by Protocol Labs
- there is a finacial incentive to store the data (governed by smart contract and consensus)
- not having to raly on third party service like Pinata, Temporal, Infura

## How
Instal the IPFS go implementation on Linux

1. Download the Linux binary:
```sh
wget https://dist.ipfs.io/go-ipfs/v0.11.0/go-ipfs_v0.11.0_linux-amd64.tar.gz
```

2. Unzip the file:
```sh
tar -xvzf go-ipfs_v0.11.0_linux-amd64.tar.gz
```

3. Move into the go-ipfs folder and run the install script:
```sh
cd go-ipfs
sudo bash install.sh
```

4. Check if installed correclty
```sh
ipfs --version
```