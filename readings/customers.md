# Customers

### Who a business sells to and whom it bills. A Customer is a party that buys: a person or an organization. A
### Billing Account is what the business bills: subscriptions, usage and invoices are an account's, and payments are
### received on it. A customer holds one billing account or several, one for each contract, program or budget it is
### billed under; each says what it stands for, in which currency it is billed, where its invoices go and how many
### days it is given to pay. The business may keep an account in an External System too (a payment processor, an
### accounting system) under that system's own reference, and what that system reports of the account is said here
### of the account it names. No system is named here: which one a business uses is its own fact.

## Cross-domain References

Currency (from money)

## Value Types

Customer Name is a value type.
  The data type of Customer Name is text.
Billing Account Code is a value type.
  The data type of Billing Account Code is text.
### What an account is billed under, in words: a contract, a program, a budget.
Billing Account Name is a value type.
  The data type of Billing Account Name is text.
Email Address is a value type.
  The data type of Email Address is text.
### A number of days: the days given to pay an invoice, the days of a trial.
Day Count is a value type.
  The data type of Day Count is integer.
### The name the business gives an external system it keeps records in.
System Name is a value type.
  The data type of System Name is text.
### What an external system calls a record of its own.
External Reference is a value type.
  The data type of External Reference is text.

## Entity Types

### How a customer is identified is the business's own to say: by an email address, by a number it gives, by a tax number.
Customer is an entity type.
Billing Account(.Billing Account Code) is an entity type.
### A system outside the business that keeps some of its records and reports facts about them: a payment processor,
### an accounting system, a marketplace.
External System(.System Name) is an entity type.

## Fact Types

### The customer
Customer has Customer Name.
  Each Customer has at most one Customer Name.

### The billing account
Billing Account belongs to Customer.
  Each Billing Account belongs to exactly one Customer.
Billing Account has Billing Account Name.
  Each Billing Account has at most one Billing Account Name.
Billing Account is billed in Currency.
  Each Billing Account is billed in at most one Currency.
### Where its invoices are sent.
Billing Account is invoiced at Email Address.
  Each Billing Account is invoiced at at most one Email Address.
### Its payment terms: the days from an invoice's issue to the day it is due (net 30).
Billing Account pays within Day Count.
  Each Billing Account pays within at most one Day Count.
### Takes no new subscriptions. What it owes is still owed.
Billing Account is closed.

### The account kept elsewhere. A reference names one account in its system, and an account has at most one
### reference in each system; a customer who signed up twice there holds two accounts here.
Billing Account is recorded in External System as External Reference.
  For each Billing Account and External System, that Billing Account is recorded in that External System as at most one External Reference.
  For each External System and External Reference, at most one Billing Account is recorded in that External System as that External Reference.

## Domain Metadata

Domain 'customers' has Description 'Who a business sells to and whom it bills: customers, the billing accounts each is billed under with their currency, invoice address and payment terms, and the references an external system such as a payment processor keeps them by.'.
