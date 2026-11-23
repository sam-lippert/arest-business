# Receivables

### What customers owe the business and what they have paid it. An Invoice bills one billing account in one currency
### for what its lines charge; a line charges an amount and may say for which offering, at which price, for how many
### units and for which period. A Credit Note takes part of an invoice back. A Payment is money a billing account
### sends: it is allocated to the invoice it is meant for, or to several, and an invoice may be paid by several. A
### payment is pending until it is reported collected or failed, and only one collected pays anything. A Refund
### returns part or all of a payment. Such documents are often kept by a payment processor, an external system that
### reports them under its own references; the facts it reports are these. What an invoice still asks for, and what
### an account owes, are derived from the documents and never kept as a balance. An invoice whose payments fail as
### often as the account's collection policy allows is delinquent: what the business then does is its own to say.

## Cross-domain References

Amount (from money)
Currency (from money)
Billing Period (from money)
Offering (from offerings)
Price (from offerings)
Quantity (from offerings)
Billing Account (from customers)
Day Count (from customers)
External System (from customers)
External Reference (from customers)

## Value Types

Invoice Id is a value type.
  The data type of Invoice Id is text.
Invoice Line Id is a value type.
  The data type of Invoice Line Id is text.
Credit Note Id is a value type.
  The data type of Credit Note Id is text.
Payment Id is a value type.
  The data type of Payment Id is text.
Refund Id is a value type.
  The data type of Refund Id is text.
Collection Policy Name is a value type.
  The data type of Collection Policy Name is text.
### A calendar day, as 2026-02-28.
Date is a value type.
  The data type of Date is date.
### What became of a payment. One with none is still pending.
Payment Outcome is a value type.
  The possible values of Payment Outcome are 'succeeded', 'failed'.
### Why a payment failed, as its processor says it: 'card declined', 'insufficient funds'.
Failure Reason is a value type.
  The data type of Failure Reason is text.
Attempt Count is a value type.
  The data type of Attempt Count is integer.

## Entity Types

Invoice(.Invoice Id) is an entity type.
Invoice Line(.Invoice Line Id) is an entity type.
Credit Note(.Credit Note Id) is an entity type.
Payment(.Payment Id) is an entity type.
Refund(.Refund Id) is an entity type.
### How patiently the business collects: how many failed payments of an invoice it takes and how many days past
### the due date it waits before it treats the invoice as delinquent.
Collection Policy(.Collection Policy Name) is an entity type.

## Fact Types

### An invoice, a payment or a refund that an external system reports is known by its reference before anything else of it
### is, so nothing else of one is mandatory here; a business that writes its own says what it must have.

### The invoice
Invoice is billed to Billing Account.
  Each Invoice is billed to at most one Billing Account.
Invoice is in Currency.
  Each Invoice is in at most one Currency.
### A draft has no issue date and asks for nothing yet.
Invoice is issued on Date.
  Each Invoice is issued on at most one Date.
### Stated by the issuer, or derived from the issue date and the account's payment terms. An account given terms has
### its invoices due by them; one whose invoices state their own due dates is given none.
Invoice is due on Date. +
  Each Invoice is due on at most one Date.
Invoice covers Billing Period.
  Each Invoice covers at most one Billing Period.
### The total the issuer states, which the lines should add up to.
Invoice has stated- Amount.
  Each Invoice has at most one stated- Amount.
### Cancelled: it asks for nothing.
Invoice is void.
### Given up as not collectable. What it still asks for is written off.
Invoice is written off.
Invoice is recorded in External System as External Reference.
  For each Invoice and External System, that Invoice is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Invoice is recorded in that External System as that External Reference.

### The line
Invoice Line is on Invoice.
  Each Invoice Line is on exactly one Invoice.
### Signed: a line that credits, as a proration does, is less than zero.
Invoice Line has Amount.
  Each Invoice Line has exactly one Amount.
