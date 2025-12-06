# NovaBank – Peer-to-Peer Currency Transfer System

An Oracle-based backend project that enables users to deposit, withdraw, and transfer money across multiple currencies (FRW, USD, EUR, KES) with automatic currency conversion and full transaction auditing.

---

## 👥 Identity

| Name             | Student ID     |
|------------------|----------------|
| MUGISHA Espoir   | 27824          |

---

## 🧠 Problem Statement

Managing peer-to-peer transactions across different currencies is challenging. Users often struggle with manual conversions, high transfer fees, and delays in traditional banking systems.

## Key Objectives

Traditional banking and financial apps often suffer from:
- Manual currency conversion delays.
- Lack of transparency in user transactions.
- Limited real-time logging and auditability.

**NovaBank** solves this by:
- Allowing instant deposits (form-based).
- Enabling cross-currency transfers with exchange rate logic.
- Recording every transaction and balance change in real-time.

---

## Quick start instructionS

## 1. Requirements
Before running the NovaBank project, ensure the following tools are installed:

Oracle Database 21c (XE or Standard)

SQL Developer 23+

A working Pluggable Database (PDB)

## 2. Start Oracle Database Services
start them using Command Prompt:

net start OracleServiceXE

net start OracleXETNSListener

## 3. Connect to the Database in SQL Developer
Open SQL Developer

Create a new connection and fill in these fields:

(Connection Name, Username, Password, Service Name'use your PDB database you have created')

Click Test → ensure status is Success

Click Connect


## 4. Load the NovaBank Database Schema

Once connected:

Open the SQL file:
NovaBank_Schema.sql
(contains tables, sequences, triggers, and sample data)

Run the script using:
(F5) Run Script

Confirm that all objects are created successfully:

Tables: ACCOUNTS, CUSTOMERS, TRANSACTIONS, etc.

Sequences for primary keys

Triggers for logging activities

## 5. Run NovaBank Queries

After importing the schema, you can execute any NovaBank operations, such as:

SELECT * FROM customers;
SELECT * FROM accounts;

## 6. (Optional) Switch to NovaBank PDB

If starting in CDB$ROOT, switch to the correct PDB:

ALTER SESSION SET CONTAINER = WEN_27824_ESPOIR_NOVABANK_FINANCIALSYSTEM_DB;
---

### ✅🔐 NovaBank – Table Constraints Overview

| **Table**         | **Attribute**          | **Constraint**                                             |
|-------------------|------------------------|------------------------------------------------------------|
| `USERS`           | `user_id`              | `PRIMARY KEY`, `AUTO INCREMENT`                            |
| `USERS`           | `email`                | `UNIQUE`, `NOT NULL`                                       |
| `ACCOUNTS`        | `account_id`           | `PRIMARY KEY`, `AUTO INCREMENT`                            |
| `ACCOUNTS`        | `user_id`              | `FOREIGN KEY REFERENCES USERS(user_id)`                    |
| `ACCOUNTS`        | `currency_code`        | `FOREIGN KEY REFERENCES CURRENCIES(currency_code)`         |
| `ACCOUNTS`        | `balance`              | `CHECK (balance >= 0)`                                     |
| `TRANSACTIONS`    | `transaction_id`       | `PRIMARY KEY`, `AUTO INCREMENT`                            |
| `TRANSACTIONS`    | `account_id`           | `FOREIGN KEY REFERENCES ACCOUNTS(account_id)`              |
| `TRANSACTIONS`    | `transaction_type`     | `CHECK (IN ('deposit', 'withdraw', 'transfer_in', 'transfer_out'))` |
| `TRANSACTIONS`    | `amount`               | `CHECK (amount > 0)`                                       |
| `EXCHANGERATES`   | `rate_id`              | `PRIMARY KEY`, `AUTO INCREMENT`                            |
| `EXCHANGERATES`   | `from_currency`        | `FOREIGN KEY REFERENCES CURRENCIES(currency_code)`         |
| `EXCHANGERATES`   | `to_currency`          | `FOREIGN KEY REFERENCES CURRENCIES(currency_code)`         |
| `EXCHANGERATES`   | `rate`                 | `CHECK (rate > 0)`                                         |
| `CURRENCIES`      | `currency_code`        | `PRIMARY KEY`                                              |
| `CURRENCIES`      | `name`                 | `UNIQUE`, `NOT NULL`                                       |
| `ACCOUNTS_LOG`    | `log_id`               | `PRIMARY KEY`, `AUTO INCREMENT`                            |
| `ACCOUNTS_LOG`    | `account_id`           | `FOREIGN KEY REFERENCES ACCOUNTS(account_id)`              |
| `ACCOUNTS_LOG`    | `change_type`          | `CHECK (IN ('deposit', 'withdraw', 'transfer_in', 'transfer_out'))` |

---

### ✅ NORMALIZATION
This section explains how the PeerFlow database satisfies the three main stages of normalization: **1NF**, **2NF**, and **3NF**.

### ✅ First Normal Form (1NF)

**Definition:**  
- All values must be **atomic** (no repeating groups or arrays).
- Each record must be **uniquely identifiable**.

**How PeerFlow Meets 1NF:**
- `USERS` stores one name, email, and phone per row.
- `ACCOUNTS` uses one row per currency/account.
- `TRANSACTIONS` stores one row per transaction.
- `EXCHANGERATES` contains one row per currency pair.
- `ACCOUNTS_LOG` logs one row per balance update.

✅ **All NovaBank tables store atomic values and have primary keys** → Satisfies 1NF.

---

### ✅ Second Normal Form (2NF)

**Definition:**  
- Must be in 1NF.
- No **partial dependency**: Every non-key column must depend on the **entire** primary key.

**How PeerFlow Meets 2NF:**
- No table uses a composite primary key — all use simple, auto-incremented IDs.
- All columns in each table depend on their full primary key (e.g., `transaction_id`, `account_id`).

✅ **All non-key attributes depend fully on the primary key** → Satisfies 2NF.

---

### ✅ Third Normal Form (3NF)

**Definition:**  
- Must be in 2NF.
- No **transitive dependencies**: non-key attributes should not depend on other non-key attributes.

**How NovaBank Meets 3NF:**

| Table           | Explanation |
|------------------|-------------|
| `USERS`          | `email`, `phone` → depend only on `user_id`. |
| `ACCOUNTS`       | `currency_code` references `CURRENCIES`, not duplicated. |
| `TRANSACTIONS`   | `amount`, `type`, `details` → depend on `transaction_id`. |
| `EXCHANGERATES`  | `rate` depends only on `rate_id`. |
| `ACCOUNTS_LOG`   | `change_type`, `old_balance`, `new_balance` → all depend on `log_id`. |

✅ **No attribute depends on a non-key attribute** → Satisfies 3NF.

---

### 📊 Summary

| Normal Form | Description                                      | Satisfied? |
|-------------|--------------------------------------------------|------------|
| **1NF**     | Atomic fields, no repeating groups               | ✅ Yes     |
| **2NF**     | Full functional dependency on primary key        | ✅ Yes     |
| **3NF**     | No transitive dependencies on non-key attributes | ✅ Yes     |

---



# END THANK YOU!!!


