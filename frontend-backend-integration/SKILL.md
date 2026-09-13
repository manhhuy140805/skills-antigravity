---
name: frontend-backend-integration
description: >-
  Use this skill whenever the user asks to connect, integrate, consume, or
  implement Frontend features against an existing separate Backend API,
  including authentication, profiles, CRUD, uploads, search, pagination, and
  admin flows. Apply it even when the user does not name this skill. Treat the
  Backend API contract as read-only source of truth; do not use for Backend
  implementation.
---

# Frontend Backend Integration

Use this skill when the workspace has separate Frontend and Backend repositories and a task must consume an existing Backend API. It applies to authentication, users, profiles, products, admin, uploads, search, pagination, CRUD, and comparable Frontend-to-Backend integration work.

## Language

Respond in Vietnamese by default, including analysis, implementation plans, progress updates, blockers, follow-up items, and final reports. Keep API routes, HTTP methods, code identifiers, filenames, commands, and exact required status text unchanged. Use another language only when the user explicitly requests it.

## Non-negotiable boundary

The **Backend is read-only** and is the source of truth for the API contract. Inspect it as needed: routes/controllers, HTTP methods, DTOs and response DTOs, Swagger decorators, guards, services needed to establish behavior, enums, error responses, API prefix, headers/cookies, pagination/filtering, and token/session behavior.

Do not modify, format, refactor, or otherwise write Backend files. Do not create Backend migrations, alter contracts, implement missing Backend endpoints, or fix unrelated Backend defects. If the capability needed by the Frontend does not exist, identify it as a blocker or follow-up. Never change Backend merely to simplify Frontend work.

All implementation changes belong in the **Frontend** repository only.

## Phase 1 — Analysis and plan

Before writing code or modifying source files:

1. Locate both repositories, establishing which is Frontend and which is Backend.
2. Inspect the Frontend's `git status` and current branch. Do not alter either repository in this phase.
3. Use the Backend source to establish every relevant API detail; do not infer a contract that can be determined from source.
4. Assess the Frontend's architecture, folders, API client, services, types, schemas, forms, state management, authentication state when relevant, error/loading handling, routing, and reusable utilities/components. Follow the existing Frontend conventions.

Present the following plan, then **stop and wait for explicit user approval**. Do not create a branch or implement any source change until approval.

### Backend contract

For each relevant endpoint, state:

- HTTP method and route
- request and response DTOs
- authentication/guard requirement
- query and path parameters
- required headers and cookies
- expected status codes and important error responses

If Swagger and the Backend implementation disagree, identify the discrepancy. Do not silently choose a behavior or change Backend.

### Frontend assessment

Describe the existing integration-relevant conventions and reusable pieces found in the Frontend, including the items inspected above.

### Implementation plan

Specify the implementation sequence; files to create or modify and each responsibility; data and error flows; relevant security considerations; testing strategy; verification commands; and blockers or missing Backend capabilities.

### Git plan

Report the Frontend repository, current branch, proposed dedicated task branch, Backend repository, and an explicit confirmation that Backend will remain read-only. Use the task identifier supplied by the user; never invent a work-item identifier when one already exists.

## Phase 2 — Implementation

Begin only after explicit approval of the Phase 1 plan.

Before modifying Frontend code:

1. Recheck and understand the Frontend working-tree state.
2. Create or switch to a dedicated task branch according to repository conventions. Never implement on `main`, `master`, `develop`, or another shared/protected branch.
3. Verify the active branch.
4. Confirm that no Backend files were modified.

Implement narrowly inside the Frontend. Reuse existing abstractions and architecture, preserve type safety, and avoid unrelated refactors, speculative features, and unnecessary abstractions. Handle loading, success, and error states where applicable. Do not install a dependency unless needed; when practical, explain the need before adding one.

### Security-sensitive work

For authentication or other sensitive flows, inspect token storage, cookies and HttpOnly/Secure/SameSite behavior, Authorization headers, refresh/session flow, relevant CSRF implications, and sensitive response fields. Do not log or expose passwords, tokens, hashes, verification/reset tokens, or application secrets. Do not weaken Backend security to enable integration.

### Scope and verification

Document unrelated issues under **Follow-up** rather than fixing them. Make the smallest coherent change that satisfies the task.

After implementation, run the relevant available checks: lint, type-check, related tests, and production build. Report commands and actual results; if a command cannot run, say why. Never claim a check succeeded unless it completed successfully.

## Required final report

End every implementation with these sections:

## Summary

What was implemented.

## Files Changed

Created/modified Frontend files and their purposes.

## API Contract Used

Backend endpoints integrated.

## Verification

Commands executed and their results.

## Backend Integrity

Report exactly: `Backend modified: NO`. Check Backend `git status` when possible. If this task accidentally modified Backend, revert only changes introduced by this task before completing.

## Follow-up

Out-of-scope issues or missing Backend capabilities.

Read broadly. Plan before editing. Change narrowly. Verify completely. Report transparently.
