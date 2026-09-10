# Serve Tracker

A single-file web app for tallying volleyball serves from the sideline —
built to replace a paper stat sheet.

Live: https://cday413-source.github.io/SCS-Gators/

## Using it

Open `index.html` in any browser, or add it to a phone home screen. No build
step, no server, no dependencies, no sign-in, and **no network** — fonts and
all are embedded in the one file, so it works in a gym with no signal.

1. **Team** — the roster comes pre-loaded with the squad's jersey numbers and
   no names, so a first-time visitor can start tapping without entering
   anything. Names are optional throughout; add or edit players here, and
   there is a "Paste a whole roster" box that takes one player per line.
   The numbers live in `SEED_NUMBERS` near the top of the script.
2. **Track** — pick Overhand or Underhand, tap the player who is serving, then
   tap the result: **Ace**, **Over**, **Short**, or **Out**. Undo removes the
   most recent tap.
3. **Stats** — per-player totals for the current game or the whole season:
   serves, over-the-net count and percentage, aces, short, out, plus a mix bar.
   Toggle the overhand / underhand split, or export a CSV.

Keyboard shortcuts while a player is selected: `A` ace, `O` over, `S` short,
`X` out, `U` undo.

## How a serve is counted

Every tap is one serve. **Over** is any serve that cleared the net and stayed
in; aces are included in the Over total and also counted on their own, so
`serves = over + short + out`.

## Handing a game between phones

If someone else covers a game, they send you a **share code** from the Stats
tab and you paste it into yours. The code carries jersey numbers and counts
only — never a name — so it is safe to text to anyone:

```
SERVES1 2026-09-12 Trinity
12 1,1,0,0,0,0,0,0
29 0,0,1,1,0,0,0,0
```

Line 1 is the tag, the date, and an optional opponent. Each following line is
a jersey number and eight counts, in the order
`overhand ace,over,short,out` then `underhand ace,over,short,out`.

On import the app matches by jersey number, adds any number it has not seen
before as a nameless player you can name later, and lets you drop the game in
as a new one or merge it into the game on screen. Importing the same code
twice doubles the counts, so it asks before merging.

CSV export is the separate, spreadsheet-facing format and does include names.

## Where the data lives

Stats are written to `localStorage` on the device first, so a tap is never lost
to a bad connection. Each device keeps its own copy — nothing is uploaded and
there are no accounts.

Published as a Claude Artifact, the page also picks up the `db` capability and
syncs across the devices signed in to that account. Changes that fail to upload
stay in a queue, retry with backoff, and are never overwritten by incoming
data — the footer shows how many are still waiting. The `downloads` capability
handles CSV export, falling back to a plain browser download.
