# Offerings

### What a business sells and what it asks for it: its price book. An Offering is a thing sold: a plan, a product, a
### service, a kind of trip. An offering may include others, as a plan includes the products it gives access to. A
### Price is what one offering costs in one currency, charged in one of three ways: once, as a fee; every billing
### interval, as a recurring fee; or for each unit used, as a usage price, which may let some units of each period go
### free. A price may hold only within a bundle, as the price of a product per call within one plan. Prices are kept
### and not overwritten: a price no longer offered is retired, and a new price takes its place for new sales.

## Cross-domain References

Amount (from money)
Currency (from money)

## Value Types

Offering Name is a value type.
  The data type of Offering Name is text.
Price Id is a value type.
  The data type of Price Id is text.
### The unit an offering is used and billed in: a call, a trip, a mile, a seat, a gigabyte, an hour.
Billing Unit is a value type.
  The data type of Billing Unit is text.
### A number of units, whole or not: 3 seats, 12.4 miles, 1000 calls.
Quantity is a value type.
  The data type of Quantity is decimal.
### How often a recurring price is charged.
Billing Interval is a value type.
  The possible values of Billing Interval are 'weekly', 'monthly', 'quarterly', 'annually'.
Month Count is a value type.
  The data type of Month Count is integer.

## Entity Types

### What the business sells, and so what its costs are finally attributed to (cost-attribution).
Offering(.Offering Name) is an entity type.
Price(.Price Id) is an entity type.

## Fact Types

### The offering
### A bundle and what is in it: a plan and the products it gives access to, a package and the services in it.
Offering includes Offering.
  Each Offering, Offering combination occurs at most once in the population of Offering includes Offering.
  Offering includes Offering is acyclic.
### No longer sold to new buyers. Those who hold it keep it.
Offering is withdrawn.

### The price
Price is for Offering.
  Each Price is for exactly one Offering.
Price is in Currency.
  Each Price is in exactly one Currency.
### What one unit costs: the fee itself, or the price of one unit used.
Price has unit- Amount.
  Each Price has exactly one unit- Amount.
### A recurring price, charged every interval.
Price recurs every Billing Interval.
  Each Price recurs every at most one Billing Interval.
### A usage price, charged for each unit used.
Price is charged per Billing Unit.
  Each Price is charged per at most one Billing Unit.
### The units of each billing period a usage price lets go free: an allowance.
Price includes free- Quantity.
  Each Price includes at most one free- Quantity.
### The bundle a price holds within, as a product's price per call within one plan. A price that names none holds
### whatever the offering is bought with.
Price applies within Offering.
  Each Price applies within at most one Offering.
### No longer offered for new sales. What was sold at it is still billed at it.
Price is retired.

### The months a billing interval spans, by which recurring prices are compared month for month. A weekly interval
### spans no whole number of months and is given none.
Billing Interval spans Month Count.
  Each Billing Interval spans at most one Month Count.

### What follows from the price
Price is recurring. *
Price is metered. *
Price is one-time. *
### What a recurring price comes to in a month: an annual price over twelve.
Price has monthly- Amount. *
  Each Price has at most one monthly- Amount.
### A price within a bundle that does not include the offering the price is for.
Price is outside its bundle. *

## Constraints

### A price recurs or is charged by use, never both.
For each Price, at most one of the following holds: that Price recurs every some Billing Interval; that Price is charged per some Billing Unit.
### An allowance is of units used.
If some Price includes some free- Quantity then that Price is charged per some Billing Unit.

## Derivation Rules

* Price is recurring iff that Price recurs every some Billing Interval.
* Price is metered iff that Price is charged per some Billing Unit.
* Price is one-time iff that Price is for some Offering and it is not true that that Price is recurring and it is not true that that Price is metered.
* Price has monthly- Amount1 iff that Price recurs every some Billing Interval and that Billing Interval spans some Month Count and that Price has some unit- Amount2 and monthly- Amount1 is that unit- Amount2 divided by that Month Count.
* Price is outside its bundle iff that Price applies within some Offering1 and that Price is for some Offering2 and it is not true that that Offering1 includes that Offering2.

## Deontic Constraints

### A price within a bundle is for something the bundle includes.
It is forbidden that some Price is outside its bundle.

## Instance Facts

Billing Interval 'monthly' spans Month Count '1'.
Billing Interval 'quarterly' spans Month Count '3'.
Billing Interval 'annually' spans Month Count '12'.

## Domain Metadata

Domain 'offerings' has Description 'What a business sells and its price book: offerings and the bundles they make, and prices charged once, every billing interval or for each unit used, with a free allowance and a monthly equivalent.'.
