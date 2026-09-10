# Serve Tracker

A single-file web app for tallying volleyball serves from the sideline —
built to replace a paper stat sheet.

## Using it

Open `index.html` in any browser, or add it to a phone home screen. No build
step, no server, no dependencies, no sign-in. Works offline.

1. **Team** — name the team, then add your players (there is a "Paste a whole
   roster" box that takes one player per line, number first).
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

## Where the data lives

Stats are written to `localStorage` on the device first, so a tap is never lost
to a bad connection. Each device keeps its own copy — nothing is uploaded and
there are no accounts.

Published as a Claude Artifact, the page also picks up the `db` capability and
syncs across the devices signed in to that account. Changes that fail to upload
stay in a queue, retry with backoff, and are never overwritten by incoming
data — the footer shows how many are still waiting. The `downloads` capability
handles CSV export, falling back to a plain browser download.
