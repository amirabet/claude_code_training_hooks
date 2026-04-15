# E-Commerce Data Utilities

> **Learning project for the [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) course by Anthropic.**  


A TypeScript utility library providing query functions for a SQLite-backed e-commerce database. Designed to run as a daily cron job for automated order monitoring and alerting.

## Overview

This project:
- Initializes and manages a SQLite database schema for a full e-commerce system
- Provides modular, Promise-based query functions organized by domain
- Runs daily via cron to monitor pending orders and send Slack alerts to `#order-alerts`

## Requirements

- Node.js (v18+)
- npm

## Setup

```bash
npm run setup
```

This installs dependencies and runs initialization scripts.

## Project Structure

```
src/
  main.ts              # Entry point — initializes DB and runs daily job
  schema.ts            # Database schema creation
  queries/
    customer_queries.ts    # Customer lookups and management
    product_queries.ts     # Product catalog queries
    order_queries.ts       # Order management queries
    analytics_queries.ts   # Reporting and analytics
    inventory_queries.ts   # Inventory management
    promotion_queries.ts   # Promotions and discounts
    review_queries.ts      # Product reviews
    shipping_queries.ts    # Shipping and fulfillment
hooks/                 # Claude Code hooks (query validation, type checking)
scripts/               # Initialization scripts
```

## Database Schema

The SQLite database (`ecommerce.db`) contains tables for a complete e-commerce system:

| Domain | Tables |
|---|---|
| Customers | `customers`, `addresses`, `customer_segments`, `customer_activity_log` |
| Products | `products`, `categories`, `inventory`, `warehouses` |
| Orders | `orders`, `order_items` |
| Engagement | `reviews`, `promotions` |

## Writing Queries

All query functions must be placed in `src/queries/`. Functions return Promises and follow this pattern:

```typescript
import { Database } from "sqlite";

export function getCustomerByEmail(db: Database, email: string): Promise<any> {
  const query = `SELECT * FROM customers WHERE email = ?`;
  return new Promise((resolve, reject) => {
    db.get(query, [email], (err, row) => {
      if (err) reject(err);
      else resolve(row);
    });
  });
}
```

- Use `db.get()` for single-record queries
- Use `db.all()` for multi-record queries
- Always use parameterized queries (`?` placeholders) — never interpolate user input directly into SQL

## Tech Stack

- **TypeScript** — strict typing throughout
- **sqlite / sqlite3** — database driver
- **tsx** — TypeScript execution without a build step
- **@anthropic-ai/claude-agent-sdk** — Claude agent integration
