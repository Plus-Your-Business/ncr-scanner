---
name: ncr-setup
description: >
  This skill should be used when the user asks to "set up NCR Scanner",
  "configure NCR scanning", "start NCR monitoring", "change my NCR settings",
  or invokes the NCR Scanner plugin for the first time. It captures the
  user's preferences and creates the weekly scheduled scan.
metadata:
  version: "0.1.0"
  author: "Plus Your Business"
---

# NCR Scanner setup

Configure the NCR Scanner for this user and create their weekly scheduled scan. The guiding principle for the whole plugin: the assistant proposes, the human disposes. Nothing is ever filed or recorded automatically.

Run the four steps below in order.

## Step 1: Check connectors

Identify which of the user's connected tools fall into these categories. Do not name specific products in questions unless the user has them connected; work from what is actually available.

- **CRM (required).** A HubSpot connection is the baseline assumption for this plugin. If no HubSpot connection is available, stop here. Explain that the plugin is built for HubSpot-connected users, and point them to claude.ai connector settings to add it. Do not continue setup without it.
- **Email (strongly recommended).** Any connected email source (for example Gmail or Outlook). Most NCR signal lives in client email threads, not in CRM records. If email is not connected, say plainly that results will be materially weaker without it, offer to continue with CRM only, and note that they can add email later and re-run setup.
- **Chat (optional).** Any connected chat tool (for example Slack or Teams). Only relevant if the user chooses chat delivery in Step 2. Do not ask them to connect one otherwise.

## Step 2: Ask the configuration questions

Use the AskUserQuestion tool. Ask only about options that are real for this user given Step 1. Questions to cover:

1. **Standards mode** (multi-select): General NCR (default), ISO 9001 quality, ISO 27001 information security, ISO 42001 AI management. General mode skips ISO framing and flags plain quality or process issues. Toggles are independent; a user can select several.
2. **Delivery route**: inline conversation digest (default), or a message to their connected chat tool. If chat is chosen, ask whether DM (default) or a named channel.
3. **Cadence**: weekly by default. Ask for preferred day and time.
4. **Sources**: CRM only, or CRM plus connected email. Recommend including email if it is connected.
5. **Scope hints** (optional, free text): team member names to use for the responsibility field, and any client names to include or exclude.

Record keeping needs no question: spreadsheet export is always available on demand after any digest, and off by default.

## Step 3: Create the scheduled task

Create a weekly scheduled task at the user's chosen day and time. Write every configuration answer directly into the task's prompt so each run arrives fully configured with no stored state. Use this prompt template, filling the bracketed values from Step 2:

> Run the ncr-scan skill from the ncr-scanner plugin. Configuration: standards mode [selected standards or "General"]; delivery [inline / chat DM / chat channel name]; sources [CRM only / CRM plus email]; scope hints [names, inclusions, exclusions, or "none"]. Scan the last 8 days of activity. Do not re-raise candidates already presented in a previous digest in this task's history.

The 8-day window gives one day of overlap with a weekly cadence so a delayed run loses no coverage.

If the user later asks to change any setting, update the scheduled task's prompt rather than creating a second task.

## Step 4: Offer an immediate first scan

Offer to run the first scan now with a 30-day lookback (let the user adjust this) so they see value immediately rather than waiting for the first scheduled run. If accepted, invoke the ncr-scan skill with the agreed configuration and the 30-day window.

## Closing note

End setup by stating plainly: this assistant supports an internal NCR or quality process. It is not a substitute for a certified management system and makes no claim of ISO certification or compliance.
