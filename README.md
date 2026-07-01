# Tamakun PM — Project Management Platform

An in‑house project‑management platform built for **Tamakun** and running in production at **[pm.tamakun.sa](https://pm.tamakun.sa)**. It runs the company's engineering and operations end‑to‑end — planning, time tracking, team communication, analytics, and app provisioning — with a standout twist: **first‑class AI‑agent automation**, where AI coding agents pull, execute, and complete real tasks through a built‑in integration.

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

## Status

Actively developed and **in production** at [pm.tamakun.sa](https://pm.tamakun.sa).

## Author

Built by **Mohammed Abideen** ([@mohamed-abedeen](https://github.com/mohamed-abedeen)).

---

<sub>This is a public write‑up of a private, proprietary platform. No source code is included. © Tamakun — all rights reserved.</sub>
