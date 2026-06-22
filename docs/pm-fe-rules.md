# Frontend Architecture Rules (PM Constraints)

**Context:** Use these rules alongside `tech-stack.md`, PRD, and wireframes to structure the frontend execution plan (`tasks.md`).

---

## 1. Architectural Boundaries
- **UI Layer:** Renders presentation and user interaction only.
- **State Layer:** Orchestrates remote data and global client state.
- **Service Layer:** Executes all external API calls.
- **Backend:** Owns all business rules, authorization, and persistence. Frontend handles UI validation only.

## 2. Structure & State
- Group architecture by feature.
- Separate "Smart" Container components (data fetching/wiring) from "Dumb" Presenter components (pure rendering).
- Use URL parameters as the single source of truth for filters, pagination, and navigation state.
- Keep shared UI primitives strictly decoupled from domain logic.

## 3. UI Task Requirements
Every data-driven feature mapped in `tasks.md` MUST explicitly include tasks for:
- Loading states.
- Error states.
- Empty states.
- Auth routing gates (if protected).

## 4. Required Task Generation Order
When generating step-by-step frontend tasks, enforce this exact sequence:
1. Define API Service Layer contracts.
2. Scaffold Feature Folder and local types.
3. Build State Layer / Data-fetching hooks.
4. Build Presenter components (UI primitives).
5. Build Container components (State/Service wiring).
6. Wire Route and apply Loading/Error/Empty states.