# Backend Architecture Rules (PM Constraints)

**Context:** Use these rules alongside `tech-stack.md`, PRD, and wireframes to structure the backend execution plan (`tasks.md`).

---

## 1. Architectural Boundaries
- **API Layer:** Handles HTTP routing, strict input validation, and response mapping only.
- **Service Layer:** Owns all business logic, orchestration, and domain errors.
- **Data Access Layer:** Owns all database queries, transactions, and entity mapping.
- **Middleware:** Handles cross-cutting concerns (authentication, logging, rate limiting, global error catching).

## 2. API Contract & Data Flow
- Define the complete API contract before starting endpoint implementation tasks.
- Validate every request body and query parameter at the API entry point.
- Wrap all responses in a standardized envelope (`data`, `error`, `meta`, `request_id`).
- Map Service Layer domain errors to specific HTTP status codes within the API Layer.

## 3. Database & Security
- Execute all schema changes via explicit migration tasks.
- Enforce authorization middleware on all protected endpoints.
- Execute object-level ownership and permission checks strictly within the Service Layer.
- Wrap multi-step database writes in transaction tasks.

## 4. Required Task Generation Order
When generating step-by-step backend tasks, enforce this exact sequence:
1. Write ADR and define the API contract.
2. Define Database Schemas and create Migration/Seed tasks.
3. Build Data Access Layer (queries and transactions).
4. Build Service Layer (business logic and third-party integrations).
5. Build API Layer (routes, validation schemas, and error mapping).
6. Wire Auth and Middleware.