Invoice Line is for Offering.
  Each Invoice Line is for at most one Offering.
Invoice Line charges Price.
  Each Invoice Line charges at most one Price.
Invoice Line has Quantity.
  Each Invoice Line has at most one Quantity.
Invoice Line covers Billing Period.
  Each Invoice Line covers at most one Billing Period.

### The credit note
Credit Note is against Invoice.
  Each Credit Note is against exactly one Invoice.
Credit Note has Amount.
  Each Credit Note has exactly one Amount.
Credit Note is issued on Date.
  Each Credit Note is issued on at most one Date.
Credit Note is recorded in External System as External Reference.
  For each Credit Note and External System, that Credit Note is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Credit Note is recorded in that External System as that External Reference.

### The payment
Payment is from Billing Account.
  Each Payment is from at most one Billing Account.
Payment has Amount.
  Each Payment has at most one Amount.
Payment is in Currency.
  Each Payment is in at most one Currency.
Payment is received on Date.
  Each Payment is received on at most one Date.
Payment has Payment Outcome.
  Each Payment has at most one Payment Outcome.
Payment has Failure Reason.
  Each Payment has at most one Failure Reason.
### The part of a payment meant for an invoice.
Payment is allocated to Invoice for Amount.
  For each Payment and Invoice, that Payment is allocated to that Invoice for at most one Amount.
Payment is recorded in External System as External Reference.
  For each Payment and External System, that Payment is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Payment is recorded in that External System as that External Reference.

### The refund
Refund is of Payment.
  Each Refund is of at most one Payment.
Refund has Amount.
  Each Refund has at most one Amount.
Refund is issued on Date.
  Each Refund is issued on at most one Date.
Refund is recorded in External System as External Reference.
  For each Refund and External System, that Refund is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Refund is recorded in that External System as that External Reference.

### Collection
Billing Account is under Collection Policy.
  Each Billing Account is under at most one Collection Policy.
### The failed payments of one invoice after which the invoice is delinquent.
Collection Policy gives up after Attempt Count.
  Each Collection Policy gives up after at most one Attempt Count.
### The days past its due date an invoice is given before collection is pressed.
Collection Policy allows grace- Day Count.
  Each Collection Policy allows at most one grace- Day Count.

### What follows from the documents
Payment is collected. *
Payment fails. *
Invoice has line- Amount. *
  Each Invoice has at most one line- Amount.
Invoice has credited- Amount. *
  Each Invoice has at most one credited- Amount.
Invoice has paid- Amount. *
  Each Invoice has at most one paid- Amount.
### What credits and collected payments have taken off the invoice.
Invoice has settled- Amount. *
  Each Invoice has at most one settled- Amount.
### What the invoice still asks for.
Invoice has due- Amount. *
  Each Invoice has at most one due- Amount.
Invoice is settled. *
### Issued, and neither settled, void nor written off.
Invoice is open. *
Invoice is out of balance. *
Invoice has failed- Attempt Count. *
  Each Invoice has at most one failed- Attempt Count.
Invoice is delinquent. *
### The last day collection waits for: the due date and the policy's grace.
Invoice has collection-deadline- Date. *
  Each Invoice has at most one collection-deadline- Date.
### What the account owes on its open invoices.
Billing Account has outstanding- Amount. *
  Each Billing Account has at most one outstanding- Amount.
Payment has allocated- Amount. *
  Each Payment has at most one allocated- Amount.
### What a collected payment has not been allocated to any invoice: a credit the account holds.
Payment has unallocated- Amount. *
  Each Payment has at most one unallocated- Amount.
Payment has refunded- Amount. *
  Each Payment has at most one refunded- Amount.
Payment is over-allocated. *
Payment is over-refunded. *
Refund is of an uncollected payment. *

## Constraints

### Only an invoice that was issued is voided or written off. A draft that is not wanted is deleted.
If some Invoice is void then that Invoice is issued on some Date.
If some Invoice is written off then that Invoice is issued on some Date.

