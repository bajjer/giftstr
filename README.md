# giftstr
giftstr is work in progress to implement assignment, transfer, and redemption of gift cards using [nostr](https://github.com/nostr-protocol/nostr).

- [Why gift cards?](https://github.com/bajjer/giftstr#why-gift-cards)
- [User personas](https://github.com/bajjer/giftstr#user-personas)
- [Requirements](https://github.com/bajjer/giftstr#requirements)
- [User stories](https://github.com/bajjer/giftstr#user-stories)
- [What can go wrong?](https://github.com/bajjer/giftstr#what-can-go-wrong)
- [Nostrification](https://github.com/bajjer/giftstr#nostrification)

## Why gift cards?
- The global gift card market is [projected to reach $2.3 trillion by 2030](https://www.reportlinker.com/p06219503/Global-Gift-Cards-Industry.html)
- [62% of 18-34 year olds interested in gift cards through social media](https://www.cardcash.com/gift-card-statistics/)
- **fast and easy to buy, return, redeem, and transfer**
- **give flexibility to the recipient** vs. plain gift
- **increase the usage of bitcoin** and its lightning network as a medium of exchange
- **reduce the need for on/off ramp intermediaries**
- can be a **source of funding** for bitcoiner merchants
- can be a means to diversify USD-denominated deposits away from the fractionally-reserved banking system for customers

## User personas
- Alice: A merchant that wishes to sell gift cards for her store
- Bob: Direct buyer of a gift card to Alice's store
- Charlie: Indirect buyer of a gift card to Alice's store
- David: Recipient of a gift in the form of a gift card purchased by another user

## Requirements
### Alice, the merchant
- **list gift cards for sale**
- **receive payment** in exchange for a gift card to her store
- **refund gift cards** to Bob or Charlie
- **identify the holders of gift cards** to her store at all times

### Bob, the direct gift card buyer
- **send payment** to Alice, in exchange for a gift card to Alice's store
- **redeem the gift card**, either in-person at Alice's brick-and-mortar store or online at Alice's online store
- **confirm succesful redemption** of his gift card
- **document failure** in redeeming his gift card 
- **list the gift card for sale** instead of redeeming it
- **receive payment** and **confirm transfer** of gift card to Charlie
- view the history of succesful redemptions, documented failures, and outstanding gift cards Alice has assigned

### Charlie, the indirect gift card buyer
- **request to buy** gift card to Alice's store from Bob
- **send payment** to Bob in exchange for a gift card
- **receive gift card** from Bob and **confirmation of reassignment** by Alice
- **redeem, return, confirm successful, or document failure** of redemption from Alice, same as Bob would
- **relist the gift card for sale** instead of redeeming it, **receive payment** and **confirm transfer**, same as Bob would
- view the history of successful redemptions, documented failures, and outstanding gift cards Alice has assigned, same as Bob would

### David, the gift card recipient
- **receive gift card** to Alice's store from Alice, Bob, or Charlie and **confirmation of reassignment** by Alice
- **redeem, return, confirm successful, document failure, relist for sale, and view history**, same as Charlie would

## User stories
### Bob buys a gift card from Alice
- assign-00: Alice posts event with a specified amount (e.g., 100 USD, 100 EUR, 1,000,000 sats, 0.01 BTC) and notes Bob as the recipient
- assign-01: Alice posts event with a specified amount (e.g., 100 USD, 100 EUR, 1,000,000 sats, 0.01 BTC) to offer gift cards for sale
- assign-02: Bob posts event in response (to assign-01) to request to purchase
- assign-03: Alice posts event in response (to assign-02) with a lightning invoice, exchange rate, and any fees
- assign-04: Bob pays invoice and posts event in response (to assign-03) with preimage of the invoice
- assign-05: Alice posts event in response (to assign-04) confirming Bob as owner of a new gift card
- assign-06 (optional): Bob may post an event in response to his event with the pre-image (assign-04) to document failure of receipt
- assign-07 (optional): Bob may post an event in response to failure of receipt (assign-06) confirming receipt and revoking complaint
- assign-08 (optional): Bob may post an event in response to assignment (assign-00) confirming receipt

### Bob (or Charlie or David) returns a gift card to Alice
- return-01: Bob posts event in response to Alice's confirmation event (assign-00 or assign-05 or transfer-07 or gift-01) to request to return gift card
- return-02: Alice posts event in response to Bob's request (return-01) and requests a lightning invoice with a specified amount of satoshis, disclosing the exchange rate and any fees
- return-03: Bob posts event in response (to return-02) including a lightning invoice for the specified amount of satoshis
- return-04: Alice pays invoice and posts event in response (to return-03) with preimage of the invoice, which also serves as the revocation of Bob's gift card
- return-05 (optional): Bob posts event in response (to return-04) acknowledging refund
- return-06 (optional): Bob posts event in response (to return-04) documenting lack of refund per terms
- return-07 (optional): Bob posts event in response (to return-06) confirming refund and revoking complaint

### Bob (or Charlie or David) redeems gift card from Alice
- redeem-01: Bob posts event (in response to assign-00, assign-05, transfer-07, or gift-01) with redemption request
- redeem-02: Alice posts event in response (to redeem-01) acknowledging redemption
- redeem-03: Alice sends DM to Bob with agreed upon redemption mechanism (e.g., gift card code)
- redeem-04 (optional): Bob posts event in response (to redeem-02) acknowledging redemption
- redeem-05 (optional): Bob posts event in response (to redeem-01) documenting lack of redemption
- redeem-06 (optional): Bob posts event in response (to redeem-05) confirming redemption and revoking complaint

### Bob (or Charlie or David) sells gift card to Charlie
- transfer-01: Bob posts event in response to assign-00 or assign-05 (or transfer-07 or gift-01) to request permission from Alice to transfer his gift card
- transfer-02: Alice posts event in response (to transfer-01) confirming Bob may transfer
- transfer-03: Bob posts event referencing assign-05 (or assign-00 or transfer-07 or gift-01), transfer-02, and any additional terms in free-form
- transfer-04: Charlie posts event in response (to transfer-03) requesting to purchase Bob's gift card
- transfer-05: Bob posts event in response (to transfer-04) with a lightning invoice, exchange rate, and any fees
- transfer-06: Charlie pays invoice and posts event in response (to transfer-05) with preimage of the invoice
- transfer-07: Alice posts event acknowledging Charlie as the new owner of the gift card
- transfer-08 (optional): Charlie posts event in response (to transfer-07) confirming receipt
- transfer-09 (optional): Charlie posts event in response (to transfer-07) documenting lack of acknowledgment
- transfer-10 (optional): Charlie posts event in response (to transfer-09) confirming acknowledgment and revoking complaint

### Charlie (or Bob or David) gifts David with gift card
- gift-00: Charlie posts event in response to transfer-07 (or assign-00 or assign-05 or gift-01) to assign gift card to David
- gift-01: Alice posts event in response to gift-00 confirming assignment to David
- gift-02 (optional): Charlie posts event citing gift-01 to share gift card with David (David will need gift-01 to redeem from Alice)

## What can go wrong?
Below are the risks associated with malicious behavior or technical errors (e.g., insufficient propogation by relays), which may lead to unintended financial loss to the different personas.

### Alice as a trusted party
#### Alice may refuse redemption of the gift card even if she can identify the rightful owner
Gift cards necessarily involve trust in the merchant. This risk is accepted and not net new to nostr as a medium for gift cards.

#### Alice may spoof positive reviews of herself
Since identity is managed via key pairs, we cannot easily verify that the purchasers and positive reviewers of Alice, contributing to her reputation, are "real" buyers and not impersonations by Alice. Consumers should seek merchants they trust based on their personal risk assesment and the positive reviews they know or trusted by people they know. This risk is accepted and deferred to the implementation of the reputation scoring by individual buyers. This risk is accepted and not net new to nostr as a medium for gift cards.

#### Users other than Alice may spoof negative reviews of Alice
Since redemption of the gift card to Alice's store involves an "off-band" transaction for a good or service, we cannot easily verify if a complaint raised by a buyer of Alice's gift cards is indeed due to a failed redemption. Merchants and consumers should seek social proof based on their network prior to assigning significant weight to any negative feedback provided to a merchant. This risk is accepted and not net new to nostr as a medium for gift cards.

### Double-spend attempts
- **Alice:** Alice may attempt to inject events with modified timestamps, i.e., `created_at`, to spoof the rightful owner of a gift card. This attack is risky for Alice as observers can note Alice's multiple confirmation notes tied to the same gift card. A malicious merchant would be better served refusing redemptions to obfuscate their malice from other consumers. 
- **Bob or Charlie or David:** Bob or Charlie or David may attempt to reassign a gift card they have previously sold, transferred, or gifted. Since Alice's confirmation is required after each reassignment, Alice would be able to detect such attempts by keeping track of their assignments.
- **Bob or Charlie or David in collusion with Alice:** Alice and one of Bob, Charlie, or David may collude to reassign a gift card after it has been sold or transferred to another party. In this instance, the double spend would be succesful but detectable by other observers if the events are adequately propagated. Similar to sole double spending, Alice would be better served refusing redemptions, which cannot be independently observed, whereas double-spend attempts, even if succesful, would be detectable with proper propagation.
- **A note about timestamping**: [OpenTimestamps](https://opentimestamps.org/) can prove that some information existed prior to the timestamp registered by the system. Including the hash of the most recent Bitcoin block can prove that some information existed later than that block. If both timestamps are used, the information must have existed sometime between the two timestamps and the `created_at` field may be compared to these references to validate its within the right range. However, a malicious user may timestamp an event with both OpenTimestamps and the Bitcoin block hash without publishing it to relays. There is no single global state and relays do not reject events `created_at` an earlier time by default. As such, the best defense against double spends is the fact that they can be detected with properly propogated events and reduced to a problem of trust in the merchant issuing the gift card.  

### Unpropagated events
The lack of appropriate propogation for certain events can cause discrepancies in the state observable to the users or independent observers. The below tables describe the risks associated with poor propagation of each of the events identified in the user stories. 

The riskiest of these events (marked with an asterisk (*) in the tables) are as follows:
- assign-04: Bob pays invoice and posts event in response (to assign-03) with preimage of the invoice
- assign-05: Alice posts event in response (to assign-04) confirming Bob as owner of a new gift card
- transfer-06: Charlie pays invoice and posts event in response (to transfer-05) with preimage of the invoice
- transfer-07: Alice posts event acknowledging Charlie as the new owner of the gift card

In the case of assign-04 and assign-05, Alice may perform an undetected double spend. While she is already a trusted party, the inability of other users to detect her double spends may allow for her to continue with her malicious streak without incurring the requisite reputational loss, particularly if Bob is also unable to propagate assign-06 to penalize Alice.

In the case of transfer-06 and transfer-07, Bob may perform an undetected double spend, which goes undetected by Alice and other users. This can lead to Bob obtaining funds from users other than Charlie.

**TODO:** Are there precautions that can be taken? Can there be a pre-arranged agreement on the relays to use? Would the relays be a federation? Can the federation be replaced in case relays are down? Is there a better mechanism to initiate transfers? Can a [nostr 2.0](https://medium.com/@colbyserpa/nostr-2-0-layer-2-off-chain-data-storage-b7d299078c60) type merkle tree scheme help reduce the risk of fraud/double spend?


#### Assignment
| Event | Risk from unpropagated event |
| ----- | ---------------------------- |
| 00 | Bob loses gift card, no loss to Alice |
| 01 | No loss to any party |
| 02 | No loss to any party |
| 03 | No loss to any party |
| 04 | * Bob may lose gift card if Alice is malicious, no loss to Alice |
| 05 | * Bob may lose gift card if Alice is malicious, no loss to Alice |
| 06 | Bob loses ability to penalize Alice, no loss to Alice |
| 07 | Alice incurs reputational loss, no loss to Bob |
| 08 | Alice loses reputational gain, no loss to Bob |

#### Return
| Event | Risk from unpropagated event |
| ----- | ---------------------------- |
| 01 | Bob unwillingly retains gift card, no loss to Alice |
| 02 | Bob unwillingly retains gift card, no loss to Alice |
| 03 | Bob unwillingly retains gift card, no loss to Alice |
| 04 | Alice may incur reputational loss, no loss to Bob |
| 05 | Alice loses reputational gain, no loss to Bob |
| 06 | Bob loses ability to penalize Alice, no loss to Alice |
| 07 | Alice incurs reputational loss, no loss to Bob |

#### Redemption
| Event | Risk from unpropagated event |
| ----- | ---------------------------- |
| 01 | Bob unwillingly retains gift card, no loss to Alice |
| 02 | Bob unwillingly retains gift card, no loss to Alice |
| 03 | Bob unwillingly retains gift card, no loss to Alice |
| 04 | Alice loses reputational gain, no loss to Bob |
| 05 | Bob loses ability to penalize Alice, no loss to Alice |
| 06 | Alice incurs reputational loss, no loss to Bob |

#### Transfer
| Event | Risk from unpropagated event |
| ----- | ---------------------------- |
| 01 | Bob unwillingly retains gift card, no loss to Alice (or Charlie) |
| 02 | Bob unwillingly retains gift card, no loss to Alice (or Charlie) |
| 03 | Bob unwillingly retains gift card, no loss to Alice (or Charlie) |
| 04 | Bob unable to sell to Charlie, Charlie unable to buy from Bob, no loss to Alice |
| 05 | Bob unable to sell to Charlie, Charlie unable to buy from Bob, no loss to Alice |
| 06 | * Charlie may lose gift card if Bob is malicious, no loss to Alice or Bob |
| 07 | * Charlie may lose gift card if Bob is malicious, no loss to Alice or Bob |
| 08 | Alice loses reputational gain, no loss to Bob or Charlie |
| 09 | Charlie loses ability to penalize Alice, no loss to Alice or Bob |
| 10 | Alice incurs reputational loss, no loss to Bob or Charlie |

#### Gift
| Event | Risk from unpropagated event |
| ----- | ---------------------------- |
| 00 | Charlie unable to gift David, David unable to receive gift card, no loss to Alice or Bob |
| 01 | Charlie unable to gift David, David unable to receive gift card, no loss to Alice or Bob |
| 02 | Charlie unable to gift David, David unable to receive gift card, no loss to Alice or Bob |

## Nostrification
Below is a set of nostr events that can represent the above events using the current set of NIPs. Some of the nostr implementation possibilities (NIPs) utilized below may still be in draft form and additional NIPs may be necessary for a more efficient representation of the necessary events.

**TODO:** There may be an improvement to the below to pre-select relays to publish events. 

| giftsr event | nostr event |
------------------------------
| assign-00 | Alice creates event with kind 30009, awards it to Bob with event kind 8 |
| assign-01 | Alice creates event with kind 30008, posts a sale notice with event kind 1 |
| assign-08 | Bob creates event with kind 30008 |

