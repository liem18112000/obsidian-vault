---
ai_hash: 926dbe6c4c304a37
ai_model: google/gemini-2.5-flash
ai_updated: '2026-09-27'
attachments: 0
depth: 3
entities: []
relevance: 0.818
source: https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49755357221/iLetter+Inbox+Status+Chips+Web+Implementation+Spec
space: LUZ
status: reference
tags:
- confluence
- programming
- space/luz
title: iLetter Inbox Status Chips — Web Implementation Spec
topic: programming
type: source
updated: 2026-09-15
---

# iLetter Inbox Status Chips — Web Implementation Spec

> [!info] Imported from Confluence
> Space **LUZ** · updated 2026-09-15 · [open original](https://axonivy.atlassian.net/wiki/spaces/LUZ/pages/49755357221/iLetter+Inbox+Status+Chips+Web+Implementation+Spec)
> Relevance 0.818 · topic `programming`

## Overview

The iLetter (SmartLetter) inbox cards display a status chip indicating the current state of the letter. This spec defines all chip states, their resolution logic, wording in all 4 languages, visual styling, and icons — everything needed to implement the chips on the web platform analogous to the mobile (React Native) implementation.

## Chip States (6 mutually exclusive)

iLetter inbox chips have **6 possible states**, resolved in strict priority order. An **informative iLetter** (no CTA / no form) shows **no chip at all**.

### Resolution Order (highest priority first)

<div>

| \# | State | Condition | Tone |
|----|----|----|----|
| 1 | **Closed** | `isClosed === true` | `closed` (muted) |
| 2 | **Answered** | `isCompleted === true` OR `answeredAt != null` | `done` (success) |
| 3 | **In Progress** | Draft answers have progress (at least one non-empty answer) | `action` (warning) |
| 4 | **Answer By \[date\]** | `autoCloseDate` is set and non-empty | `action` (warning) |
| 5 | **Answer Requested** | None of the above — actionable iLetter with no deadline | `action` (warning) |
| 6 | **No chip** | `letterType === "informative"` OR letter is null | — |

</div>

The first matching condition wins. E.g., if both `isClosed` and `isCompleted` are true, the chip shows "Closed" (not "Answered").

### Overdue variant

"Answer By \[date\]" has a special **overdue** variant: when `autoCloseDate` is in the past, the label switches to "Deadline passed" / "Frist abgelaufen" etc.

## Wording — All 4 Languages

### Chip Labels

<div>

| State | Key | DE | EN | FR | IT |
|----|----|----|----|----|----|
| Answer By | `iletter_inbox_answer_by` | Antworten bis {{date}} | Answer by {{date}} | Répondre jusqu'au {{date}} | Rispondi entro {{date}} |
| Answer By (overdue) | `iletter_inbox_answer_by_overdue` | Frist abgelaufen | Deadline passed | Délai dépassé | Scadenza scaduta |
| Answer Requested | `iletter_inbox_answer_requested` | Bitte antworten | Please reply | Merci de répondre | Per favore rispondi |
| In Progress | `iletter_inbox_continue` | Weiter — {{current}} von {{total}} | Continue — {{current}} of {{total}} | Continuer — {{current}} sur {{total}} | Continua — {{current}} di {{total}} |
| In Progress (branching) | `iletter_inbox_continue_plain` | Weiter | Continue | Continuer | Continua |
| Answered | `iletter_inbox_answered` | Beantwortet {{date}} | Answered {{date}} | Répondu {{date}} | Risposto {{date}} |
| Closed | `iletter_inbox_closed` | Geschlossen — keine Antwort | Closed — no answer | Clos — sans réponse | Chiuso — nessuna risposta |

</div>

### Date Format

Dates use Swiss format: `dd.MM.` (e.g., "15.09."). Near dates use relative labels:

- Tomorrow → "morgen" / "tomorrow" / "demain" / "domani"

- Today → "heute" / "today" / "aujourd'hui" / "oggi"

- Overdue → switches to the overdue label (see above)

## Visual Styling

### Tones → Colors

Each chip state maps to a **tone**, which determines colors:

<div>

| Tone | Used by | Text/Icon Color | Background |
|----|----|----|----|
| `action` | Answer By, Answer Requested, In Progress | `warning` (amber/brown) | `rgba(warning, 0.12)` |
| `done` | Answered | `success` (green) | `rgba(success, 0.12)` |
| `closed` | Closed | `fgSecondary` (muted gray) | translucent |

</div>

On **branded cards** (sender has custom background color), the chip auto-adjusts its text/background for WCAG contrast using the `signingInboxBadgeToneOnBrand()` logic (same as signing badges).

### Icons

<div>

| State            | Icon                                        |
|------------------|---------------------------------------------|
| Answer By        | 🕐 Clock (`TimeClockIcon`)                  |
| Answer Requested | ✏️ Pencil (`PencilIcon`)                    |
| In Progress      | ⏳ Hourglass (`HourglassIcon`)              |
| Answered         | ✅ Checkmark Circle (`CheckmarkCircleIcon`) |
| Closed           | ⊖ Minus Circle (`MinusCircleIcon`)          |

</div>

**Exception:** "In Progress" with known total (not branching) shows a **progress fill bar** instead of an icon. The fill = `answered / total`, fill color = `rgba(warning, 0.35)`.

### Action Ring

Action-tone chips on the **inbox card** (not player header) get a subtle 1px border ring: `rgba(146, 64, 14, 0.55)`.

## Data Requirements (Backend Status)

**⚠️ The backend does NOT currently deliver the required fields.** A field contract has been sent to the dev team.

### Fields needed from inbox list API

<div>

| Field | Type | Drives |
|----|----|----|
| `letterType` | `"informative"` or `"actionable"` | No chip for informative |
| `isClosed` | boolean | "Closed" chip |
| `isCompleted` | boolean | "Answered" chip |
| `answeredAt` | ISO date string or null | "Answered \[date\]" chip |
| `autoCloseDate` | date string or null | "Answer By \[date\]" / "Overdue" chip |
| `draftAnswers` | object or null | "In Progress" chip |
| `smartLetterStatus` | string (currently empty) | Future consolidated status |

</div>

### Current API reality (as of 2026-09-15)

- `smartLetterStatus` field **exists but is empty** (`""`)

- `letterInfo` field is **not present**

- All other chip-driving fields are **missing**

- SmartLetters are identifiable via `mediaType: "application/vnd.ch.klara.epost.smartletter.v1+json"`

Until the backend delivers these fields, use **mock/stub data** for development and design validation.

## RN Reference Code

The complete resolution logic lives in:

- `src/utils/iLetterInboxChip.ts` — `resolveILetterInboxChip()` function

- `src/components/iletter/ILetterInboxStatusChip.tsx` — React Native chip component

- `src/utils/dueChipCopy.ts` — relative date label resolution

## Open Questions for Web

1.  Does the web inbox card already have a chip/badge slot? If yes, reuse the existing component pattern.

2.  Web may use klara-theme Badge or Tag components — check existing signing badge implementation for reference.

3.  Branded card tone adjustment: verify the same WCAG contrast logic applies on web (or use klara-theme built-in contrast utilities).

%% ai-graph-start %%

**Related notes:**
- [[iLetter current backend architecture]]
- [[Steps to implement unread letters count]]
- [[One API 0.03.28.00 (11.08.2026 - 24.08.2026)]]
- [[Public API - letterbox - API get deleted letters from trash]]
- [[LUZ-115505 Public API - letterbox Part 3]]

%% ai-graph-end %%