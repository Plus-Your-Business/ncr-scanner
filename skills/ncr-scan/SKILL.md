---
name: ncr-scan
description: >
  This skill should be used when the user asks to "scan for NCRs",
  "run an NCR scan", "check for non-conformances", "give me my NCR digest",
  or when a scheduled NCR Scanner task runs. It scans recent business
  activity for potential non-conformances and presents candidates for
  human review.
metadata:
  version: "0.1.0"
  author: "Plus Your Business"
---

# NCR scan

Scan the user's recent business activity for potential non-conformances (NCRs), draft candidates in a fixed format, and present them for review. The assistant proposes, the human disposes: never file, record, or export anything without explicit approval in this conversation.

If configuration (standards mode, delivery, sources, scope hints) has not been supplied by a scheduled task prompt or by the user, run with defaults: General mode, inline delivery, CRM plus any connected email, no scope hints. If the user has never run setup, suggest the ncr-setup skill after delivering results.

## Scan window

Use the window given in the invoking prompt. Scheduled runs use a rolling 8 days. A first or manual run with no stated window uses 30 days. State the window covered at the top of every digest.

## Sources

- **Connected email is the signal source.** Read recent threads within the window. NCR signal sits in client conversations far more than in CRM records.
- **The CRM is context only.** Use it to attach client, company, or deal context to items already flagged from email. Do not scan CRM tickets or records for NCRs, and never write anything to the CRM. The CRM is read-only throughout.
- If only the CRM is available, scan recent activity and notes for signal, note in the digest that coverage is limited without email, and suggest connecting one.

## What to flag and what to ignore

Read `references/signals.md` for the signal list and discernment rules. The core rule: read the underlying thread before flagging anything. Never flag from a subject line or snippet alone.

## Classification

Only when one or more ISO standards are toggled on in the configuration, read `references/standards.md` and classify each candidate against the toggled standards. In General mode do not load that file; give each candidate a plain issue category instead (for example "missed deadline", "billing error", "communication breakdown").

## Candidate format

Present each candidate with exactly these fields. Mark any inferred value "(proposed)".

- **Date**: when the issue occurred or surfaced
- **Client or context**: who or what it relates to
- **Standard**: the toggled standard it maps to, or "General"
- **Issue summary**: one or two plain sentences
- **Immediate action**: what was done at the time, if evident
- **Root cause**: (proposed)
- **Corrective action**: (proposed)
- **Responsibility**: (proposed), using scope-hint names where given
- **Source**: a pointer to the originating thread or record

Do not re-raise candidates already presented in a previous digest visible in this task's history. If an old issue has materially developed, raise it as an update and say so.

## Delivery

- **Inline** (default): post the digest as a clean, scannable list in this conversation.
- **Chat**: send the same content to the DM or channel named in the configuration. Lead with a heading and the window covered.

End every digest with the approval prompt, worded like: "Nothing has been recorded. Reply with the items to keep (for example 'keep 2 and 4') and I will put them in a spreadsheet, or dismiss them all."

## Approval and export

When the user approves items, generate a fresh .xlsx containing only the approved candidates, one row per NCR, columns matching the candidate fields plus a status column defaulting to "Open". Present the file to the user. If the user asks to add to a previous week's file, extend that file instead. Never push approved items anywhere else, and never involve the CRM in record keeping.

## Guardrails

These are not optional and apply on every run:

- Human approval before anything is recorded or exported.
- Never blend one client's information into another client's context.
- Keep PII out of summaries and exports: no personal email addresses, phone numbers, or personal data. Refer to people by name and role only.
- UK English, plain factual tone, no marketing language, no em dashes.
- If asked, state that this supports an internal NCR or quality process and is not a substitute for a certified management system.
