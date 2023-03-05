# giftstr
giftstr is work in progress to implement assignment, transfer, and redemption of gift cards by merchants using [nostr](https://github.com/nostr-protocol/nostr).

- [Why gift cards?](https://github.com/bajjer/giftstr#why-gift-cards)
- [User personas](https://github.com/bajjer/giftstr#user-personas)
- [Requirements](https://github.com/bajjer/giftstr#requirements)
- [User stories](https://github.com/bajjer/giftstr#user-stories)
- [What can go wrong?](https://github.com/bajjer/giftstr#what-can-go-wrong)
- [Nostrification](https://github.com/bajjer/giftstr#nostrification)

## Why gift cards?
- The global gift card market is [projected to reach $2.3 trillion by 2030](https://www.reportlinker.com/p06219503/Global-Gift-Cards-Industry.html)
- [62% of 18-34 year olds interested in gift cards through social media](https://www.cardcash.com/gift-card-statistics/)
- **fast and easy to buy**, fastr and easyr with nostr.
- **give flexibility to the recipient**, flexr with nostr.
- **increase the usage of bitcoin** and its lightning network as a medium of exchange
- **reduce the need intermediaries** for fiat on/off ramps between merchants and consumers

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
- **redeem, return, confirm succesful, or document failure** of redemption from Alice, same as Bob would
- **relist the gift card for sale** instead of redeeming it, **receive payment** and **confirm transfer**, same as Bob would
- view the history of succesful redemptions, documented failures, and outstanding gift cards Alice has assigned, same as Bob would

### David, the gift card recipient
- **receive gift card** to Alice's store from Alice, Bob, or Charlie and **confirmation of reassignment** by Alice
- **redeem, return, confirm succesful, document failure, relist for sale, and view history**, same as Charlie would

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

### Bob (or Charlie or David) redeems gift card from Alice
- redeem-01: Bob posts event (in response to assign-00, assign-05, transfer-07, or gift-01) with redemption request
- redeem-02: Alice posts event in response (to redeem-01) acknowledging redemption
- redeem-03: Alice sends DM to Bob with agreed upon redemption mechanism (e.g., gift card code)
- redeem-04 (optional): Bob posts event in response (to redeem-02) acknowledging redemption
- redeem-05 (optional): Bob posts event in response (to redeem-01) documenting lack of redemption
- redeem-06 (optional): Bob posts event in response (to redeem-05) confirming redemption and revoking complaint

### Bob (or Charlie or David) sells gift card to Charlie
-  transfer-01: Bob posts event in response to assign-00 or assign-05 (or transfer-07 or gift-01) to request permission from Alice to transfer his gift card
-  transfer-02: Alice posts event in response (to transfer-01) confirming Bob may transfer
-  transfer-03: Bob posts event referencing assign-05 (or assign-00 or transfer-07 or gift-01), transfer-02, and any additional terms in free-form
-  transfer-04: Charlie posts event in response (to transfer-03) requesting to purchase Bob's gift card
-  transfer-05: Bob posts event in response (to transfer-04) with a lightning invoice, exchange rate, and any fees
-  transfer-06: Charlie pays invoice and posts event in response (to transfer-05) with preimage of the invoice
-  transfer-07: Alice posts event acknowledging Charlie as the new owner of the gift card
-  transfer-08 (optional): Charlie posts event in response (to transfer-07) confirming receipt
-  transfer-09 (optional): Charlie posts event in response (to transfer-07) documenting lack of acknowledgment
-  transfer-10 (optional): Charlie posts event in response (to transfer-09) confirming acknowledgment and revoking complaint

### Charlie (or Bob or David) gifts David with gift card
- gift-00: Charlie posts event in response to transfer-07 (or assign-00 or assign-05 or gift-01) to assign gift card to David
- gift-01: Alice posts event in response to gift-00 confirming assignment to David
- gift-02 (optional): Charlie posts event citing gift-01 to share gift card with David (David will need gift-01 to redeem from Alice)

## What can go wrong?
Below are the risks associated with malicious behavior or technical errors (e.g., insufficient propogation by relays), which may lead to unintended financial loss to the different personas.

### Alice as a trusted third-party
Alice may refuse to allow redemption of the gift card even if she can identify the rightful owner

### Double-spend attempts
Alice may attempt to inject events with modified timestamps to spoof the rightful owner of a gift card

### Unpropogated events

## Nostrification
Below are initial hypotheses to reflect the above events as nostr events. Some of the nostr implementation possibilities (NIPs) utilized below may still be in draft form and additional NIPs may be necessary for a more efficient representation of the necessary events.

