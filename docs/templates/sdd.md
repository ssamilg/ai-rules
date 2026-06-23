# System Design Document (SDD)
**Feature / Project:** [name]
**Author:** [name]
**Date:** [DD-MM-YYYY]

## 1. System Architecture
- **Frontend:** [Framework/Library]
- **Backend:** [Framework/Library]
- **Database:** [Database Type/System]
- **Core Pattern:** [e.g., REST API with SPA, SSR, Event-Driven]

## 2. Database Schema (Data Access Layer)
Define all entities, exact field types, constraints, and relationships.

### [Entity Name]
- id: [Type, Primary Key]
- [field_name]: [Type, Nullable/Required, Default]
- Relations: [e.g., 1:N with User, FK: user_id]

## 3. API Contract (API Layer)
Define the strict input and output shapes for all endpoints.

### [GET/POST/PUT/DELETE] /api/v1/[resource]
- **Purpose:** [Single sentence description]
- **Auth Required:** [Yes/No, Required Role]

**Request Parameters/Body:**
{
  "field": "type"
}

**Response Format:**
{
  "data": {},
  "error": null
}

## 4. Business Logic (Service Layer)
Define the strict rules the backend must enforce before executing data transactions.
- **[Domain Model]:** [e.g., User cannot be deleted if active subscriptions > 0]
- **[Validation]:** [e.g., Passwords must be hashed via Argon2 before DB insertion]

## 5. Frontend State & Data Flow
Define how the client application manages data.
- **Server State:** [Fetching, caching, and invalidation strategy]
- **Client State:** [Global store definition vs local component state]
- **Routing:** [Layout wrappers and protected route logic]

## 6. Security & Infrastructure
- **Authentication:** [Token type, storage mechanism]
- **Authorization:** [Middleware implementation details]