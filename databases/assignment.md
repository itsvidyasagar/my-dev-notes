# Postgres Capstone Project — Blueprint (Coder‑Readable, No Code)

Below is a **clean, concise, coder‑readable specification**. No SQL code, only naming, structure, and requirements. Use this as the single authoritative blueprint when you build the actual project.

---

# 1. SYSTEM OVERVIEW

A fintech‑style company database split into multiple **schemas** inside one PostgreSQL database. Each schema represents a domain. Cross‑schema references exist. Security is role-driven.

Schemas:

* `auth`
* `sales`
* `finance`
* `support`
* `hr`
* `audit`

---

# 2. GLOBAL DESIGN RULES

* All primary keys use `uuid` unless specified.
* All timestamps use `timestamptz`.
* All money fields use `numeric(18,4)`.
* Use enums for status/type fields.
* Foreign keys may cross schemas.
* `audit` schema is append‑only.
* Sensitive schemas (`finance`, `hr`) use restricted roles.

---

# 3. SCHEMA BLUEPRINTS (NO CODE)

## 3.1 SCHEMA: `auth`

### Tables

**users**

* user_id (uuid, pk)
* username (text, unique)
* email (text)
* created_at (timestamptz)

**roles**

* role_name (text, pk)

**user_roles**

* user_id (fk → auth.users)
* role_name (fk → auth.roles)

### Purpose

* Central user/role registry.
* Other schemas reference user_id for auditing.

---

## 3.2 SCHEMA: `sales`

### Enums

* order_status: draft, placed, shipped, cancelled

### Tables

**products**

* product_id (uuid, pk)
* sku (text, unique)
* name (text)
* price (numeric)
* active (boolean)

**sales_reps**

* rep_id (uuid, pk)
* full_name (text)
* email (text)

**orders**

* order_id (uuid, pk)
* customer_id (fk → finance.customers)
* sales_rep_id (fk → sales.sales_reps)
* status (order_status)
* placed_at (timestamptz)

### Purpose

* Sells products to finance customers.

---

## 3.3 SCHEMA: `finance`

### Enums

* account_type: checking, savings, credit
* tx_type: debit, credit, adjustment

### Tables

**customers**

* customer_id (uuid, pk)
* full_name (text)
* primary_email (text)

**accounts**

* account_id (uuid, pk)
* customer_id (fk → finance.customers)
* account_type
* balance (numeric)  *(updated by triggers/procedures)*

**transactions**  *(partitioned by month or year)*

* tx_id (bigint, pk)
* account_id (fk → finance.accounts)
* tx_type
* amount (numeric > 0)
* posted_at (timestamptz)
* balance_after (numeric) *(computed)*

**ledger**

* ledger_id (uuid, pk)
* tx_id (fk → finance.transactions)
* entry (jsonb)  *(record of posting)*

### Purpose

* Core financial engine.
* Referenced heavily across entire system.

---

## 3.4 SCHEMA: `support`

### Tables

**support_agents**

* agent_id (uuid, pk)
* full_name (text)
* email (text)

**tickets**

* ticket_id (uuid, pk)
* customer_id (fk → finance.customers)
* subject (text)
* status (text: open, closed, pending)
* created_at (timestamptz)

**ticket_messages**

* message_id (uuid, pk)
* ticket_id (fk → support.tickets)
* sender_type (enum: customer, agent)
* sender_id (uuid) *(customer_id or agent_id)*
* message (text)
* sent_at (timestamptz)

### Purpose

* Customer support system tied to finance customers.

---

## 3.5 SCHEMA: `hr`

### Tables

**employees**

* employee_id (uuid, pk)
* full_name (text)
* department (text)
* joined_at (timestamptz)

**payroll**

* payroll_id (uuid, pk)
* employee_id (fk → hr.employees)
* salary_amount (numeric)
* paid_at (timestamptz)

### Purpose

* Internal HR/payroll.

---

## 3.6 SCHEMA: `audit`

### Tables

**change_log** *(append‑only)*

* audit_id (bigint, pk)
* schema_name (text)
* table_name (text)
* record_id (text)
* action (text: insert, update, delete)
* changed_by (text)
* changed_at (timestamptz)
* payload (jsonb)

### Purpose

* Central change logging.
* Populated by triggers.

---

# 4. CORE REQUIREMENTS (NO SQL CODE)

## 4.1 Custom Types / Domains

* one enum per schema
* one domain for `email` (validated text)
* one domain for `money_positive` (numeric > 0)

## 4.2 Triggers

* `audit` trigger on all **finance** and **sales** tables
* BEFORE trigger on `transactions` to compute `balance_after`
* AFTER trigger to update `accounts.balance`

## 4.3 Functions (logic only)

* `post_transaction(account_id, amount, tx_type)` → inserts transaction, updates ledger
* `create_order(customer_id, product_list)` → creates order, calculates totals

## 4.4 Procedures

* `monthly_closure(year, month)` → aggregates financial data, locks period

## 4.5 Aggregates

* custom aggregate: running balance or weighted average

## 4.6 Materialized Views

* `mv_monthly_revenue`
* `mv_customer_value`

## 4.7 Index Requirements

* expression index on `lower(email)` fields
* partial index for `orders WHERE status = 'placed'`
* GIN index for jsonb in `ledger.entry`

## 4.8 Partitioning

* `transactions` partitioned by month or year

## 4.9 Security / Permissions

Roles:

* `role_sales`
* `role_finance`
* `role_support`
* `role_hr`
* `role_readonly`

Rules:

* cross‑schema FKs allowed
* SELECT blocked across domains unless allowed
* RLS on transactions: only account owners + finance

---

# 5. RELATION MAP (TEXT ER)

```
auth.users 1---* user_roles *---1 auth.roles

finance.customers 1---* finance.accounts 1---* finance.transactions *---1 finance.ledger
finance.customers 1---* sales.orders
finance.customers 1---* support.tickets

sales.sales_reps 1---* sales.orders
sales.products 1---* sales.orders (via order_items if you create it)

support.tickets 1---* support.ticket_messages

hr.employees 1---* hr.payroll
```

---

# 6. DELIVERABLES

You will implement:

* all schemas
* all tables
* all enums/domains
* all relationships
* all triggers and functions
* all roles and permissions
* indexes, partitions, mviews
* seed data
* README documentation

---

# 7. WHAT YOU SUBMIT

Folder structure:

```
/ddl
/docs
/functions
/procedures
/triggers
/tests
```

---

This is now a *pure blueprint*, fully coder‑readable, no code, no filler text.
