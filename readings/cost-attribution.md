# Cost Attribution

### What a business spends, traced from the bills its suppliers send to what it sells. A supplier bills, for a
### billing period, for the resources it supplies: a server, a database, a seat, fuel for a vehicle, a policy. Each
### line of a bill is for one resource. A resource is allocated to the cost objects it serves (a service, a vehicle, a
### route, a team) by a share, and a cost object to the offerings it serves (a product, a kind of trip) by a share,
### so each period's cost is attributed once, in parts. A cost object may instead be allocated to its offerings by
### usage: each offering's part is the share of the period's usage it had. An offering's unit cost is the cost
### attributed to it over the usage it had in the period.

## Cross-domain References

Amount (from money)
Currency (from money)
Billing Period (from money)

## Value Types

Supplier Name is a value type.
  The data type of Supplier Name is text.
Resource Name is a value type.
  The data type of Resource Name is text.
Cost Object Name is a value type.
  The data type of Cost Object Name is text.
Offering Name is a value type.
  The data type of Offering Name is text.
Bill Id is a value type.
  The data type of Bill Id is text.
Bill Line Id is a value type.
  The data type of Bill Line Id is text.
Resource Id is a value type.
  The data type of Resource Id is text.
Line Number is a value type.
  The data type of Line Number is integer.
Quantity is a value type.
  The data type of Quantity is decimal.
Billing Unit is a value type.
  The data type of Billing Unit is text.
### The part of a whole a thing is given, from 0 to 1. The shares one thing is allocated by add to 1.
Share is a value type.
  The data type of Share is decimal.
### How many units of an offering were used in a period: calls made, trips driven, seats sold.
Usage Count is a value type.
  The data type of Usage Count is integer.

## Entity Types

Supplier(.Supplier Name) is an entity type.
### What a supplier sends for a billing period, under the id the business keeps it by (the supplier's own invoice
### number serves). A supplier may send several in a period.
Supplier Bill(.Bill Id) is an entity type.
Bill Line(.Bill Line Id) is an entity type.
### What a supplier bills for: a server, a database, a seat, a vehicle's fuel, a policy.
Supplier Resource(.Resource Id) is an entity type.
### What a cost is attributed to on its way to what is sold: a service, a vehicle, a route, a team.
Cost Object(.Cost Object Name) is an entity type.
### What the business sells, and so what its costs are finally attributed to.
Offering(.Offering Name) is an entity type.

## Fact Types

### The bill
Supplier Bill is from Supplier.
  Each Supplier Bill is from exactly one Supplier.
Supplier Bill covers Billing Period.
  Each Supplier Bill covers exactly one Billing Period.
Supplier Bill is in Currency.
  Each Supplier Bill is in at most one Currency.
### The total the bill states, which its lines should add up to.
Supplier Bill has stated- Amount.
  Each Supplier Bill has at most one stated- Amount.
Bill Line is on Supplier Bill.
  Each Bill Line is on exactly one Supplier Bill.
### Its place on the bill, as the bill numbers it.
Bill Line has Line Number.
  Each Bill Line has at most one Line Number.
Bill Line has Amount.
  Each Bill Line has exactly one Amount.
Bill Line has Quantity.
  Each Bill Line has at most one Quantity.
Bill Line has Billing Unit.
  Each Bill Line has at most one Billing Unit.
Bill Line has unit-price- Amount.
  Each Bill Line has at most one unit-price- Amount.
Bill Line is for Supplier Resource.
  Each Bill Line is for at most one Supplier Resource.

### The resource
Supplier Resource is supplied by Supplier.
  Each Supplier Resource is supplied by exactly one Supplier.
### What the supplier calls it.
Supplier Resource has Resource Name.
  Each Supplier Resource has at most one Resource Name.
### A resource no cost object is to carry, said rather than left to be inferred from the absence of an allocation.
Supplier Resource is unattributed.

### Allocation
Supplier Resource is allocated to Cost Object by Share.
  For each Supplier Resource and Cost Object, that Supplier Resource is allocated to that Cost Object by at most one Share.
Cost Object is allocated to Offering by Share.
  For each Cost Object and Offering, that Cost Object is allocated to that Offering by at most one Share.
### A cost object whose cost is split among its offerings as their usage in each period is: a service shared by the
### products that call it, a vehicle by the kinds of trip it drives.
Cost Object is allocated to Offering by usage.
  Each Cost Object, Offering combination occurs at most once in the population of Cost Object is allocated to Offering by usage.

### Usage. Counted where the business counts it (a meter, a trip log) and stated here for each period.
Offering has Usage Count for Billing Period.
  For each Offering and Billing Period, that Offering has at most one Usage Count for that Billing Period.

### What the bills come to
Supplier Bill has line- Amount. *
  Each Supplier Bill has at most one line- Amount.
### A bill whose lines do not add up to the total it states: a credit, a rounding or a line not recorded.
Supplier Bill is unreconciled. *
### What a supplier billed for a period: the totals its bills for that period state.
Supplier has period- Amount for Billing Period. *
  For each Supplier and Billing Period, that Supplier has at most one period- Amount for that Billing Period.
