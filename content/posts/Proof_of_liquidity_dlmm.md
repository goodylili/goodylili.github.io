+++
title = "Proof of Liquidity: to DLMM as a Service or Not"
date = "2024-10-22"
author = "Ukeje Goodness"
description = "What happens when DLMMs meet Proof of Liquidity"

[taxonomies]
tags = ["PoL", "Web3", "Development", "DLMM", "Berachain"]
+++

---

![poldlmm1.jpeg](/screenshot/poldlmm1.jpeg)


Proof of Liquidity is a breath of fresh air to the household flywheel of the consensus mechanisms of most proof of stake
systems. It’s supposed to improve capital efficiency and liquidity on the Berachain and boost ecosystem growth.

The article title is clickbait for nerds. Liquidity concentration is always a great idea, but now that you’re here, you
will learn about DLMMs, how they work, and my opinion on what happens when DLMMs meet proof of liquidity.

## Introduction to DLMMs

DLMMs (Dynamic Liquidity Market Makers) are systems/apps built to improve liquidity on dApps in the resident ecosystem
by improving liquidity management across protocols.

DLMMs are dual-facing services, which means they service protocols with liquidity and users with more earnings than they
would likely provide liquidity on DEXes directly.

DLMMs use advanced algorithms to dynamically adapt liquidity, pricing, and fees in response to market conditions, unlike
traditional Automated Market Makers (AMMs), which rely on static pricing formulas.

![poldlmm2.jpg](/screenshot/poldlmm2.png)

Image Credit: [Deep Dive on DLMMs by Tanishq Sharma](https://tutorials.hashnode.dev/deep-dive-on-metora-dlmm)

Liquidity is generally divided into different price bins that hold specific amounts of an asset for trading. When trades
are executed within the same bin, there is no slippage.

By aggregating price bins, DLMMs improve capital efficiency on the supported asset pairs, and everyone goes home happy:
traders with good trades unaffected by slippage and liquidity providers with more earnings.

DLMMs are not a new DeFi primitive. There’s a probability that they power your trades on your favourite DEXes.

One of the popular DLMMs is the Meteora DLMM on Solana. It sits pretty with
a  [$133m total value locked](https://arc.net/l/quote/siwoxzjh), servicing the biggest Solana DEXes, from Orca to
Jupiter, Banana Gun Bot, and many others.

![poldlmm3.jpg](/screenshot/poldlmm3.png)

DLMMs have been the holy grail for efficient liquidity across protocols, and if you know anything about Berachain’s
Proof of Liquidity, then you know it’ll take it up a notch.

## What Is Proof of Liquidity

Berachain is one of the new L1s debuting proof of liquidity, and mainnet is coming soon. What is Proof of Liquidity?

Proof of Liquidity is a consensus mechanism similar to Proof of Stake (PoS) that addresses the limitations of PoS by
eliminating the issues associated with locked liquidity and how it deters the development of DeFi environments onchain.

Unlike Traditional PoS systems, where tokens are locked to provide security to the network, Proof of Liquidity (PoL)
allows users to provide the same security. At the same time, liquidity remains usable within the Berachain ecosystem.

![poldlmm4.jpg](/screenshot/poldlmm4.png)

Image Credit: [IQ.Wiki Proof of Liquidity](https://iq.wiki/wiki/pol-proof-of-liquidity/)

Proof of Liquidity enables all onchain participants to win by making liquidity the prize and taking the competition up a
notch by incentivising users to provide liquidity and aligning validators and liquidity providers under a unified
flywheel.

### The Berachain Proof of Liquidity Flywheel

![poldlmm5.jpg](/screenshot/poldlmm5.png)

Berachain PoL Flywheel. Source: Berachain Core Docs

Berachain PoL flywheel operates on a dual-token model with two tokens: $BERA and $BGT.

1. Users and validators stake $BERA (the Berachain gas token) to secure the network.
2. Validators are randomly chosen to propose new blocks based on their stakes on the network.
3. The validator chosen to propose a new block receives and distributes the Berachain Governance Token ($BGT) to reward
   vaults.
4. Users provide liquidity by depositing tokens into pools where they’d receive receipt tokens.
5. Users stake their receipt tokens in reward vaults to earn $BGT emissions
6. Users can stake the receipt tokens in reward vaults to earn $BGT
7. Participants can burn $BGT for $BERA tokens at a 1:1 ratio for claims.

Proof of Liquidity is dangerous...to other chains vying for liquidity. Through this flywheel, you can expect more
competition from protocols and validator ecosystems since both will actively try to attract liquidity by offering
bribes, great APYs, and other incentives.

## Future Outlook for Berachain’s DeFi

First, you need some foresight into what to expect from protocols building on Proof of Liquidity chains.

Protocols will provide more significant incentives than those on competitor chains to attract users, builders and
liquidity providers to Berachain. This time, the DAU could stay if the chain delivers on user experience. I expect proof
of liquidity to help with user retention and kill the shiny chain/protocol syndrome.

Berachain’s dual-token system creates a foundation for aligning interests. Validators get incentives in the form of
governance tokens and bribes. Liquidity providers get great emissions that would hopefully be enough in contrast to
rewards on other chains.

![poldlmm6.jpg](/screenshot/poldlmm6.png)
Image Credit: [Berachain Blog: The PoL Post](https://blog.berachain.com/blog/the-pol-post)

Bribes are ***the drop that fills the cup*** for Berachain’s Proof of Liquidity. We can expect them to create something
similar
to [the Curve Wars](https://kyrianalex.substack.com/p/the-curve-wars?utm_source=%2Fprofile%2F11997845-kyriansol-&utm_medium=reader2&s=r),
but this time, on a larger, more beneficial, more competitive scale.

In this case, the bribery mechanisms will permit validators to influence BGT emissions to specific pools, eventually
making those pools more lucrative. Protocols must outbid others to stay relevant and keep the DAU up only.

The wealth centralization and issues around bribery and governance integrity that surfaced during the Curve wars may
resurface on the same larger scale, but that’s a story for Christmas dinner.

## DLMMs Seat on PoL: Throne or Dungeon

> TLDR: Validators and DLMMs Protocols would make great alliances which is great for the network and users.
>

I’ve always thought DLMMs are a crypto nerd’s fancy name for hyperefficient liquidity aggregators.

If Berachain and DLMMs have anything in common, it is keeping liquidity at the centre of everything. The PoL incentive
structure is designed for consistent liquidity, which is where they mismatch.

The synergy between DLMMs and PoL could theoretically create deeper liquidity pools with reduced slippage and have DLMM
protocols emerge as the holy grail, assuming there’s a stable influx of liquidity incentivised through $BGT emissions.
But what happens next?

Building an LP layer on PoL would make sense regarding aligning emissions and liquidity, whereas DLMMs would be the
great leveller.

For protocols that DLMMs service, achieving reduced impermanent loss and better yield opportunities is compelling, but
the protocols must find a balance between attracting liquidity and maintaining sustainable yields.

Validator preferences for efficient DLMM strategies could create a feedback loop that rewards certain protocols over
others, but *who cares about decentralization these days?*

The risks associated with DLMMs still pour surface in this case. DLMMs would increase competition and stabilize
liquidity concentration. The interplay between DLMM strategies and validator bribes should introduce an intricate game
theory dynamic that would create new avenues for bad actors to manipulate markets.

## Conclusion

DLMM as a service protocols are promising, and the fact that there’s only one popular one in crypto is already making
waves for the resident ecosystem.

DLMMs could make or break proof of liquidity depending on their adoption. Nonetheless, crypto mirrors humanity; the path
to destruction always has too many turning points for salvation.