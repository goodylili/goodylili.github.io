+++
title = "Asset Rights Abstraction: The First Wonder of Crypto"
date = "2024-10-22"
author = "Ukeje Goodness"
description = "Crypto solution to ownership may be half-baked"

[taxonomies]
tags = ["ownership", "Web3", "Development", "Sui"]
+++

---


![ara1.jpg](/screenshot/ara1.png)

Crypto’s fashion of enabling actual ownership and eliminating intermediaries is attractive in onboarding participants who intend to create and distribute value onchain.

Today, there’s a high-pitched trust function when interacting with dApps in crypto, especially in DeFi. While considering the prevalent incessant hacks, it’s clear that asset ownership needs to stay with the asset owners sometimes.

This article overviews the concept of Asset Rights Abstraction, its place in the future of DeFi, and how it could change how ownership is perceived on the internet.

### **The State of Asset Rights in Crypto**

![ara2.jpg](/screenshot/ara2.png)

Asset Rights Flow Explained Like I’m Five

Asset rights define ownership, control, and transferability of digital and real-world assets represented on-chain. These rights are guaranteed through blockchains' immutability, transparency, and “decentralization.”

Crypto crafted a new paradigm for ownership and asset rights. Newly spawned critical issues around security, legal recognition, and user protection could use some addressing.

Simply put, the properties of asset rights can be summed up into:

- **Ownership**: Public key cryptography, where the private keys sign transactions to prove asset ownership.
- **Custody**: dApps and wallet providers allow users to control their assets as they interact directly with smart contracts.
- **Roles**: Owners, users, and managers may have different rights decoupled from direct ownership.
- **Transferability**: Control and custody is the owner’s choice. They can transfer to a new owner or burn the asset onchain.

Ownership is custody, and responsibility is a big deal. The sensitivity of saving private keys in secure locations already falls short of the UX in crypto’s alternative industry, especially when you consider that private keys sign you up for a single point of failure and unlock defects across the other asset-right properties.

Custody is also perplexing. When you deposit liquidity into a protocol, you transfer custody to the protocol’s smart contracts.

- It’s not necessary in some cases. e.g. staking
- You may be missing out on holder benefits.

Finally, complex ownership systems are challenging to simulate in crypto on the client side. Multi-sigs are the holy grail for assets with multiple owners, and on most blockchains, they’re hardly great at it.

### **What Is Asset Rights Abstraction**

![ara3.jpg](/screenshot/ara3.png)

Asset Rights Abstraction (ARA) is a framework that separates **asset custody** from **ownership rights** in a user-centric fashion. It allows users to manage and use their assets flexibly and autonomously without transferring full ownership to other users or protocols.

In implementation, ARA uses a mechanism where the **rights** associated with an asset (e.g., permissions to transfer, lease, or modify) are separate from the control of the asset without giving up full ownership. This way, owners can exercise ownership rights and allow others to exercise certain functions related to the assets.

![ara4.jpg](/screenshot/ara4.png)

Asset Rights Abstraction Flow

- The owner (asset custodian) has the technical ability to use, manage, interact with, and receive all the perks associated with the asset.
- The beneficial owner (asset rights owner) has legal entitlements (enforced by the smart contract) to sell, transfer or lease based on predefined conditions.

ARA’s ability to differentiate between **control** and **rights** opens up opportunities for flexible asset management, where users can delegate control while maintaining ownership rights.

Asset rights abstraction is one of the components for building great creator economies on the internet. Here are some cases:

- Lending platforms can now collateralize like traditional banks (especially in cases where the platform is financed by protocol-owned liquidity). The user keeps the asset and gets the loan, while the platform keeps the rights.
- Digital creators can allow others to use their content while retaining ownership without relying on intermediaries to cut off access (record labels, I mean no disrespect).
- Users can delegate and automate actions like (executing transactions, making payments, or managing access) without relinquishing ownership.

... and many other cases. I think ARAs are the blue pill that fixes many issues associated with asset rights in traditional systems.

Traditional systems are fragmented, with complex custody and slow settlement systems, which will benefit from ARAs, especially clear custody representation.

## Asset Rights Abstraction OnChain

Many chains already provide developers with the tools they need to implement asset rights abstraction.

Let’s take a look at Sui’s Kiosk: a primitive on Sui that allows users to trade digital assets securely with smart contracts that enforce guarantees that clearly define ownership roles.

Every Kiosk has some roles; the main ones are Kiosk Owner, Buyer and Creator.

- The Owner who places, lists, withdraws and borrows assets.
- Only Kiosk owners can take, list, borrow, or modify the assets in their Kiosk.
- Buyers that purchase items. They must meet the TransferPolicy for the trade to be successful.
- Creators that create and control the TransferPolicy for a single type of asset.
- Creators can set policies for trades like Royalties and deactivate or activate them immediately (fast finality).

This is built into Sui’s standard library. If you're interested in integrating it, check out [the documentation](https://docs.sui.io/standards/kiosk) to learn more.

## Applications of Asset Rights Abstraction OnChain

ARAs are how we can further consolidate the ownership principles that traditional system users clamour about.
Here are some fields of application outside DeFi that could use ARAs and how they’d find these helpful.

### Digital Content Distribution

Creators are bearing the brunt of the delicate nature of digital asset distribution in traditional systems; however, everything could change.

- **Content Distribution**: ARAs could give creators full rights to their publications onchain while allowing consumers access without transferring ownership.
- **Streaming and Leasing:** Platforms can provide temporary access to content for a limited period, and after the time elapses, the contract enforces the agreement's details.
- **Billing**: Services can bill users gradually based on their rights to the asset. For example, Yearly retractable plans mean that when the user chooses to cancel, they don’t need to request a refund; it’s already in their wallet.

These solve distribution issues and receive earnings/income, the best of both worlds.

### Real Estate Interactions Onchain

Asset rights abstraction is also great for enforcing the complexities of fractional ownership and time sharing on assets.

- **Fractional Ownership**: Dividing tangible assets into shares allows multiple owners to have rights based on their own units.
- **Time Sharing**: Owners can schedule property access and enforce the lease time for specific time slots.

There’s so much more ARAs can do for real estate and RWAs.

### **Digital Identity and Data Privacy**

ARAs will fix digital identity and data privacy concerns around access periods and delegation authentication.

- **Temporary Access**: Users can grant temporary access to their data for verification purposes.
- **Delegated Authentication**: Third-party providers can request authentication rights without transferring the user’s identity data.

Digital identity and data privacy are delicate issues that require abstraction.

## **Conclusion**

Asset rights abstraction helps clarify the rules for ownership. You can’t mid-curve it; it is what it is.
I’m looking forward to seeing how other chains and protocols implement asset abstraction, which could prevent some DeFi hacks and give owners more control.
