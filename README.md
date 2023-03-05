# giftstr
giftstr is work in progress to implement assignment, transfer, and redemption of gift cards by merchants using [nostr](https://github.com/nostr-protocol/nostr).

- [Why gift cards?](https://github.com/bajjer/giftstr#why-gift-cards)
- [User personas](https://github.com/bajjer/giftstr#user-personas)
- [Requirements](https://github.com/bajjer/giftstr#requirements)
- [User stories](https://github.com/bajjer/giftstr/blob/main/README.md#user-stories)

## Why gift cards?
- **used by a lot of people.** The global gift card market is [projected to reach $2.3 trillion by 2030](https://www.reportlinker.com/p06219503/Global-Gift-Cards-Industry.html)
- **fast and easy to buy**
- **give flexibility to the recipient**
- **increase the usage of bitcoin** and its lightning network in global commerce
- make gift card markets **more efficient**

## User personas
- Alice: A merchant that wishes to sell gift cards for her store
- Bob: Direct buyer of a gift card to Alice's store
- Charlie: Indirect buyer of a gift card to Alice's store

## Requirements
### Alice, the merchant
- **list gift cards for sale**
- **receive payment** in exchange for a gift card to her store
- **refund gift cards** to Bob or Charlie
- **identify the holders of gift cards** to her store at all times

### Bob, the direct buyer
- **send payment** to Alice, in exchange for a gift card to Alice's store
- **redeem the gift card**, either in-person at Alice's brick-and-mortar store or online at Alice's online store
- **confirm succesful redemption** of his gift card
- **document failure** in redeeming his gift card 
- **list the gift card for sale** instead of redeeming it
- **receive payment** and **confirm transfer** of gift card to Charlie
- view the history of succesful redemptions, documented failures, and outstanding gift cards Alice has assigned

### Charlie, the indirect buyer
- **request to buy** gift card to Alice's store from Bob
- **send payment** to Bob in exchange for a gift card
- **receive gift card** from Bob and **confirmation of reassignment** by Alice
- **redeem the gift card**, **confirm succesful redemption**, or **document failure** of redempting from Alice, same as Bob would
- **relist the gift card for sale** instead of redeeming it, **receive payment** and **confirm transfer**, same as Bob would
- view the history of succesful redemptions, documented failures, and outstanding gift cards Alice has assigned, same as Bob would

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

### Bob returns a gift card to Alice
- return-01: Bob posts event in response to Alice's confirmation event (assign-05) to request to return gift card
- return-02: Alice posts event in response to Bob's request (return-01) and requests a lightning invoice with a specified amount of satoshis, disclosing the exchange rate and any fees
- return-03: Bob posts event in response (to return-02) including a lightning invoice for the specified amount of satoshis
- return-04: Alice pays invoice and posts event in response (to return-03) with preimage of the invoice, which also serves as the revocation of Bob's gift card

### Bob redeems gift card from Alice
- redeem-01: Bob posts event (in response to assign-05) with redemption request
- redeem-02: Alice posts event in response (to redeem-01) acknowledging redemption
- redeem-03: Alice sends DM to Bob with agreed upon redemption mechanism (e.g., gift card code)
- redeem-04 (optional): Bob posts event in response (to redeem-02) acknowledging redemption
- redeem-05 (optional): Bob posts event in response (to redeem-01) documenting lack of redemption
- redeem-06 (optional): Bob posts event in response (to redeem-05) confirming redemption and revoking complaint

### Bob sells gift card to Charlie
-  transfer-01: Bob posts event in response to assign-05 (or transfer-07) to request permission from Alice to transfer his gift card
-  transfer-02: Alice posts event in response (to transfer-01) confirming Bob may transfer
-  transfer-03: Bob posts event referencing assign-05, transfer-02, and any additional terms in free-form
-  transfer-04: Charlie posts event in response (to transfer-03) requesting to purchase Bob's gift card
-  transfer-05: Bob posts event in response (to transfer-04) with a lightning invoice, exchange rate, and any fees
-  transfer-06: Charlie pays invoice and posts event in response (to transfer-05) with preimage of the invoice
-  transfer-07: Alice posts event acknowledging Charlie as the new owner of the gift card
-  transfer-08 (optional): Charlie posts event in response (to transfer-07) confirming receipt
-  transfer-09 (optional): Charlie posts event in response (to transfer-07) documenting lack of acknowledgment
-  transfer-10 (optional): Charlie posts event in response (to transfer-09) confirming acknowledgment and revoking complaint

