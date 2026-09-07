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

**Money** keeps a bill list that ticks itself off as the date passes, with a
manual override for the ones that fail or go early. *Edit* turns the list into
fields so a name, amount or day changes in place; *Grid* opens the lot at once
and takes a paste straight out of a spreadsheet. There's a subscription audit
sorted by annual cost, annual bills amortised to monthly, and a five-year
projection of your net position that steps up each time a debt ends.

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
and it models that honestly: a minimum is a flat percentage of the balance, so
it shrinks as the balance does and the term runs to decades. The app puts the
same amount held flat right beside it, shows how much of each payment is
interest, and flags it when the bill you budgeted doesn't match what actually
leaves your account.

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
