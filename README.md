# Mailing Scheduler

A NiceGUI app that plans when marketing emails should go out, respecting per-segment sending limits and a scored weekly time-slot grid.

## Problem

Sending too many emails to the same audience segment in a short window hurts deliverability and drives unsubscribes/complaints, but manually tracking "how many emails has segment X already received this week, and which time slots are still good" across many concurrent campaigns doesn't scale in a spreadsheet. The tool automates that bookkeeping and does the actual slot assignment.

## Solution & Architecture

- Domain model built around three concepts: **Segments** (with daily/weekly send limits and an active flag), **Slots** (day-of-week + hour, each with a quality score), and **Sends** (a planned campaign tied to a segment, priority, and a year/week) — scheduled sends are then assigned to specific dates/slots respecting segment limits and slot scores.
- SQLite persistence with a schema that's evolved across versions (this is the 4th iteration — the version history shows the model being progressively simplified, most recently by dropping product/keypoint concepts that turned out to add complexity without enough value).
- NiceGUI front-end for managing segments, slots, and the send queue; Excel import/export (`openpyxl`) so schedules can be shared with non-technical stakeholders who work in spreadsheets.
- This is the one project in the set with a real commit history, showing the design being iterated on rather than built once and left alone.

## Key Product Decisions

- **Simplify the model over time, not up front** — earlier versions modeled more entities (products, keypoints) than were actually useful; v4 deliberately strips the model down to segments + slots + sends, which better matched how the tool was actually used.
- **Score-based slots instead of a fixed calendar** — time slots carry a quality score rather than being simply available/unavailable, so scheduling can prefer better-performing send times instead of just the next open one.
- **Excel as an interoperability layer** — rather than requiring everyone to use the app UI, schedules can be exported to/from Excel for handoff to people who aren't going to log into an internal tool.

## Outcome

Replaced manual, error-prone tracking of per-segment send frequency and time-slot allocation with a single source of truth that enforces the limits automatically.

## Preview

<!-- screenshot placeholder — added separately -->

## Note

Source code is kept in a private repository; happy to share on request.