## Derivation Rules

### A variable is its type and subscript; a role name (line-, paid-) is text of the reading it is in. A total that
### sums nothing is not there, so each difference says what it is when one side is missing.

+ Invoice is due on Date1 if that Invoice is issued on some Date2 and that Invoice is billed to some Billing Account and that Billing Account pays within some Day Count where Date1 = AddDays(Date2, Day Count).

* Payment is collected iff that Payment has Payment Outcome 'succeeded'.

* Payment fails iff that Payment has Payment Outcome 'failed'.

* Invoice has line- Amount iff line- Amount is the sum of Amount where some Invoice Line is on that Invoice and that Invoice Line has that Amount.

* Invoice has credited- Amount iff credited- Amount is the sum of Amount where some Credit Note is against that Invoice and that Credit Note has that Amount.

* Invoice has paid- Amount iff paid- Amount is the sum of Amount where some Payment is allocated to that Invoice for that Amount and that Payment is collected.

* Invoice has settled- Amount1 iff that Invoice has some paid- Amount2 and that Invoice has some credited- Amount3 and settled- Amount1 is that paid- Amount2 plus that credited- Amount3 or that Invoice has that paid- Amount1 and it is not true that that Invoice has some credited- Amount or that Invoice has that credited- Amount1 and it is not true that that Invoice has some paid- Amount.

* Invoice has due- Amount1 iff that Invoice has some line- Amount2 and that Invoice has some settled- Amount3 and due- Amount1 is that line- Amount2 minus that settled- Amount3 or that Invoice has that line- Amount1 and it is not true that that Invoice has some settled- Amount.

* Invoice is settled iff that Invoice is issued on some Date and that Invoice has some due- Amount and that due- Amount is at most 0.

* Invoice is open iff that Invoice is issued on some Date and it is not true that that Invoice is void and it is not true that that Invoice is written off and it is not true that that Invoice is settled.

* Invoice is out of balance iff that Invoice has some stated- Amount1 and that Invoice has some line- Amount2 and that stated- Amount1 is less than that line- Amount2 or that Invoice has some stated- Amount1 and that Invoice has some line- Amount2 and that line- Amount2 is less than that stated- Amount1.

* Invoice has failed- Attempt Count iff failed- Attempt Count is the count of Payment where that Payment is allocated to that Invoice for some Amount and that Payment fails.

* Invoice is delinquent iff that Invoice is open and that Invoice is billed to some Billing Account and that Billing Account is under some Collection Policy and that Collection Policy gives up after some Attempt Count1 and that Invoice has some failed- Attempt Count2 and that Attempt Count1 is at most that failed- Attempt Count2.

* Invoice has collection-deadline- Date1 iff that Invoice is due on some Date2 and that Invoice is billed to some Billing Account and that Billing Account is under some Collection Policy and that Collection Policy allows some grace- Day Count where Date1 = AddDays(Date2, Day Count).

* Billing Account has outstanding- Amount iff outstanding- Amount is the sum of due- Amount where some Invoice is billed to that Billing Account and that Invoice is open and that Invoice has that due- Amount.

* Payment has allocated- Amount iff allocated- Amount is the sum of Amount where that Payment is allocated to some Invoice for that Amount.

* Payment has unallocated- Amount1 iff that Payment is collected and that Payment has some Amount2 and that Payment has some allocated- Amount3 and unallocated- Amount1 is that Amount2 minus that allocated- Amount3 or that Payment is collected and that Payment has that Amount1 and it is not true that that Payment has some allocated- Amount.

* Payment has refunded- Amount iff refunded- Amount is the sum of Amount where some Refund is of that Payment and that Refund has that Amount.

* Payment is over-allocated iff that Payment has some Amount1 and that Payment has some allocated- Amount2 and that Amount1 is less than that allocated- Amount2.

