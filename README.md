# giftstr
giftstr is work in progress to implement assignment, transfer, and redemption of gift cards by merchants using [nostr](https://github.com/nostr-protocol/nostr).

- [Why gift cards?](https://github.com/bajjer/giftstr#why-gift-cards)
- [User profiles](https://github.com/bajjer/giftstr#user-profiles)
- [Requirements](https://github.com/bajjer/giftstr#requirements)

## Why gift cards?
- **used by a lot of people.** The global gift card market is [projected to reach $2.3 trillion by 2030](https://www.reportlinker.com/p06219503/Global-Gift-Cards-Industry.html)
- **fast and easy to buy**
- **give flexibility to the recipient**
- **increase the usage of bitcoin** and its lightning network in global commerce
- make gift card markets **more efficient**

## User profiles
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
