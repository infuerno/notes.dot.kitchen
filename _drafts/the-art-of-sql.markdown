---
layout: post
title: The Art of SQL
---

## Chaper One - Laying Plans
>A good design is a design that doesn't require crazy queries.

Databases should typically be normalised to at least third normal form. Normalization is important. Without it data inconsistency and bloated data-entry and error handling code at the application level is a real risk. 

### Ensure Atomicity
Attributes should be **atomic**. Ensure attributes are broken down to enough granularity such that parts of attributes do not need to be referred to inside a where clause. e.g. `WHERE SUBSTRING(Postcode, 3) = 'SW1'` OR `WHERE Address LIKE '%London%'`. Without this, we lose two major benefits:

* Efficient search - regular indexes require atomic values as keys
* Data correctness - easy to insert incorrect data if e.g. a long description string holds keywords instead of several enums being defined

Always test components against the **business requirements** to determine what kind of atomicity is required. Too much / unnecessary atomicity can make data entry / maintenance more difficult.

## Identify the primary key
The primary key is/are the attribute(s) which can uniquely identify a row.

>As a general rule, you should, whenever possible, use a unique identifier that has meaning rather than some obscure sequential integer. I must stress that the primary key is what characterizes the data - which is not the case with some sequential identifier associated with each new row. You may choose to add such an identifier later. But a technical, numerical identifier doesn't constitute a real primary key by the mere virtue of its existence and mustn't be mistaken for the real thing.