* Payment is over-refunded iff that Payment has some Amount1 and that Payment has some refunded- Amount2 and that Amount1 is less than that refunded- Amount2.

* Refund is of an uncollected payment iff that Refund is of some Payment and it is not true that that Payment is collected.

## Deontic Constraints

### An account is invoiced in the currency it is billed in.
It is obligatory that if some Invoice is billed to some Billing Account and that Billing Account is billed in some Currency then that Invoice is in that Currency.
### A payment pays the invoices of the account it came from.
It is obligatory that if some Payment is allocated to some Invoice for some Amount and that Payment is from some Billing Account then that Invoice is billed to that Billing Account.
### Nothing is paid toward a cancelled invoice.
It is forbidden that some Payment is allocated to some Invoice for some Amount and that Invoice is void.
It is forbidden that some Invoice is out of balance.
It is forbidden that some Payment is over-allocated.
It is forbidden that some Payment is over-refunded.
It is forbidden that some Refund is of an uncollected payment.

## Instance Facts

### An invoice is a draft until it is issued, and open until its payments and credits settle it, it is voided or it
### is written off. One written off that is later paid is paid all the same, and one written off may still be voided.
### A draft is not voided: it was never sent, and is deleted.
State Machine Definition 'Invoice' is for Object Type 'Invoice'.
Status 'Draft' is defined in State Machine Definition 'Invoice'.
Status 'Open' is defined in State Machine Definition 'Invoice'.
Status 'Paid' is defined in State Machine Definition 'Invoice'.
Status 'Void' is defined in State Machine Definition 'Invoice'.
Status 'Uncollectible' is defined in State Machine Definition 'Invoice'.
Status 'Draft' is initial in State Machine Definition 'Invoice'.
Status 'Paid' is terminal in State Machine Definition 'Invoice'.
Status 'Void' is terminal in State Machine Definition 'Invoice'.

Transition 'issue-invoice' is defined in State Machine Definition 'Invoice'.
  Transition 'issue-invoice' is from Status 'Draft'.
  Transition 'issue-invoice' is to Status 'Open'.
  Transition 'issue-invoice' is triggered by Fact Type 'Invoice is issued on Date'.
Transition 'settle-invoice' is defined in State Machine Definition 'Invoice'.
  Transition 'settle-invoice' is from Status 'Open'.
  Transition 'settle-invoice' is to Status 'Paid'.
  Transition 'settle-invoice' is triggered by Fact Type 'Invoice is settled'.
Transition 'void-invoice' is defined in State Machine Definition 'Invoice'.
  Transition 'void-invoice' is from Status 'Open'.
  Transition 'void-invoice' is to Status 'Void'.
  Transition 'void-invoice' is triggered by Fact Type 'Invoice is void'.
Transition 'write-off-invoice' is defined in State Machine Definition 'Invoice'.
  Transition 'write-off-invoice' is from Status 'Open'.
  Transition 'write-off-invoice' is to Status 'Uncollectible'.
  Transition 'write-off-invoice' is triggered by Fact Type 'Invoice is written off'.
Transition 'recover-invoice' is defined in State Machine Definition 'Invoice'.
  Transition 'recover-invoice' is from Status 'Uncollectible'.
  Transition 'recover-invoice' is to Status 'Paid'.
  Transition 'recover-invoice' is triggered by Fact Type 'Invoice is settled'.
Transition 'void-uncollectible' is defined in State Machine Definition 'Invoice'.
  Transition 'void-uncollectible' is from Status 'Uncollectible'.
  Transition 'void-uncollectible' is to Status 'Void'.
  Transition 'void-uncollectible' is triggered by Fact Type 'Invoice is void'.

## Domain Metadata

Domain 'receivables' has Description 'What customers owe and pay: invoices and their lines, credit notes, payments allocated to invoices, refunds, and the collection policy by which an unpaid invoice becomes delinquent; what an invoice still asks for and what an account owes are derived.'.
