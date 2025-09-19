# Vue Guide

This document defines the structure and rules for building Vue 3 components using Inertia.js and TailwindCSS. It mirrors the backend architecture and ensures clean separation of concerns, modular design, and long-term maintainability.

---

## Core Principles

- Pages only coordinate — no logic or bloat.
- Logic must be extracted to composables or services.
- Components are atomic, reusable, and scoped.
- Consistency and reuse always take priority over duplication.

---

## Coding Rules

### State & Reactivity
- All state-related logic must be placed in composables.
- Prefer `computed` properties over `watch`/`watchEffect`.
    - Watchers are allowed only for unavoidable side effects.

### Utilities & Reuse
- Reuse existing utility classes, formatters, and parsers before creating new ones.
- If a helper is broadly reusable, promote it to `/utils` instead of keeping it inline in a component.

### Components
- Must be presentational and lean; avoid business logic.
- Keep components under 300 lines where possible.
- Avoid deeply nested component trees — prefer flatter hierarchies.

### Styling
- Use Tailwind utility classes for styling.
- Custom CSS must be scoped and used only when Tailwind cannot solve the problem.

### Layouts
- Layouts must only be registered at the page level.

### Forms
- Always prefer `useForm` for form handling.

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
- Must never contain logic, mutation, or reusable markup.

### Components
- Organized into subfolders:
    - `Layouts/` – App and auth wrappers (`AppLayout.vue`).
    - `{domain}/` – Reusable, grouped form blocks (`UserForm.vue`).
- Must always use composition API `<script setup>`.
- Components should be as stateless as possible, delegating logic to composables.

### Composables
- Named like `useXxx.js` (e.g., `useModal`, `usePagination`).
- Extract reusable logic from components/pages.
- Must be stateless and scoped.

### Services
- Wrap external interactions such as Inertia visits, API helpers, and uploaders.
- Encapsulate all data-fetching and side-effect logic.

### Enums
- Provide a single source of truth for shared constants (e.g., statuses, roles).
- Auto-generated from the backend using `php artisan atlas:export-enums`.

### Utils
- Stateless helpers (`formatDate`, `slugify`).
- No app state, DOM access, or side effects.
- Broadly reusable helpers must always be promoted here.

---

## Naming Conventions

| Layer       | Format     | Example             |
|-------------|------------|---------------------|
| Pages       | PascalCase | `Users/Index.vue`   |
| Components  | PascalCase | `UserForm.vue`      |
| Composables | camelCase  | `useModal.js`       |
| Services    | camelCase  | `api.js`            |
| Utils       | camelCase  | `formatDate.js`     |

<!-- end of Vue Guide -->