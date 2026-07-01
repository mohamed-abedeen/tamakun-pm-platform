<h1 align="center">Tamakun PM — Project Management Platform</h1>

<p align="center">
  An in‑house project‑management platform with <strong>first‑class AI‑agent task automation</strong>, running in production at <a href="https://pm.tamakun.sa">pm.tamakun.sa</a>.
</p>

<p align="center">
  <img alt="Status" src="https://img.shields.io/badge/status-in%20production-1E8E4E">
  <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white">
  <img alt="NestJS" src="https://img.shields.io/badge/NestJS-E0234E?logo=nestjs&logoColor=white">
  <img alt="Next.js" src="https://img.shields.io/badge/Next.js-000000?logo=nextdotjs&logoColor=white">
  <img alt="Prisma" src="https://img.shields.io/badge/Prisma-2D3748?logo=prisma&logoColor=white">
  <img alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white">
  <img alt="Redis" src="https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white">
  <img alt="Tailwind CSS" src="https://img.shields.io/badge/Tailwind_CSS-06B6D4?logo=tailwindcss&logoColor=white">
  <img alt="socket.io" src="https://img.shields.io/badge/Realtime-socket.io-010101?logo=socketdotio&logoColor=white">
  <img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white">
  <img alt="MCP" src="https://img.shields.io/badge/AI-MCP%20integration-5A67D8">
  <img alt="i18n" src="https://img.shields.io/badge/i18n-AR%20%2F%20EN%20(RTL)-0891B2">
  <img alt="License" src="https://img.shields.io/badge/license-proprietary-B23B3B">
</p>

> ℹ️ **This repository is a showcase / write‑up of the platform.** The source code is private and not included here. All rights reserved.

---

## What it is

Tamakun PM is a full project‑management platform where teams plan work on boards, log time, chat, and review analytics — in **both Arabic and English**. What makes it different is that it treats **AI coding agents as first‑class “employees”**: a task can be assigned to an agent that authenticates, pulls the task with its full context and project rules, does the work, and reports back — with time and token usage tracked automatically.

## Who it serves

- **Engineering & operations teams** — manage projects, tasks, timesheets, reviews, and files day‑to‑day.
- **Admins** — provision and observe deployed applications (logs, metrics, errors) from inside the platform, manage users, and audit sensitive actions.
- **AI agents (Claude Code)** — act as automated developers: pull the next assigned task, follow project‑wide rules, execute the work, and complete it — routed to review or done based on the project's review policy.

---

## Highlights

- 🤖 **AI‑agent task workflow** — a Model Context Protocol (MCP) integration lets coding agents authenticate, pull the next task (with accumulated context, comments, and follow‑up history), start a work timer, execute, and complete — all with per‑task time and token accounting.
- 🗂️ **Projects & tasks** — Kanban, List, Calendar, and Timeline views; dependencies, subtasks, checklists, labels, priorities, and follow‑up loops.
- ⏱️ **Time tracking** — live timers with idle detection and auto‑stop, validated manual logs, per‑project timesheets, and CSV export.
- 💬 **Team chat** — auto‑created project channels plus direct and group messages, attachments, reactions, @mentions, unread tracking, all realtime.
- 📊 **Analytics** — personal and team dashboards, burndown/velocity, workload heatmaps, and exportable PDF reports.
- 🔗 **GitHub integration** — link PRs and commits to tasks, auto‑manage repo collaborators on assignment, and drive task transitions from webhook events.
- 🚀 **App provisioning & observability** — spin up applications and read their container, CI/CD, error, and metric logs without leaving the platform.
- 🛡️ **Audit log** — an IP / User‑Agent‑stamped record of sensitive actions across the system.
- 🔔 **Notifications** — in‑app, email, and **Web Push** (installable PWA), with per‑type user preferences.
- 🔐 **Access control** — role‑based (Admin / Reviewer / Developer), project membership, and invite‑only GitHub sign‑in.
- 🌍 **Bilingual & RTL** — complete Arabic/English experience with a polished, emerald‑themed UI and light/dark modes.

---

## Screenshots

