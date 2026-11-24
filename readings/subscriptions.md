# Subscriptions

### A billing account's standing order for an offering, from the day it starts until the day it ends. Each
### Subscription Item is one price the subscription bills: a recurring price, at a quantity where it is sold by the
### seat or the vehicle, or a usage price, charged for what is used. A subscription may begin with a trial of the days
### its offering gives. It moves on what happens to it: a trial starts; an invoice for it is paid; a payment for one
### fails, which leaves it past due until an invoice is paid (dunning); it ends. Whether a past-due or delinquent
### subscription is waited on, suspended or ended is the business's own to say. What a subscription brings in a month
### is derived from its items, and summed by offering it is the offering's monthly recurring revenue.

## Cross-domain References

Amount (from money)
Currency (from money)
Offering (from offerings)
Price (from offerings)
Quantity (from offerings)
Billing Account (from customers)
Day Count (from customers)
External System (from customers)
External Reference (from customers)
Invoice (from receivables)
Payment (from receivables)
Date (from receivables)

## Value Types

Subscription Id is a value type.
  The data type of Subscription Id is text.
Subscription Item Id is a value type.
  The data type of Subscription Item Id is text.

## Entity Types

Subscription(.Subscription Id) is an entity type.
Subscription Item(.Subscription Item Id) is an entity type.

## Fact Types

### The subscription
### As an invoice is, a subscription a biller reports is known before whose it is or what it is to.
Subscription is held by Billing Account.
  Each Subscription is held by at most one Billing Account.
### What is subscribed to: a plan, a contract, a service.
Subscription is to Offering.
  Each Subscription is to at most one Offering.
Subscription starts on Date.
  Each Subscription starts on at most one Date.
### It ends at the end of the period it is in, unless the setting is undone before then.
Subscription is set to cancel at period end.
### The day it ended, reported when it ends, whoever ended it.
### That it ended, reported by whoever ended it, or known by the day it did.
Subscription ends. ++
Subscription ends on Date.
  Each Subscription ends on at most one Date.
Subscription is recorded in External System as External Reference.
  For each Subscription and External System, that Subscription is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Subscription is recorded in that External System as that External Reference.
### Which subscription an invoice bills, where it bills one.
Invoice is for Subscription.
  Each Invoice is for at most one Subscription.

### The items
Subscription Item is on Subscription.
  Each Subscription Item is on exactly one Subscription.
Subscription Item bills Price.
  Each Subscription Item bills exactly one Price.
  For each Subscription and Price, at most one Subscription Item is on that Subscription and bills that Price.
### Seats, vehicles, licences: how many units of a recurring price are billed. An item without one bills one.
Subscription Item has Quantity.
  Each Subscription Item has at most one Quantity.

### The trial
### The days of trial an offering gives a new subscription before its first charge.
Offering gives trial- Day Count.
  Each Offering gives at most one trial- Day Count.
Subscription trial starts.
### Stated by the biller, or derived from the start and the trial days the offering gives. A trial of an offering that
### gives trial days ends by them.
Subscription has trial-end- Date. +
  Each Subscription has at most one trial-end- Date.

### What follows
### The offering subscribed to and the offerings it includes.
Subscription covers Offering. *
### An offering the subscription bills a usage price for.
Subscription meters Offering. *
Subscription Item is outside its subscription. *
### Reported by the biller, or derived from the payments allocated to the invoice.
Subscription has paid- Invoice. ++
Subscription has failed- Invoice. ++
Subscription is delinquent. *
### Started and not ended.
Subscription is in force. *
### In its trial: the trial started and no invoice for it has been paid.
Subscription is in its trial. *
Billing Account has trialed- Subscription. *
### What an item brings in a month: its price's monthly amount times its quantity.
Subscription Item has monthly- Amount. *
  Each Subscription Item has at most one monthly- Amount.
Subscription has monthly- Amount. *
  Each Subscription has at most one monthly- Amount.
### Monthly recurring revenue: what the subscriptions in force and past their trials bring in a month, by offering
### and currency.
Offering brings monthly-recurring- Amount in Currency. *
  For each Offering and Currency, that Offering brings at most one monthly-recurring- Amount in that Currency.

## Derivation Rules

+ Subscription has trial-end- Date1 if that Subscription trial starts and that Subscription starts on some Date2 and that Subscription is to some Offering and that Offering gives some trial- Day Count where Date1 = AddDays(Date2, Day Count).

* Subscription covers Offering iff that Subscription is to that Offering or that Subscription is to some Offering1 and that Offering1 includes that Offering.

* Subscription meters Offering iff some Subscription Item is on that Subscription and that Subscription Item bills some Price and that Price is for that Offering and that Price is metered.

* Subscription Item is outside its subscription iff that Subscription Item is on some Subscription and that Subscription Item bills some Price and that Price is for some Offering and it is not true that that Subscription covers that Offering.

+ Subscription has paid- Invoice if that Invoice is for that Subscription and that Invoice is settled.

+ Subscription has failed- Invoice if that Invoice is for that Subscription and some Payment is allocated to that Invoice for some Amount and that Payment fails.

* Subscription is delinquent iff some Invoice is for that Subscription and that Invoice is delinquent.

+ Subscription ends if that Subscription ends on some Date.
* Subscription is in force iff that Subscription starts on some Date and it is not true that that Subscription ends.

* Subscription is in its trial iff that Subscription trial starts and it is not true that that Subscription has some paid- Invoice.

* Billing Account has trialed- Subscription iff that Subscription is held by that Billing Account and that Subscription trial starts.

