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

Traditional banking and financial apps often suffer from:
- Manual currency conversion delays.
- Lack of transparency in user transactions.
- Limited real-time logging and auditability.

**PeerFlow** solves this by:
- Allowing instant deposits (form-based).
- Enabling cross-currency transfers with exchange rate logic.
- Recording every transaction and balance change in real-time.

---

## 📸 Screenshots
- ✅ BPM(Business Process Modeling)
<img src="screenshots/PeerFlow.jpg" width="800px" height="400">

- ✅ ERD(Entity Relationship Diagram)
<img src="screenshots/ERD.jpg" width="800px" height="400">

- ✅ Conceptual Diagram
<img src="screenshots/conceptualDiagram.jpg" width="800px" height="400">

---
- ✅ Pluggable database creation

<img src="screenshots/PDB1.jpg" width="800px" height="400">

- continuation....
<img src="screenshots/PDB2.jpg" width="800px" height="400">

---

- ✅ All tables creations(DDL Commands with CREATE TABLE command)

- USERS: Stores user personal information such as full name, email, and phone number.
<img src="screenshots/USERStb_creation.jpg" width="800px" height="400">

- ACCOUNTS: Each user has one or more accounts in different currencies. Tracks balance and currency type.
<img src="screenshots/ACCOUNTStb_creation.jpg" width="800px" height="400">

- TRANSACTIONS: Records all deposit, withdrawal, and transfer actions along with timestamps and details.
<img src="screenshots/TRANSACTIONStb_creation.jpg" width="800px" height="400">

- EXCHANGERATES: Contains conversion rates between supported currencies (e.g., USD → FRW).
<img src="screenshots/EXCHANGERATEStb_creation.jpg" width="800px" height="400">

- CURRENCIES: Master list of all allowed currencies in the system (USD, EUR, FRW, KES).
<img src="screenshots/CURRENCIEStb_creation.jpg" width="800px" height="400">

- ACCOUNTS_LOG: Trigger-based audit log that captures every account balance change.
<img src="screenshots/ACCOUNTLOGStb_creation.jpg" width="800px" height="400">

---

- ✅ DML (data insertion in the tables)
  
- Inserting in Users
<img src="screenshots/users_insertion.jpg" width="800px" height="400">

- Inserting in Accounts
<img src="screenshots/accounts_insertion.jpg" width="800px" height="400">

---

- ✅ Packages testing screenshots

- Package for USER account creation
<img src="screenshots/package_user_creation.jpg" width="800px" height="400">

---

- ✅ Procedures testing screenshots

- Procedure for deposit money
<img src="screenshots/deposit_test.jpg" width="800px" height="400">

- Procedure for withdrawal money
<img src="screenshots/withdraw_test.jpg" width="800px" height="400">

- Procedure for transfering money
<img src="screenshots/transfer_test.jpg" width="800px" height="400">

- Procedure for Show Profile of User
<img src="screenshots/show_profile_test.jpg" width="800px" height="400">

---

- ✅ Trigger creation and where it logs changes

- Trigger creation
<img src="screenshots/trigger_creation.jpg" width="800px" height="400">

- where it logs changes in AACOUNTS_LOG Table
<img src="screenshots/where_trigger_logs_changes.jpg" width="800px" height="400">

---

### ✅🔐 PeerFlow – Table Constraints Overview

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

✅ **All PeerFlow tables store atomic values and have primary keys** → Satisfies 1NF.

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

**How PeerFlow Meets 3NF:**

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


