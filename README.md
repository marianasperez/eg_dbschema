# eg_dbschema
# Database Schema Diagram

## Overview
This repository contains the relational database schema for an online betting company. The schema represents the relationships between several tables that track users, games, bets, wallets, and related entities.

## Database Schema

The schema includes the following tables:

- **user**: Contains user information such as `id`, `name`, and `created_at`.
- **game**: Stores details about games, including the `id`, `name`, `provider`, and `created_at`.
- **bets**: Contains information about bets made by users on games, including the `user_id`, `game_id`, `value`, `multiplier`, `profit`, and `created_at`.
- **wallet**: Stores wallet details for users, including the `currency` and `created_at`.
- **exchange_rate**: Tracks exchange rates for various currencies, including `currency`, `rate`, and `created_at`.
- **deposit**: Tracks deposits made into wallets, including the `wallet_id`, `value`, and `created_at`.
- **user_kyc**: Contains the Know Your Customer (KYC) information for users, with fields like `user_id`, `kyc_level`, and `created_at`.
- **campaign**: Contains campaign data, with fields such as `id`, `name`, `user_id`, and `created_at`.
- **referral**: Tracks referral information, linking campaigns to referral users, with `campaign_id` and `referral_user_id`.

## Relationships

- **user** → **bets**: One-to-many relationship (One user can have multiple bets).
- **game** → **bets**: One-to-many relationship (One game can have multiple bets).
- **wallet** → **deposit**: One-to-many relationship (One wallet can have multiple deposits).
- **user** → **user_kyc**: One-to-one relationship (One user can have one KYC record).
- **user** → **campaign**: One-to-many relationship (One user can create multiple campaigns).
- **campaign** → **referral**: One-to-many relationship (One campaign can have multiple referrals).

## Design Decisions

- **Primary Keys**: Each table has a primary key (`id`) to uniquely identify each record.
- **Foreign Keys**: Foreign keys are used to establish relationships between tables, ensuring data integrity:
  - The `user_id` in the `bets` table references the `id` in the `user` table.
  - The `game_id` in the `bets` table references the `id` in the `game` table.
  - The `currency` in the `wallet` table references the `currency` in the `exchange_rate` table.
  - The `wallet_id` in the `deposit` table references the `id` in the `wallet` table.
  - The `user_id` in the `user_kyc` table references the `id` in the `user` table.
  - The `user_id` in the `campaign` table references the `id` in the `user` table.
  - The `campaign_id` in the `referral` table references the `id` in the `campaign` table, and `referral_user_id` references the `id` in the `user` table.

## Assumptions

1. **Data Types**:
   - Fields like `value`, `multiplier`, and `profit` in the `bets` table are assumed to be decimal because they likely involve monetary values.
   - `currency` in the `wallet` and `exchange_rate` tables is assumed to be `varchar` to accommodate different currency codes like "USD" or "AUD".
   - `created_at` is assumed to be `datetime` for all tables to consistently track record creation timestamps.

2. **Standalone Tables**:
   - The `wallet`, `deposit`, and `exchange_rate` tables are standalone, as no link to the `user` table was provided.
   - It is assumed that these tables serve a separate financial module that can operate independently.

## Chosen System: dbdiagram.io

For the database schema diagram, **dbdiagram.io** was chosen for the following reasons:

1. **Ease of Use**:
   - dbdiagram.io provides an intuitive, user-friendly interface that allows users to quickly visualise and design database schemas. It’s simple to create tables, add columns, define relationships, and export the schema.
   
2. **Support for DBML**:
   - dbdiagram.io uses **DBML (Database Markup Language)**, which is a lightweight, text-based markup language that makes defining database structures quick and readable. It’s ideal for generating, editing, and sharing database schemas.

3. **Quick Collaboration**:
   - The platform allows easy sharing of the diagrams via links, making it possible to collaborate and review the schema with teammates or stakeholders.
   
4. **Visual Representation**:
   - dbdiagram.io automatically generates visual diagrams from the DBML code, making it easy to understand and communicate the schema’s structure. This visualisation is particularly helpful when explaining the relationships between tables.

5. **Free and Accessible**:
   - dbdiagram.io is free to use with essential features, and it doesn’t require any installation or complex setup, making it highly accessible for users working on quick projects or challenges like this one.

## Diagram

The diagram under database_schema.pdf shows the visual representation of the database schema.

## DBML Code

You can check the code used for the schema creation under DBML_database_schema

## Potential Improvements

1. **Link Between Wallet and User**:
   - Adding a `user_id` foreign key to the `wallet` table would enable tracking which user owns a specific wallet.
   - This connection could improve reporting on user finances and enable better integration with other tables like `bets` or `campaign`.

   **Reason for Not Adding**:
   - The provided schema does not include this relationship, possibly to keep the financial system modular and separate.

2. **Normalization of Exchange Rates**:
   - If exchange rates change frequently, an additional table could track historical rates by including `effective_from` and `effective_to` timestamps.
   - This would allow for tracking exchange rate changes over time and ensure historical accuracy.

3. **Data Validation**:
   - Adding constraints (e.g., ensuring positive values for `value` in `deposit` and `bets`) can improve data integrity and ensure that invalid or out-of-range data does not enter the system.

