---
title: "ETH Node Part 1: Raspberry Pi 4"
date: 2021-08-13
tags: ["ethereum", "web3", "node"]
categories: ["general"]
draft: false
---

I’ve been meaning to look into running an ETH node for a while, and Roman helpfully sent me [this guide](https://medium.com/@JustinMLeroux/running-ethereum-full-nodes-a-guide-for-the-barely-motivated-a8a13e7a0d31), which helpfully links to [another guide on running an ETH node on a Raspberry Pi 4](https://kauri.io/#communities/Ethereum%20Node%20Runners/running-an-ethereum-full-node-on-a-raspberrypi-4-/). There was some scepticism if a Raspberry Pi 4 is actually powerful enough, but the hardware would cost me just $300 for both the Pi and an external 500GB SSD, and it’s still usable for other projects even if this doesn’t work out.

So a few things to note:

An SSD is the bare minimum to run a full node.

The Pi runs really hot while syncing, so a metal case with fan is recommended. I stupidly forgot to put the thermal pads so it runs around 65-75 degrees C.

After the initial boot set-up making sure that SSH is running, I could just do the rest of the steps via SSH from my desktop. (WSL is a good tool!)

# What do you get out of running an ETH node?
The short answer is: nothing, other than increasing the security and redundancy of the ETH network.

The slightly longer answer is: more personal security using wallets and if developing dApps. It is also easier to query the state of the blockchain without relying on a 3rd party service. However, for full data, that would require an archive node, which would require TBs of SSD storage, more RAM and more powerful CPU than what the Raspberry Pi 4 provides.

[The Ethereum Foundation website has a more comprehensive overview.](https://ethereum.org/en/developers/docs/nodes-and-clients/)

[Running an ETH node is different from mining!](https://bitcoin.stackexchange.com/questions/59220/what-is-the-difference-between-a-miner-and-a-full-node) Miners process the block transactions and nodes are the ones checking if the transactions are valid.

# How long will it take to sync?
I have no idea. The first 12 hours reached 140M state entries (the current total is at least 700M?), but this rate has now slowed down to only another 35M state entries in the past 12 hours.

Or maybe Roman is right and it will never reach full sync.