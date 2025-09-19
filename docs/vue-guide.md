# Vue Guide

This document defines the structure and rules for building Vue 3 components using Inertia.js and TailwindCSS. It mirrors the backend architecture and ensures clean separation of concerns, modular design, and long-term maintainability.

---

## Core Principles

- Pages only coordinate — no logic or bloat.
- All logic is extracted to composables or services.
- Components are atomic, reusable, and scoped.
- **Avoid `watch`/`watchEffect` unless absolutely necessary.** Prefer `computed` properties or alternative solutions. Watchers are a last resort for handling side effects that cannot be expressed declaratively.
- **Prefer reuse before creating new code.** Always look for existing utilities, formatters, and parsers before implementing new ones. If a helper looks broadly applicable across multiple domains, implement it as a new utility instead of embedding logic directly inside a single component.

---

## Structure Overview

### Directories

    resources/
    ├── assets/ 
    │   └── css/
    │   └── images/
    ├── js/
    │   └── pages/
    │   └── components/
    │   └── composables/
    │   └── services/
    │   └── enums/
    │   └── utils/
    ├── tests/
    │   └── components/
    │   └── composables/
    │   └── services/
    │   └── utils/
    └── views/

### Pages

- One file per route.
- Receives props from Inertia.
- Delegates everything to components, composables, or layouts.
- Never contains logic, mutation, or reusable markup.

### Components

Organized into subfolders:
- `Layouts/` – App and auth wrappers (`AppLayout.vue`).
- `{domain}/` – Reusable, grouped form blocks (`UserForm.vue`).
- Always use composition api `<script setup>`.

> Components are purely presentational. No logic or heavy state. When using state, try to keep local, but prefer composables.

### Composables

- Named like `useXxx.js` (e.g., `useModal`, `usePagination`).
- Extract reusable logic from components/pages.
- Must be stateless and scoped.

### Services

- Optional layer for wrapping Inertia visits, API helpers, or uploaders.
- Encapsulates external data interactions.

### Enums

- Provide a single source of truth for shared constants (e.g., statuses, roles) so the front-end matches Laravel's enums without magic values.
- Auto-generated from the backend using `php artisan atlas:export-enums`. See the [Enum Exporter docs](https://github.com/tmarois/atlas-laravel/blob/main/docs/features/enum-exporter.md).

### Utils

- Stateless helpers (`formatDate`, `slugify`).
- No app state, DOM access, or side effects.
- **Always prefer existing utilities before adding new ones.**
- If a helper is domain-agnostic and potentially reusable, promote it to a utility instead of leaving it as an inline function inside a component or composable.

---

## Naming Conventions

| Layer       | Format     | Example             |
|-------------|------------|---------------------|
| Pages       | PascalCase | `Users/Index.vue`   |
| Components  | PascalCase | `UserForm.vue`      |
| Composables | camelCase  | `useModal.js`       |
| Services    | camelCase  | `api.js`            |
| Utils       | camelCase  | `formatDate.js`     |

---

## Best Practices

- Keep components under 300 lines when possible.
- All state-related logic must go into composables.
- Avoid deeply nested components — flatten where possible.
- Use Tailwind for styling; never write custom CSS unless scoped.
- Register layouts only at the page level.
- Forms should always try use `useForm` when possible.
- **Prefer `computed` properties over `watch`.** Watchers must only be used when no cleaner, declarative solution exists.
- **Reuse existing utilities before writing new ones.** Promote broadly reusable helpers to `/utils` to avoid duplication and ensure consistency across the app.
