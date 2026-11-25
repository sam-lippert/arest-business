# Usage

### What a billing account used, counted where the business counts it (a meter, a trip log, a request log) and
### reported as Usage Records: so many units of an offering, in a billing period, under the subscription they were
### used on where there is one. A record is reported once, under the id its reporter keeps it by, so a report sent
### twice is one record. A subscription's usage of a period is rated at the usage price it bills for that offering:
### the units past the price's free allowance, times its unit amount, which is what its invoice for the period ought
### to charge for them. Summed by offering over every account, usage is what an offering's cost is divided by
### (cost-attribution).

## Cross-domain References

### Amount (from money)
### Billing Period (from money)
### Offering (from offerings)
### Price (from offerings)
### Quantity (from offerings)
### Billing Account (from customers)
### Date (from receivables)
### Subscription (from subscriptions)
### Subscription Item (from subscriptions)

## Value Types

### The id the reporter keeps a record by, which makes a second report of it the same record.
Usage Record Id is a value type.
  The data type of Usage Record Id is text.

## Entity Types

Usage Record(.Usage Record Id) is an entity type.

## Fact Types

### The record
Usage Record is for Billing Account.
  Each Usage Record is for exactly one Billing Account.
Usage Record is of Offering.
  Each Usage Record is of exactly one Offering.
### How many units: calls, trips, miles, gigabytes.
Usage Record has Quantity.
  Each Usage Record has exactly one Quantity.
Usage Record falls in Billing Period.
  Each Usage Record falls in exactly one Billing Period.
Usage Record occurred on Date.
  Each Usage Record occurred on at most one Date.
Usage Record is under Subscription.
  Each Usage Record is under at most one Subscription.

### What it comes to
Subscription has used- Quantity of Offering in Billing Period. *
  For each Subscription, Offering and Billing Period, that Subscription has at most one used- Quantity of that Offering in that Billing Period.
### The units of a period the usage price charges for: those past its free allowance.
Subscription has billable- Quantity of Price in Billing Period. *
  For each Subscription, Price and Billing Period, that Subscription has at most one billable- Quantity of that Price in that Billing Period.
Subscription owes usage- Amount for Price in Billing Period. *
  For each Subscription, Price and Billing Period, that Subscription owes at most one usage- Amount for that Price in that Billing Period.
Subscription owes usage- Amount in Billing Period. *
  For each Subscription and Billing Period, that Subscription owes at most one usage- Amount in that Billing Period.
### Everything used of an offering in a period, over every account.
Offering has used- Quantity for Billing Period. *
  For each Offering and Billing Period, that Offering has at most one used- Quantity for that Billing Period.
### Used on a subscription that bills no usage price for it.
Usage Record is unpriced. *

## Derivation Rules

* Subscription has used- Quantity1 of Offering in Billing Period iff used- Quantity1 is the sum of Quantity2 where some Usage Record is under that Subscription and that Usage Record is of that Offering and that Usage Record falls in that Billing Period and that Usage Record has that Quantity2.

* Subscription has billable- Quantity1 of Price in Billing Period iff some Subscription Item is on that Subscription and that Subscription Item bills that Price and that Price is metered and that Price is for some Offering and that Subscription has some used- Quantity2 of that Offering in that Billing Period and that Price includes some free- Quantity3 and that free- Quantity3 is less than that used- Quantity2 and billable- Quantity1 is that used- Quantity2 minus that free- Quantity3 or some Subscription Item is on that Subscription and that Subscription Item bills that Price and that Price is metered and that Price is for some Offering and that Subscription has that used- Quantity1 of that Offering in that Billing Period and it is not true that that Price includes some free- Quantity.

* Subscription owes usage- Amount1 for Price in Billing Period iff that Subscription has some billable- Quantity of that Price in that Billing Period and that Price has some unit- Amount2 and usage- Amount1 is that unit- Amount2 times that billable- Quantity.

* Subscription owes usage- Amount1 in Billing Period iff usage- Amount1 is the sum of usage- Amount2 where that Subscription owes that usage- Amount2 for some Price in that Billing Period.

* Offering has used- Quantity1 for Billing Period iff used- Quantity1 is the sum of Quantity2 where some Usage Record is of that Offering and that Usage Record falls in that Billing Period and that Usage Record has that Quantity2.

* Usage Record is unpriced iff that Usage Record is under some Subscription and that Usage Record is of some Offering and it is not true that that Subscription meters that Offering.

## Deontic Constraints

### Usage on a subscription is the usage of the account that holds it.
It is obligatory that if some Usage Record is under some Subscription and that Usage Record is for some Billing Account then that Subscription is held by that Billing Account.
### What is used on a subscription is priced by it.
It is forbidden that some Usage Record is unpriced.

## Domain Metadata

Domain 'usage' has Description 'What a billing account used, as usage records of an offering in a billing period, rated at the usage price its subscription bills: the units past the free allowance times the unit amount; and usage by offering, which an offering''s cost is divided by.'.
