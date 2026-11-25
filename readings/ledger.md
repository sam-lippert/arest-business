# Ledger

### The business's books, kept by double entry. Every movement of value is one Ledger Entry: an amount one ledger
### account is debited and another credited, posted on a date and in a billing period, so the books balance by
### construction. A posting of several parts, as an invoice with tax on it, is several entries under one journal
### reference. A ledger account is of one kind (asset, liability, equity, revenue, expense) and may roll up into
### another. Its balance is derived from its entries and never kept: what it was debited less what it was credited
### for an asset or an expense, and the reverse for the rest. An entry is never changed; a mistake is undone by an
### entry that reverses it. An entry may name the document it records (an invoice, a credit note, a payment, a
### refund, a supplier's bill), and every issued invoice and every collected payment ought to be recorded by one.

## Cross-domain References

### Amount (from money)
### Billing Period (from money)
### Supplier Bill (from cost-attribution)
### Invoice (from receivables)
### Credit Note (from receivables)
### Payment (from receivables)
### Refund (from receivables)
### Date (from receivables)

## Value Types

Ledger Account Code is a value type.
  The data type of Ledger Account Code is text.
Ledger Account Name is a value type.
  The data type of Ledger Account Name is text.
Ledger Entry Id is a value type.
  The data type of Ledger Entry Id is text.
### What ties the entries of one posting together, as the number of the invoice it posts.
Journal Reference is a value type.
  The data type of Journal Reference is text.
Entry Memo is a value type.
  The data type of Entry Memo is text.
Account Kind is a value type.
  The possible values of Account Kind are 'asset', 'liability', 'equity', 'revenue', 'expense'.

## Entity Types

Ledger Account(.Ledger Account Code) is an entity type.
Ledger Entry(.Ledger Entry Id) is an entity type.

## Fact Types

### The chart of accounts
Ledger Account has Ledger Account Name.
  Each Ledger Account has at most one Ledger Account Name.
Ledger Account is of Account Kind.
  Each Ledger Account is of exactly one Account Kind.
### A sub-account and the account it is part of.
Ledger Account rolls up into Ledger Account.
  Each Ledger Account rolls up into at most one Ledger Account.
  Ledger Account rolls up into Ledger Account is acyclic.
### An asset or an expense grows by its debits; the other kinds grow by their credits.
Account Kind is normally debited.

### The entry
Ledger Entry moves Amount.
  Each Ledger Entry moves exactly one Amount.
Ledger Entry debits Ledger Account.
  Each Ledger Entry debits exactly one Ledger Account.
Ledger Entry credits Ledger Account.
  Each Ledger Entry credits exactly one Ledger Account.
Ledger Entry is posted on Date.
  Each Ledger Entry is posted on exactly one Date.
### The month the entry counts in.
Ledger Entry is posted in Billing Period.
  Each Ledger Entry is posted in exactly one Billing Period.
Ledger Entry is under Journal Reference.
  Each Ledger Entry is under at most one Journal Reference.
Ledger Entry has Entry Memo.
  Each Ledger Entry has at most one Entry Memo.
### A correction: the entry undoes an earlier one, which stays as it was.
Ledger Entry reverses Ledger Entry.
  Each Ledger Entry reverses at most one Ledger Entry.
  For each Ledger Entry, at most one Ledger Entry reverses that Ledger Entry.
  Ledger Entry reverses Ledger Entry is irreflexive.

### The document an entry records
Ledger Entry records Invoice.
  Each Ledger Entry records at most one Invoice.
Ledger Entry records Credit Note.
  Each Ledger Entry records at most one Credit Note.
Ledger Entry records Payment.
  Each Ledger Entry records at most one Payment.
Ledger Entry records Refund.
  Each Ledger Entry records at most one Refund.
Ledger Entry records Supplier Bill.
  Each Ledger Entry records at most one Supplier Bill.

### What follows from the entries
### What an entry does to an account: its amount on the account it debits, less than zero on the account it credits.
Ledger Entry moves net- Amount on Ledger Account. *
  For each Ledger Entry and Ledger Account, that Ledger Entry moves at most one net- Amount on that Ledger Account.
Ledger Account has net-debit- Amount for Billing Period. *
  For each Ledger Account and Billing Period, that Ledger Account has at most one net-debit- Amount for that Billing Period.
Ledger Account has net-debit- Amount. *
  Each Ledger Account has at most one net-debit- Amount.
### The balance on the side the account grows on.
Ledger Account has balance- Amount. *
  Each Ledger Account has at most one balance- Amount.

## Constraints

### No entry debits and credits one account.
For each Ledger Entry and Ledger Account, at most one of the following holds: that Ledger Entry debits that Ledger Account; that Ledger Entry credits that Ledger Account.

## Derivation Rules

* Ledger Entry moves net- Amount1 on Ledger Account iff that Ledger Entry debits that Ledger Account and that Ledger Entry moves that Amount1 or that Ledger Entry credits that Ledger Account and that Ledger Entry moves some Amount2 and net- Amount1 is 0 minus that Amount2.

* Ledger Account has net-debit- Amount1 for Billing Period iff net-debit- Amount1 is the sum of net- Amount2 where some Ledger Entry moves that net- Amount2 on that Ledger Account and that Ledger Entry is posted in that Billing Period.

* Ledger Account has net-debit- Amount1 iff net-debit- Amount1 is the sum of net- Amount2 where some Ledger Entry moves that net- Amount2 on that Ledger Account.

* Ledger Account has balance- Amount1 iff that Ledger Account is of some Account Kind and that Account Kind is normally debited and that Ledger Account has that net-debit- Amount1 or that Ledger Account is of some Account Kind and it is not true that that Account Kind is normally debited and that Ledger Account has some net-debit- Amount2 and balance- Amount1 is 0 minus that net-debit- Amount2.

## Deontic Constraints

### Every issued invoice is in the books.
It is obligatory that if some Invoice is issued on some Date then some Ledger Entry records that Invoice.
### Every collected payment is in the books.
It is obligatory that if some Payment is collected then some Ledger Entry records that Payment.

## Instance Facts

Account Kind 'asset' is normally debited.
Account Kind 'expense' is normally debited.

## Domain Metadata

Domain 'ledger' has Description 'The books by double entry: ledger accounts of five kinds rolling up into one another, ledger entries that debit one account and credit another, the documents they record, and each account''s balance derived from its entries, overall and by period.'.