* Subscription Item has monthly- Amount1 iff that Subscription Item bills some Price and that Price has some monthly- Amount2 and that Subscription Item has some Quantity and monthly- Amount1 is that monthly- Amount2 times that Quantity or that Subscription Item bills some Price and that Price has that monthly- Amount1 and it is not true that that Subscription Item has some Quantity.

* Subscription has monthly- Amount1 iff monthly- Amount1 is the sum of monthly- Amount2 where some Subscription Item is on that Subscription and that Subscription Item has that monthly- Amount2.

* Offering brings monthly-recurring- Amount1 in Currency iff monthly-recurring- Amount1 is the sum of monthly- Amount2 where some Subscription is to that Offering and that Subscription is in force and it is not true that that Subscription is in its trial and that Subscription is held by some Billing Account and that Billing Account is billed in that Currency and that Subscription has that monthly- Amount2.

## Deontic Constraints

### A subscription bills prices of what it is to.
It is forbidden that some Subscription Item is outside its subscription.
### An invoice for a subscription bills the account that holds it.
It is obligatory that if some Invoice is for some Subscription and that Subscription is held by some Billing Account then that Invoice is billed to that Billing Account.
### A closed account holds no subscription in force.
It is forbidden that some Subscription is held by some Billing Account and that Billing Account is closed and that Subscription is in force.
### One trial to an account.
It is obligatory that each Billing Account has at most one trialed- Subscription.

## Instance Facts

### A subscription waits in 'Pending' until a trial starts or an invoice for it is paid. A failed payment leaves it
### 'Past Due' and a paid invoice brings it back. It ends from any status when its end is reported.
State Machine Definition 'Subscription' is for Object Type 'Subscription'.
Status 'Pending' is defined in State Machine Definition 'Subscription'.
Status 'Trialing' is defined in State Machine Definition 'Subscription'.
Status 'Active' is defined in State Machine Definition 'Subscription'.
Status 'Past Due' is defined in State Machine Definition 'Subscription'.
Status 'Ended' is defined in State Machine Definition 'Subscription'.
Status 'Pending' is initial in State Machine Definition 'Subscription'.
Status 'Ended' is terminal in State Machine Definition 'Subscription'.

Transition 'start-trial' is defined in State Machine Definition 'Subscription'.
  Transition 'start-trial' is from Status 'Pending'.
  Transition 'start-trial' is to Status 'Trialing'.
  Transition 'start-trial' is triggered by Fact Type 'Subscription trial starts'.
Transition 'activate' is defined in State Machine Definition 'Subscription'.
  Transition 'activate' is from Status 'Pending'.
  Transition 'activate' is to Status 'Active'.
  Transition 'activate' is triggered by Fact Type 'Subscription has paid- Invoice'.
Transition 'convert' is defined in State Machine Definition 'Subscription'.
  Transition 'convert' is from Status 'Trialing'.
  Transition 'convert' is to Status 'Active'.
  Transition 'convert' is triggered by Fact Type 'Subscription has paid- Invoice'.
Transition 'recover' is defined in State Machine Definition 'Subscription'.
  Transition 'recover' is from Status 'Past Due'.
  Transition 'recover' is to Status 'Active'.
  Transition 'recover' is triggered by Fact Type 'Subscription has paid- Invoice'.
Transition 'fail-first' is defined in State Machine Definition 'Subscription'.
  Transition 'fail-first' is from Status 'Pending'.
  Transition 'fail-first' is to Status 'Past Due'.
  Transition 'fail-first' is triggered by Fact Type 'Subscription has failed- Invoice'.
Transition 'miss-conversion' is defined in State Machine Definition 'Subscription'.
  Transition 'miss-conversion' is from Status 'Trialing'.
  Transition 'miss-conversion' is to Status 'Past Due'.
  Transition 'miss-conversion' is triggered by Fact Type 'Subscription has failed- Invoice'.
Transition 'fall-behind' is defined in State Machine Definition 'Subscription'.
  Transition 'fall-behind' is from Status 'Active'.
  Transition 'fall-behind' is to Status 'Past Due'.
  Transition 'fall-behind' is triggered by Fact Type 'Subscription has failed- Invoice'.
Transition 'end-pending' is defined in State Machine Definition 'Subscription'.
  Transition 'end-pending' is from Status 'Pending'.
  Transition 'end-pending' is to Status 'Ended'.
  Transition 'end-pending' is triggered by Fact Type 'Subscription ends'.
Transition 'end-trialing' is defined in State Machine Definition 'Subscription'.
  Transition 'end-trialing' is from Status 'Trialing'.
  Transition 'end-trialing' is to Status 'Ended'.
  Transition 'end-trialing' is triggered by Fact Type 'Subscription ends'.
Transition 'end-active' is defined in State Machine Definition 'Subscription'.
  Transition 'end-active' is from Status 'Active'.
  Transition 'end-active' is to Status 'Ended'.
  Transition 'end-active' is triggered by Fact Type 'Subscription ends'.
Transition 'end-past-due' is defined in State Machine Definition 'Subscription'.
  Transition 'end-past-due' is from Status 'Past Due'.
  Transition 'end-past-due' is to Status 'Ended'.
  Transition 'end-past-due' is triggered by Fact Type 'Subscription ends'.

## Domain Metadata

Domain 'subscriptions' has Description 'Standing orders of a billing account for an offering: their items and quantities, trials, the invoices that bill them, their life from pending through trial, active and past due to ended, and the monthly recurring revenue they bring by offering.'.
