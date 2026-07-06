# NCR Scanner

A Claude plugin that scans your recent business activity for potential non-conformances (NCRs), presents them for your review, and optionally saves the ones you approve to a spreadsheet.

The assistant proposes, you dispose. Nothing is ever filed or recorded automatically.

NCR Scanner is part of the GuardHub suite of quality and governance tools for HubSpot users, from Plus Your Business.

## What it does

On a weekly schedule (or on demand), the plugin reads your recent client email threads, flags candidate non-conformances such as missed deadlines, client complaints, scope discrepancies, billing issues, and data handling concerns, and delivers them as a short digest. You decide what to keep. Approved items can be saved to a simple spreadsheet.

Candidates can optionally be classified against ISO 9001 (quality), ISO 27001 (information security), or ISO 42001 (AI management), or kept in plain General mode with no ISO framing.

## Requirements

- **HubSpot connected to Claude (required).** Used read-only, to attach client and deal context to flagged issues. The plugin never writes to HubSpot.
- **Email connected (strongly recommended).** Works with Gmail or Outlook. Most NCR signal lives in client email threads, so results are materially better with email connected. The plugin still runs with HubSpot alone.
- **Chat connected (optional).** Works with Slack or Teams, only needed if you want digests delivered there rather than in the conversation.

Connect these through your Claude connector settings before running setup.

## Skills

- **ncr-setup**: say "set up NCR Scanner". Checks your connectors, asks a handful of configuration questions (standards, delivery, day and time, sources), creates the weekly scheduled scan, and offers an immediate first scan with a 30-day lookback.
- **ncr-scan**: runs on the schedule, or say "scan for NCRs now". Scans the window, presents candidates in a fixed reviewable format, and saves approved items to a spreadsheet only when you ask.

## Changing settings

Say "change my NCR settings" at any time. Settings live in the scheduled task itself, so there is nothing else to maintain.

## Positioning

This plugin supports an internal NCR or quality process. It is not a substitute for a certified management system, and it makes no claim of ISO certification or compliance.

## Author

NCR Scanner is a GuardHub tool, built by [Plus Your Business](https://www.plusyourbusiness.com), a HubSpot Elite Solutions Partner certified to ISO 9001, 27001, and 42001.

