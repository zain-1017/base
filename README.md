# Daybook

One book for money, training, the car and goals — instead of five scattered
sources. Runs offline, installs to a phone home screen, stores everything in
the browser on that device.

Working title.

## What it does

**Home is the day's page, not a dashboard.** A dashboard is read-only, and
read-only things go stale. Capture sits at the top: write a line, and if it
has a `£` in it the app files it as a spend — everything else keeps as a note.
Below that: your net for the month, what needs you today, this week's training,
and the next seven days of bills.

**Money** is ordered by the questions, not by the data. What is left to pay this
month is the headline, and under it the two figures a turn-based game would
show: **Bank**, what you have, and **Net**, what this month does to it.

Then a small calculator — *need from the 12th to the 17th: £206.54* — which
answers any period without touching anything else. It wraps, so 17th to 5th
reaches into next month, because pay cycles do.

Then the month's bills, all of them, first to last. Paid ones greyed in place;
the rest carry a running column, so reading across any row gives what it costs
to cover everything up to and including it. No notes under the names, nothing
but the figures.

Then the two lines nobody has receipts for. **Food and fuel** sit right below
the bills as two figures you can open and log against — fill-ups recorded on Car
count automatically — so an average builds up and they stop being guesses. Then
annual bills, spread across twelve months.

Then **the month**, set out like an account: bills, food and fuel, the annual
share, then total costs, wage and net at the same weight, with a rule above the
net. That last figure is the one worth moving.

Everything after that is reference and stays folded — debt, subscriptions, your
numbers — each saying what it holds while it is shut.

The bill list ticks itself off as the date passes, with a manual override for
the ones that fail or go early. *Edit* turns it into fields so a name, amount or
day changes in place; *Grid* opens the lot at once and takes a paste straight
out of a spreadsheet. Add a bill any time and the totals, the running column and
the period all follow. A **round-up** setting lifts every bill to the nearest
£1, £5 or £10 as a cushion — it changes what the app asks you to set aside,
never what you typed, and never a payment that has already gone.

**Debt is modelled properly**, because a balance is wrong if you conflate two
different things:

- A **fixed total** knows only what you will hand over. Its balance is
  *payments left* — what it costs to see the agreement out.
- Give it **what you actually borrowed and the rate** and it amortises month by
  month, so the balance becomes the real outstanding principal — what it costs
  to *clear it today*.
- A **credit line** has no term at all. It carries a balance, a rate and
  whatever you choose to pay, and the app works out how long that takes — or
  tells you it never finishes, if the payment doesn't cover the interest.

For a credit line you can say you pay **the minimum** rather than a set amount,
and it models that honestly: a minimum is a flat percentage of the statement, so
it shrinks as the balance does and the term runs to decades. The app puts the
same amount held flat right beside it, and shows how much of each payment is
interest.

A minimum payment is only unpredictable if nobody works it out — the rate and
the balance already determine the whole schedule. So there's a **forecast of
every future payment**: what's due each month, how much of it is interest, and
what the balance will be afterwards, for five years. A bill that services a
minimum-payment credit line reads that forecast instead of a figure you had to
round up and hope, so the month's total is right without maintenance. **Record
a payment** when money actually leaves, and if it's more than the minimum the
balance, the term, the interest and every future minimum all move with it.

**Payments can be recorded on any debt.** On a fixed-term agreement that pins
the count to a date instead of leaving it to arithmetic about start dates — and
that arithmetic is easy to get wrong, because an agreement names the date of the
*first payment*, not the day a clock starts. On a credit line, it snaps the
balance to what actually happened.

Crucially, **the balance accrues on its own**. It's stored against the date it
was last read, and carried forward one cycle at a time — interest on, assumed
payment off — so it stays right whether or not you record anything. Record a
payment and it snaps to what actually happened; type a balance and that becomes
the new anchor. After a couple of months without a real reading it asks you to
confirm from a statement, because a number nobody has checked is how a tracker
quietly stops being true.

The gap between those is interest you have not paid yet, and it's the whole
reason to model it: it tells you what overpaying is worth. Three solvers fill
in whichever of borrowed / payment / term / rate you don't have — including the
APR nobody ever puts in writing. Promotional 0% periods and final balloon
payments are handled.

**Health** is the exercise sheet: 102 lifts grouped by muscle, an inline logger
with a rest timer and a running workout clock, estimated 1RM from Epley, and a
next-weight suggestion that only goes up once every set hits the rep goal. Plus
a training calendar, recent bests, macro targets, recipes and a shopping list.

**Car** turns fuel into a cost per mile. Two fill-ups with odometer readings is
all it needs. Insurance, tax and the MOT are the same ledger records Money
holds — one entry, two pages. The fix list carries prices so planned work lands
in the forecast before it lands on your card.

**Goals** are unlimited, tiered life / year / month / week, and any one can be
pinned to Home. Steps can watch a real number — total debt, a specific debt, a
savings balance, an estimated 1RM, days trained this week — and tick themselves
when it crosses the line. A step can also stay locked until an earlier one is
done, which makes it a tech tree rather than a to-do list.

## How it's built

One `index.html`. No build step, no dependencies, no framework.

Four primitives carry every page, so pages are views over the same store rather
than separate apps bolted together:

| | |
|---|---|
| `ledger`  | money moving on a schedule |
| `debts`   | a ledger line with a term, so it can end |
| `entries` | something written down on a day |
| `targets` | a number with a direction and a finish line |

Reads and writes go through one `load`/`save` pair. Nothing else knows where
the data lives, which is what makes adding a sync backend later a day's work
rather than a rewrite. Every editor in the app is driven by one declarative
form, so adding a field is a line in a list.

## Your data

Nothing personal ships with the app and nothing is uploaded. The exercise
catalogue — names, muscle groups, equipment — is generic reference data and
comes with it; weights, balances and everything else does not. A fresh install
starts empty, and you add things by hand or import a file you exported.

**Storage is per-device, so two devices hold separate books.** Export and
import to move a snapshot between them. Money → *Back up your data* does both.
Keep a copy somewhere that isn't the device it came from.

## Design

Monochrome. The only two colours encode state — ahead, and overdue — never
identity, and emphasis is an inverted fill rather than a hue. Air where you
read, density where you work: overview screens are generous, the bill list and
the exercise sheet are tight and fast. No cards; hairlines and space do the
separating.

## Running it

Any static server:

```
python3 -m http.server 8000
```

Then open `http://localhost:8000`.