<!-- Add images to the /screenshots folder and reference them below, e.g.:
| Dashboard | Kanban board |
| --- | --- |
| ![Dashboard](screenshots/dashboard.png) | ![Board](screenshots/board.png) |
-->

_Screenshots are added in [`/screenshots`](screenshots). Suggested set: Dashboard · Kanban board · Task detail · Team chat · Analytics · Timesheet._

---

## Tech stack

**Backend (API)**
- [NestJS](https://nestjs.com/) — REST (`/api/v1`) + WebSocket gateway
- [Prisma](https://www.prisma.io/) ORM over **PostgreSQL**
- **Redis** (caching / realtime), **MinIO** (object storage), **Meilisearch** (search)
- [socket.io](https://socket.io/) for realtime, JWT (httpOnly cookies) auth + long‑lived tokens for the agent integration
- Web Push (VAPID), Helmet, request throttling, Swagger/OpenAPI

**Frontend (Web)**
- [Next.js](https://nextjs.org/) (App Router) + React
- [next-intl](https://next-intl-docs.vercel.app/) (AR/EN, RTL), Radix UI + Tailwind CSS, `next-themes`
- `zustand` (state), TanStack Query, `socket.io-client`
- Installable **PWA** with Web Push notifications

**AI integration**
- A **Model Context Protocol (MCP) server** that exposes the task workflow (pull → start → work → complete, plus context, notes, follow‑ups, and file attachments) to Claude Code.

**Infrastructure**
- Containerized services behind an nginx reverse proxy
- Self‑hosted **CI/CD** that auto‑builds, smoke‑tests, and **auto‑rolls‑back** on failure

---

## Architecture (high level)

A Turborepo monorepo:

```
apps/
  api/           NestJS backend  (REST + WebSocket, Prisma)
  web/           Next.js frontend (App Router, AR/EN, PWA)
packages/
  mcp-server/    Claude Code MCP integration (agent task workflow)
```

**Data layer:** PostgreSQL (system of record) · Redis (cache + realtime) · MinIO (files/attachments) · Meilisearch (search).
**Realtime:** socket.io channels for chat, notifications, presence, and live board/timer updates.
**Delivery:** Dockerized services, self‑hosted CI/CD with automatic build → smoke‑test → rollback.

---

## My role & contributions

**Full‑stack engineer & platform architect** — designed and built the platform end to end.

- **Architecture** — designed the Turborepo monorepo (NestJS API, Next.js web, MCP server) and the data layer (PostgreSQL/Prisma, Redis, MinIO, Meilisearch), realtime over socket.io, and cookie‑based JWT auth alongside long‑lived tokens for the agent integration.
- **AI‑agent workflow (MCP)** — built the Model Context Protocol server and the task lifecycle (pull → start → execute → complete, with context/notes/follow‑ups and file attachments), including per‑task time and token accounting.
- **Product features** — implemented projects & multi‑view boards (Kanban/List/Calendar/Timeline), time tracking & timesheets, realtime team chat, analytics dashboards & PDF reports, GitHub integration, app provisioning & observability, notifications (in‑app/email/Web Push), and a full audit log.
- **Frontend** — built the Next.js UI with a bilingual (AR/EN) RTL‑aware, themeable design system on Radix + Tailwind, realtime updates, and installable PWA.
- **Security & quality** — role‑based access control, input validation, request throttling, and an audit‑driven hardening pass across authentication, access scoping, and data exposure.
- **DevOps** — containerized the stack and set up a self‑hosted CI/CD pipeline that auto‑builds, smoke‑tests, and auto‑rolls‑back on failure.

**Tech:** TypeScript · NestJS · Next.js · React · Prisma · PostgreSQL · Redis · MinIO · Meilisearch · socket.io · Tailwind CSS · Docker · Model Context Protocol.

---

## Status

Actively developed and **in production** at [pm.tamakun.sa](https://pm.tamakun.sa).

## Author

Built by **Mohammed Abideen** ([@mohamed-abedeen](https://github.com/mohamed-abedeen)).

---

<sub>This is a public write‑up of a private, proprietary platform. No source code is included. © Tamakun — all rights reserved.</sub>
