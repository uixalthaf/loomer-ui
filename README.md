Loomer UI

A full-stack social platform built to explore how a familiar social experience can be engineered into a maintainable, scalable, production-ready application.

Loomer began as a React/JavaScript prototype and is being evolved into a TypeScript-based full-stack application. The goal is not to reinvent social media, but to use a real product as a vehicle for solving the engineering problems that emerge as an application grows: authentication, authorization, feed performance, real-time messaging, notifications, moderation, data integrity, and maintainable architecture.

Product vision

Loomer is a place to share, discuss, discover, and connect.

The product is intentionally built around a familiar social experience while focusing on a seamless path from:

Discover → Interact → Connect → Continue the conversation

The long-term experience brings public posts, conversations, profiles, notifications, and real-time messaging together in one consistent product.

Loomer is not positioned as a replacement for Twitter, Reddit, or other established platforms. Its value as a project comes from taking a familiar product domain and engineering it thoughtfully from the ground up.

Engineering goals

The project is being developed with the following priorities:

Type safety across the frontend, backend, and shared contracts

Feature-based architecture that remains easy to navigate as Loomer grows

Clear frontend/backend boundaries

Secure authentication and role-based authorization

PostgreSQL-backed persistence instead of Firebase

Real-time communication for Messenger using WebSockets

Optimistic UI for interactions that should feel instant

Cursor-based pagination for feed and message history

Reusable, accessible UI primitives using shadcn/ui and Base UI

Validation at system boundaries with Zod

Testing, observability, and CI/CD as the project matures

Tech stack

Layer

Technology

Web

Next.js, React, TypeScript

Styling

Tailwind CSS

UI primitives

shadcn/ui, Base UI

Icons

Lucide

API

Node.js, Express, TypeScript

Validation

Zod

Database

PostgreSQL

Real-time

WebSockets

Local infrastructure

Docker / Docker Compose

Package management

pnpm workspaces

The stack is deliberately split into a Next.js web application and a separate Node.js API so presentation concerns and backend/domain concerns can evolve independently.

Architecture

                         LOOMER
                           │
              ┌────────────┴────────────┐
              │                         │
           Web App                    API
          Next.js                  Node.js
       React + TypeScript       Express + TypeScript
              │                         │
              │ REST / WebSocket        │
              └────────────┬────────────┘
                           │
                      PostgreSQL

Repository structure

loomer/
├── apps/
│   ├── web/                    # Next.js frontend
│   │   └── src/
│   │       ├── app/            # Routes, layouts and route boundaries
│   │       ├── features/       # Domain-specific UI and client logic
│   │       ├── components/     # Shared application components
│   │       ├── hooks/          # Reusable React hooks
│   │       ├── lib/            # Frontend utilities
│   │       └── providers/      # Application providers
│   │
│   └── api/                    # Node.js backend
│       └── src/
│           ├── modules/        # Feature/domain modules
│           ├── middleware/     # Authentication, authorization, etc.
│           ├── infrastructure/ # Database, logging, storage, etc.
│           ├── config/         # Environment and application config
│           ├── app.ts
│           └── server.ts
│
├── packages/
│   ├── types/                  # Shared TypeScript contracts
│   ├── validation/             # Shared Zod schemas
│   └── config/                 # Shared configuration
│
├── docker-compose.yml
├── docs/
└── README.md

Core domains

Authentication & authorization

Loomer uses a role-based model with two initial roles:

user — standard platform access

admin — standard access plus moderation and administration capabilities

Authentication answers who the requester is. Authorization answers what that requester is allowed to do.

Authorization is enforced on the backend; frontend route protection is used to provide the correct user experience without being treated as the security boundary.

Feed & posts

The feed is designed around efficient data access rather than loading an ever-growing collection into the browser.

Planned production-oriented behavior includes:

Cursor-based pagination

Optimistic likes and interactions

Efficient database indexes

Incremental loading

Thoughtful loading and error states

Messenger

Messenger is intended to be Loomer's primary real-time feature.

User A
  │
  │ send message
  ▼
Node.js WebSocket layer
  │
  ├── persist → PostgreSQL
  │
  └── emit → User B

Planned capabilities include:

Real-time messages

Optimistic message sending

Typing indicators

Read receipts

Presence / online state

Reconnection handling

Conversation history

Notifications

Notifications will be treated as a platform-level capability rather than tightly coupling notification logic to individual UI components.

Examples include:

Likes

Comments and replies

Mentions

New messages

Follow/connect activity

The architecture leaves room for an event-driven notification pipeline as Loomer grows.

Admin & moderation

The admin area is designed around real platform operations rather than being a separate demo dashboard.

Planned capabilities include:

User management

Post management

Reports / moderation queue

Content actions

Audit logs

Platform activity metrics

Sensitive operations are protected by backend authorization.

UI architecture

Loomer's existing visual design is treated as the source of truth during the migration.

The migration is intended to change the implementation without unnecessarily changing the product's visual identity.

shadcn/ui + Base UI
        ↓
Loomer UI primitives
        ↓
Feature components
        ↓
Product screens

Shared primitives such as buttons, inputs, dialogs, menus, avatars, and tooltips live under components/ui. Product-specific components such as PostCard, PostComposer, Feed, and Messenger remain within their respective feature boundaries.

Development roadmap

Phase 1 — Foundation

Establish monorepo structure

Create Next.js web application

Create Node.js API application

Establish shared TypeScript packages

Define PostgreSQL as the target database

Complete original UI migration without visual regressions

Remove remaining Firebase dependencies

Phase 2 — Authentication

Registration

Login / logout

Secure session management

Protected routes

User/admin authorization

Password handling and validation

Phase 3 — Social core

User profiles

Posts

Comments and replies

Likes

Feed pagination

Optimistic interactions

Phase 4 — Real-time communication

Conversations

WebSocket messaging

Typing indicators

Read receipts

Presence

Reconnection strategy

Phase 5 — Platform systems

Notifications

Reporting

Moderation workflow

Admin dashboard

Audit logging

Phase 6 — Production hardening

Automated tests

Error handling and observability

Rate limiting

Performance profiling

CI/CD

Production deployment

Local development

Prerequisites

Node.js

pnpm

Docker

PostgreSQL (or the provided Docker setup)

Install

pnpm install

Start the web app

pnpm dev:web

Start the API

pnpm dev:api

Start local infrastructure

docker compose up -d

Project principles

Preserve before refactoring

Existing product behavior and UI should be understood before replacing implementation details.

Feature ownership

Business logic belongs to the feature that owns it instead of being scattered across unrelated global folders.

Server is the security boundary

The frontend can hide unavailable UI, but authorization decisions are always enforced by the backend.

Prefer explicit data contracts

Shared TypeScript types and validation schemas keep the web application and API aligned.

Optimize for real user experience

Performance work should solve measurable problems: slow feeds, unnecessary JavaScript, delayed interactions, excessive requests, and poor loading states.

Don't over-engineer prematurely

The architecture should make future growth possible without building infrastructure before the product needs it.

Project status

Loomer is currently undergoing a JavaScript → TypeScript and Vite/React → Next.js migration, alongside the transition from Firebase toward a dedicated Node.js + PostgreSQL backend.

The current priority is establishing a clean foundation while preserving the existing Loomer experience. New platform capabilities will be implemented incrementally on top of that foundation.

Why Loomer?

Loomer is intentionally more than a collection of CRUD screens. It is a long-running engineering project used to explore the realities of building a modern full-stack product:

How do you take a simple social application and evolve it into a maintainable, secure, responsive, real-time system without letting the codebase collapse under its own complexity?

That question drives the architecture and the roadmap.
