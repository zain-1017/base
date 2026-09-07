# Daybook

A single-page app for money, training, the car and goals — one book instead of
five scattered sources. Runs offline, installs to a phone home screen, and
stores everything in the browser on that device.

Working title.

## What it does

**Home is the day's page, not a dashboard.** A dashboard is read-only, and
read-only things go stale. Capture sits at the top: write a line, and if it has
a `£` in it the app files it as a spend — everything else keeps as a note.
Below that: your net for the month, what needs you today, and the next seven
days of bills.

**Money** keeps a bill list that ticks itself off as the date passes, with a
manual override for the ones that fail or go early. Debts show what each one
unlocks and when, because *"+£171 a month from Aug '27"* moves you and a
remaining balance doesn't. There's a subscription audit sorted by annual cost,
annual bills amortised to monthly, and a five-year projection of your net
position that steps up each time a debt ends.

**Health, Car and Goals** are placeholders that describe what lands in them.

## How it's built

One `index.html`. No build step, no dependencies, no framework.

Four primitives carry every page, so later ones are views over the same store
rather than separate apps bolted together:

| | |
|---|---|
| `ledger`  | money moving on a schedule |
| `debts`   | a ledger line with a term, so it can end |
| `entries` | something written down on a day |
| `targets` | a number with a direction and a finish line |

Reads and writes go through one `load`/`save` pair. Nothing else in the app
knows where the data lives, which is what makes adding a sync backend later a
day's work rather than a rewrite.

## Your data

Nothing ships with the app and nothing is uploaded. A fresh install starts
empty; you add things by hand or import a file you exported from another
device. **Because storage is per-device, two devices hold separate data** —
export and import to move a snapshot between them.

Settings → *Back up your data* does both. Keep a copy somewhere that isn't the
device it came from.

## Design

Monochrome. The only two colours in the app encode state — ahead, and overdue —
never identity, and emphasis is an inverted fill rather than a hue. Air where
you read, density where you work: overview screens are generous, working lists
are tight and fast. No cards; hairlines and space do the separating.

## Running it

Any static server:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000`.
