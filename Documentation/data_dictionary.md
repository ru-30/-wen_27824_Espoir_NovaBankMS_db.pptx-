# NovaBank Data Dictionary

This data dictionary describes all entities, attributes, data types, keys, and relationships used in the NovaBank Oracle 21c database.

---

## 🟦 1. USERS

**Table Name:** `USERS`

| Attribute  | Data Type     | Description                   | Constraints      |
| ---------- | ------------- | ----------------------------- | ---------------- |
| user_id    | NUMBER        | Unique ID for each user       | Primary Key      |
| full_name  | VARCHAR2(100) | Full name of the user         | NOT NULL         |
| email      | VARCHAR2(100) | Email address                 | UNIQUE, NOT NULL |
| phone      | VARCHAR2(20)  | Phone number                  | NULL             |
| created_at | DATE          | Date user account was created | DEFAULT SYSDATE  |

**Description:** Stores personal details of all system users.

---

## 🟦 2. CURRENCIES

**Table Name:** `CURRENCIES`

| Attribute     | Data Type    | Description                        | Constraints |
| ------------- | ------------ | ---------------------------------- | ----------- |
| currency_code | CHAR(3)      | ISO currency code (e.g., USD, RWF) | Primary Key |
| name          | VARCHAR2(50) | Name of the currency               | NOT NULL    |
| symbol        | VARCHAR2(5)  | Currency symbol                    | NULL        |

**Description:** Contains supported currencies for accounts and exchange processes.

---

## 🟦 3. ACCOUNTS

**Table Name:** `ACCOUNTS`

| Attribute     | Data Type    | Description                    | Constraints                             |
| ------------- | ------------ | ------------------------------ | --------------------------------------- |
| account_id    | NUMBER       | Unique bank account identifier | Primary Key                             |
| user_id       | NUMBER       | Owner of the account           | Foreign Key → USERS(user_id)            |
| balance       | NUMBER(15,2) | Current account balance        | DEFAULT 0                               |
| currency_code | CHAR(3)      | Type of currency used          | Foreign Key → CURRENCIES(currency_code) |
| created_at    | DATE         | Account creation date          | DEFAULT SYSDATE                         |

**Description:** Each record represents a financial account belonging to a user.

---

## 🟦 4. TRANSACTIONS

**Table Name:** `TRANSACTIONS`

| Attribute        | Data Type     | Description                           | Constraints                        |
| ---------------- | ------------- | ------------------------------------- | ---------------------------------- |
| transaction_id   | NUMBER        | Unique transaction identifier         | Primary Key                        |
| account_id       | NUMBER        | Account linked to this transaction    | Foreign Key → ACCOUNTS(account_id) |
| transaction_type | VARCHAR2(20)  | Transaction type (DEPOSIT/WITHDRAWAL) | NOT NULL                           |
| amount           | NUMBER(15,2)  | Amount of money moved                 | NOT NULL                           |
| transaction_date | DATE          | Date and time of the transaction      | DEFAULT SYSDATE                    |
| details          | VARCHAR2(255) | Additional notes                      | NULL                               |

**Description:** Captures all deposits and withdrawals performed on accounts.

---

## 🟦 5. EXCHANGERATES

**Table Name:** `EXCHANGERATES`

| Attribute     | Data Type    | Description                               | Constraints                             |
| ------------- | ------------ | ----------------------------------------- | --------------------------------------- |
| rate_id       | NUMBER       | Unique identifier for the exchange record | Primary Key                             |
| from_currency | CHAR(3)      | Base currency                             | Foreign Key → CURRENCIES(currency_code) |
| to_currency   | CHAR(3)      | Target currency                           | Foreign Key → CURRENCIES(currency_code) |
| rate          | NUMBER(15,6) | Conversion rate                           | NOT NULL                                |
| updated_at    | DATE         | Last updated timestamp                    | DEFAULT SYSDATE                         |

**Description:** Maintains conversion rates between supported currencies.

---

# 🟧 Relationship Summary

* **USERS (1) → (∞) ACCOUNTS**
* **CURRENCIES (1) → (∞) ACCOUNTS**
* **ACCOUNTS (1) → (∞) TRANSACTIONS**
* **CURRENCIES (1) → (∞) EXCHANGERATES (as from_currency & to_currency)**

