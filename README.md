# Kinevera

**An internal research workspace that finds, verifies and explains US museums, so a salesperson walks into the first conversation prepared.**

![Next.js](https://img.shields.io/badge/Next.js_16-000000?logo=nextdotjs)
![React](https://img.shields.io/badge/React_19-149ECA?logo=react&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL_8.4-4479A1?logo=mysql&logoColor=white)

> Showcase repository. The source lives in a private repo because it was built for a company's internship assessment and handles non-public workflow data. This page documents what it does and the decisions behind it.

## The problem

Find every strict museum in the USA that charges for entry or exhibitions, keep each one's details (address, phone, email, website, ticket price, published staff) with the time each fact was fetched, and prepare a salesperson for the first conversation.

## What it does

- **13,829 museums** across all 50 states and DC, imported from the public IMLS Museum Data Files, then narrowed to the **1,287 paid or mixed-admission museums** that are in scope.
- **Search-first workspace**: search by any words, filter by state and admission, open a full record with tabs for Overview, People, Technology, Sales fit and Activity.
- **One-click Scan**: a collector reads a museum's own website (robots-aware, SSRF-safe, rate-limited, stops at CAPTCHAs). An agent-console view shows six stages (Scout, Facts, People, Tech, Match, Scribe) and moves only when the worker actually saves an event.
- **City discovery pipeline**: many independent sources (open data, local directories, licensed APIs, web search) merged into organisations, so recall comes from combining sources and measuring what each adds.
- **Review queue**: every finding is staged as *pending* and approved against its source. Nothing is published by a rule alone, and each approval keeps the fetch time and a change history.
- **Coverage and Progress views** with a US tile map and price statistics; invite-only accounts with viewer, researcher and admin roles; Cloudflare Turnstile on data access; verified database backups.

## Engineering decisions worth reading

| Decision | Why |
|---|---|
| Adapters only normalise; one place classifies | Keeps judgement auditable and testable, and keeps raw evidence for every decision. A failing source is recorded and the scan carries on. |
| Entity resolution leans toward *not* merging | A wrong merge hides a museum (an invisible recall loss); a wrong split only adds a duplicate a person can mark. Every merge stores its reason. |
| "Unknown" is never "absent" | A missing technology signal or price means *not yet found*, and the UI says so instead of guessing. |
| LLM output is re-checked against a verbatim quote | Prices read by a model are staged only if the quote appears on the page. |
| Respect site terms | Search pages that may not be scraped are not scraped; licensed APIs are used where a source forbids scraping. |

## Stack

Next.js 16 · React 19 · TypeScript · MySQL 8.4 (Docker) · Node's built-in test runner via `tsx` · MapLibre · Cloudflare Turnstile · Playwright for end-to-end tests.

## Status

Runs locally; not deployed. Open items are tracked honestly in the project's requirements-status doc: leadership data for some museums, prices for 198 in-scope museums, MFA and password reset.

*Figures are from the 13 October 2026 build.*