Supplier Resource has period- Amount for Billing Period. *
  For each Supplier Resource and Billing Period, that Supplier Resource has at most one period- Amount for that Billing Period.
### Each part a resource gives a cost object, and a cost object an offering, in a period: the cost of the giver
### times the share.
Supplier Resource gives allocated- Amount to Cost Object for Billing Period. *
  For each Supplier Resource, Cost Object and Billing Period, that Supplier Resource gives at most one allocated- Amount to that Cost Object for that Billing Period.
### The usage of all the offerings a cost object is allocated to by usage, and each one's share of it.
Cost Object has usage- Usage Count for Billing Period. *
  For each Cost Object and Billing Period, that Cost Object has at most one usage- Usage Count for that Billing Period.
Cost Object gives usage- Share to Offering for Billing Period. *
  For each Cost Object, Offering and Billing Period, that Cost Object gives at most one usage- Share to that Offering for that Billing Period.
Cost Object gives allocated- Amount to Offering for Billing Period. *
  For each Cost Object, Offering and Billing Period, that Cost Object gives at most one allocated- Amount to that Offering for that Billing Period.
Cost Object has period- Amount for Billing Period. *
  For each Cost Object and Billing Period, that Cost Object has at most one period- Amount for that Billing Period.
Offering has period- Amount for Billing Period. *
  For each Offering and Billing Period, that Offering has at most one period- Amount for that Billing Period.
Offering has unit- Amount for Billing Period. *
  For each Offering and Billing Period, that Offering has at most one unit- Amount for that Billing Period.

## Derivation Rules

### A variable is its type and subscript; a role name (period-, allocated-) is text of the reading it is in. Where one
### rule names two Amounts in one calculation they are told apart by subscript.
### A cost object's part to an offering is stated by two if rules, one for a share and one for usage: together they
### are all it is.

* Supplier Bill has line- Amount iff line- Amount is the sum of Amount where some Bill Line is on that Supplier Bill and that Bill Line has that Amount.

* Supplier Bill is unreconciled if that Supplier Bill has some stated- Amount1 and that Supplier Bill has some line- Amount2 where Amount1 < Amount2.

* Supplier Bill is unreconciled if that Supplier Bill has some stated- Amount1 and that Supplier Bill has some line- Amount2 where Amount1 > Amount2.

* Supplier has period- Amount for Billing Period iff period- Amount is the sum of Amount where some Supplier Bill is from that Supplier and that Supplier Bill covers that Billing Period and that Supplier Bill has that stated- Amount.

* Supplier Resource has period- Amount for Billing Period iff period- Amount is the sum of Amount where some Bill Line is for that Supplier Resource and that Bill Line has that Amount and that Bill Line is on some Supplier Bill and that Supplier Bill covers that Billing Period.

* Supplier Resource gives allocated- Amount1 to Cost Object for Billing Period iff that Supplier Resource is allocated to that Cost Object by some Share and that Supplier Resource has some period- Amount2 for that Billing Period and allocated- Amount1 is that period- Amount2 times that Share.

* Cost Object has period- Amount for Billing Period iff period- Amount is the sum of allocated- Amount where some Supplier Resource gives that allocated- Amount to that Cost Object for that Billing Period.

* Cost Object gives allocated- Amount1 to Offering for Billing Period if that Cost Object is allocated to that Offering by some Share and that Cost Object has some period- Amount2 for that Billing Period and allocated- Amount1 is that period- Amount2 times that Share.

* Cost Object has usage- Usage Count for Billing Period iff usage- Usage Count is the sum of Usage Count where that Cost Object is allocated to some Offering by usage and that Offering has that Usage Count for that Billing Period.

* Cost Object gives usage- Share to Offering for Billing Period iff that Cost Object is allocated to that Offering by usage and that Offering has some Usage Count1 for that Billing Period and that Cost Object has some usage- Usage Count2 for that Billing Period and usage- Share is that Usage Count1 divided by that Usage Count2.

* Cost Object gives allocated- Amount1 to Offering for Billing Period if that Cost Object gives some usage- Share to that Offering for that Billing Period and that Cost Object has some period- Amount2 for that Billing Period and allocated- Amount1 is that period- Amount2 times that usage- Share.

* Offering has period- Amount for Billing Period iff period- Amount is the sum of allocated- Amount where some Cost Object gives that allocated- Amount to that Offering for that Billing Period.

* Offering has unit- Amount1 for Billing Period iff that Offering has some period- Amount2 for that Billing Period and that Offering has some Usage Count for that Billing Period and unit- Amount1 is that period- Amount2 divided by that Usage Count.

## Deontic Constraints

### A bill whose lines do not add up to its stated total is shown, not refused: suppliers' bills carry credits and
### roundings, and someone decides what to record.
It is forbidden that some Supplier Bill is unreconciled.

### Every resource a supplier bills for is carried by some cost object, or is said to be carried by none.
It is obligatory that each Supplier Resource is allocated to some Cost Object by some Share unless that Supplier Resource is unattributed.

## Domain Metadata

Domain 'cost-attribution' has Description 'Supplier bills traced to what a business sells: each bill line is for a supplier resource, a resource is allocated to cost objects by share and a cost object to offerings by share or by usage, and an offering has a unit cost over the usage it had.'.
