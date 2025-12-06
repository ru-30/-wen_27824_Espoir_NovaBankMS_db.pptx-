# NovaBank System - Design Decisions

## 1. Overview
This document explains the major design decisions made during the development of the PeerFlow banking system. It includes database design choices, procedure/package design, transaction handling, auditing, and multi-currency support.

---

## 2. Database Design Decisions

### 2.1 Relational Database
- **Choice:** Oracle RDBMS with PL/SQL procedures.
- **Rationale:** 
  - Supports strong transactional integrity.
  - Enables use of triggers, packages, and procedural logic.
  - Facilitates multi-user concurrent access with ACID compliance.

### 2.2 Users Table
- **Design:** Stores user personal information (ID, name, email, phone).
- **Decision:** Email is unique to avoid duplicate users.
- **Rationale:** Ensures each user has a unique identity; simplifies login/authentication.

### 2.3 Accounts Table
- **Design:** Each user has one or more accounts.
- **Decision:** Include currency_code and balance.
- **Rationale:** Supports multi-currency accounts and facilitates financial operations per account.

### 2.4 Transactions Table
- **Design:** Stores all deposits, withdrawals, and transfers.
- **Decision:** Include transaction_type and optional details.
- **Rationale:** Provides a detailed audit trail for user operations.

### 2.5 Currencies & ExchangeRates Tables
- **Design:** Separate tables for currency definitions and exchange rates.
- **Decision:** Use 3-letter ISO codes for currency_code.
- **Rationale:** Standardization of currency data; enables multi-currency transactions and future scalability.

### 2.6 Accounts Log Table
- **Design:** Captures account changes using triggers.
- **Decision:** Logs old/new balances and change type.
- **Rationale:** Enables auditing and regulatory compliance; ensures historical data is preserved.

---

## 3. Package & Procedure Design

### 3.1 User Account Package
- **Function:** `create_user_with_account_fn`
- **Procedure:** `create_user_with_account_proc`
- **Decision:** Use package to encapsulate user/account creation.
- **Rationale:** Promotes code reuse, simplifies API for creating users with accounts, ensures consistent messaging.

### 3.2 Transaction Procedures
- **Deposit/Withdraw/Transfer**
- **Decision:** Separate procedures for each transaction type.
- **Rationale:** Clear separation of responsibilities; allows validation and logging specific to each operation.

### 3.3 User Profile Procedure
- **Design:** `show_user_profile(p_user_id)`
- **Decision:** Joins `Users`, `Accounts`, and `Transactions` tables.
- **Rationale:** Single procedure to provide user summary and transaction history; improves maintainability.

---

## 4. Triggers

### 4.1 Account Balance Trigger
- **Trigger:** `trg_log_account_changes`
- **Decision:** AFTER UPDATE on `Accounts.balance`.
- **Rationale:** Automatically logs balance changes for auditing; ensures no manual intervention required.

### 4.2 Context-Sensitive Logging
- **Decision:** Uses `SYS_CONTEXT('peerflow_ctx', 'change_type')` to capture type of change.
- **Rationale:** Provides flexible, accurate tracking of transaction type in audit logs.

---

## 5. Transaction Management

- **Decision:** All operations wrapped with validations and committed only after success.
- **Rationale:** Ensures ACID compliance:
  - Atomicity: no partial updates
  - Consistency: database remains in valid state
  - Isolation: concurrent operations handled correctly
  - Durability: committed changes persist

---

## 6. Multi-Currency Support

- **Decision:** Include `currency_code` in Accounts and use `ExchangeRates` for conversion.
- **Rationale:** Facilitates international transactions; allows system to scale to multiple currencies without redesign.

---

## 7. Error Handling

- **Decision:** Procedures use `RAISE_APPLICATION_ERROR` for invalid operations (e.g., insufficient balance, negative deposits).
- **Rationale:** Provides clear error messages; prevents invalid data entry.

---

## 8. Console Logging

- **Decision:** Use `DBMS_OUTPUT.PUT_LINE` to provide user-friendly transaction messages.
- **Rationale:** Enhances debugging and usability in console/PLSQL Developer sessions.

---

## 9. Security and Integrity

- **Decision:** Use foreign keys for all relational links (Users → Accounts → Transactions → Currencies).
- **Rationale:** Ensures referential integrity; prevents orphaned records.

- **Decision:** Trigger-based auditing instead of manual logging.
- **Rationale:** Guarantees all balance changes are recorded without relying on application logic.
---
## 10. Summary of Key Decisions

| Component                  | Design Decision                                        | Rationale                                           |
|-----------------------------|-------------------------------------------------------|---------------------------------------------------|
| Database Type               | Oracle RDBMS with PL/SQL                              | Transaction integrity & procedural support       |
| User Identification         | Unique email                                         | Prevent duplicates & simplify login             |
| Account Structure           | Balance + currency_code                               | Supports multi-currency & financial operations  |
| Transactions Logging        | Separate Transactions table                           | Audit trail & transparency                        |
| Audit Logging               | Accounts_Log via triggers                              | Automatic & reliable audit                        |
| Packages                    | Encapsulate creation & logic                           | Reuse, maintainability                            |
| Error Handling              | Raise_application_error                                | Prevent invalid operations                        |
| Multi-Currency Handling     | Currency_code + ExchangeRates table                   | International support & scalability              |
| Console Feedback            | DBMS_OUTPUT                                           | User-friendly messaging & debugging               |

---
This design emphasizes **data integrity, auditability, modularity, and multi-currency flexibility**, providing a robust foundation for the PeerFlow banking system.
