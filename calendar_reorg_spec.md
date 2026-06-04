# Executive Calendar Reorganization Specification

This document provides structured context, rules, and prompt instructions for Claude Desktop to manage, audit, and re-organize an executive calendar.

---

## 1. Executive Profile & Preferences

### Timezone & Working Hours
- **Primary Timezone:** UK Time (`Europe/London`)
- **Standard Working Hours:** `08:30 - 18:00` (Flexible to stretch if required for critical meetings)
- **Lunch Break (Non-negotiable):** 
  - **Window:** Must occur between `11:30 - 13:30`.
  - **Minimum Duration:** `30 minutes`.
  - **Rule:** If a Tier 1 meeting conflicts, shift the lunch block within the window; do not cancel or shorten below 30 minutes.

### Energy & Focus Optimization
- **Peak Performance Window:** `08:30 - 15:00` (High energy. Maximize focus here).
- **Low Energy Window:** `16:00 - 18:00` (Avoid creative/deep work. Prefer skip-level meetings, routine 1:1s, admin, email catch-up, and external vendor syncs).
- **Buffer Times & Caps:** 
  - **Standard Buffer:** Minimum of `15 minutes` between all standard meetings.
  - **Offsite Buffer:** Minimum of `30 minutes` travel buffer before and after meetings at "Investor HQ" (or any external offsite location).
  - **Consecutive Meeting Cap:** Maximum of `3` meetings back-to-back. After the 3rd meeting, enforce a longer break of at least `30 minutes`.

---

## 2. Meeting & Scheduling Rules

### Scope of Reorganization (Recurring Meetings Only)
- **Target Meetings:** Scheduling optimizations, rescheduling, and triage rules apply **exclusively to recurring meetings**.
- **One-off / Ad-hoc Meetings:** Must remain untouched. Treat one-off events as fixed commitments that cannot be rescheduled or cancelled. Claude should work around them.

### Engineering Standup Attendance Policy
- **General Rule:** Maintain a healthy attendance percentage across all Engineering team standups.
- **Priority Team:** Prioritize attendance for the **Attest Explore** engineering team.
- **Mandatory (Do Not Miss):**
  - **Explore GTM** meetings (must always attend).
  - **Attest Explore** standups at the beginning of the week (Mondays) and at the end of the week (Fridays). These must never be missed, rescheduled, or skipped.

### Deep Work & Thinking Time
- **Frequency:** Minimum of **three 2-hour blocks** per week.
- **Priority Days:** Monday, Wednesday, and Friday.
- **Timing:** Ideally scheduled after `10:00` (to align with high-energy morning windows).
- **Override Rule:** These blocks are high-priority but flexible. If a group meeting or a Tier 1 stakeholder cannot be avoided, shift the deep work block elsewhere in the week rather than deleting it.

### Meeting Durations by Tier
- **Tier 1 (CEO / Exec / Key Explore Meetings):** Default to `45 minutes` or `1 hour`.
- **Tier 2 (Direct Reports & Key Partners):** Default to `45 minutes` or `1 hour`.
- **Tier 3 (Dept Heads):** Default to `30 minutes`.
- **Tier 4 (Skip-levels / Ad-hoc):** Always default to `30 minutes`.
- **All Hands / User-Arranged Team Syncs:** Default to `1 hour`.
- *Note on External Invites:* For incoming invites from third parties where duration is pre-determined and inflexible, accept the duration if necessary.

---

## 3. Stakeholder & Conflict Triage Tiers

When a conflict arises, Claude should prioritize events and resolve double-bookings based on this hierarchy:

| Priority Tier | Key Stakeholders / Meeting Types | Action on Conflict / Rules |
| :--- | :--- | :--- |
| **Tier 1 (Highest)** | • **Todd Latham** (CEO & Manager)<br>• **Alyssa Stringer** (Head of Product)<br>• **Sam Park**<br>• *Meetings:* Exec Team meetings, Engineering & Product Leadership Meetings, Company All Hands, Product & Engineering All Hands, **Explore GTM**, **Attest Explore Monday & Friday standups** | Never decline. Move other conflicting events. Can reschedule/shift Deep Work blocks if absolutely necessary. |
| **Tier 2** | • *Direct Reports:* **Min Ong** (EM), **Jonathan Evans** (Staff Eng), **Eliot Stocker** (Director of Eng), **Blake Newman** (Staff Eng), **Mike Birmingham** (EM)<br>• *Key Partners:* **Sam Killick** (VP of Customer), **Chris Colley** (VP of Sales), **Vanessa West** (PM), **Rotimi Ibironke** (PM) | High priority. Reschedule within the same week. Avoid double-bookings. |
| **Tier 3** | • Dept Heads (e.g., **Rebecca Batley** - Head of People)<br>• Other non-explore Engineering standups | Flexible. Reschedule to next week or delegate/decline if conflicts cannot be resolved. Attend non-explore standups as a "good percentage" target. |
| **Tier 4 (Lowest)** | • Skip-level meetings, Ad-hoc syncs, Informal catch-ups | Always 30 mins. Decline or auto-delegate if a conflict occurs. |

## 4. Claude Desktop Integration Context (MCP Workflow)

### Connected MCP Server Capabilities
Claude Desktop utilizes a Calendar MCP Server (e.g., Google Calendar or Microsoft Graph/Outlook). It has access to the following tools:
- `list_events` (To retrieve calendar event blocks for a given time range)
- `create_event` / `quick_add_event` (To schedule new events)
- `update_event` (To reschedule or modify event properties/descriptions)
- `delete_event` (To remove or cancel meetings)

### Safety & Execution Protocol
To prevent accidental or destructive modifications, Claude must follow this two-phase execution protocol:

1. **Phase 1: Read & Propose (Mandatory Confirmation)**
   - Query the calendar using read tools (e.g., `list_events`).
   - Identify conflicts, buffer violations, and rules broken based on this specification.
   - Present a clear **Before vs. Proposed After** schedule comparison.
   - **DO NOT** execute any write tools (`create_event`, `update_event`, `delete_event`) during this phase. Stop and explicitly ask the user: *"Would you like me to execute these changes?"*

2. **Phase 2: Execution**
   - Only after receiving explicit approval (e.g., "Yes, make the changes" or "Reschedule everything except meeting X"), invoke the corresponding MCP tools to update the calendar.
   - Confirm when the updates are complete.


