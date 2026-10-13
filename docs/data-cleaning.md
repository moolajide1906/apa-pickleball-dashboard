# Data cleaning

The club's attendance and membership data is collected by hand at each meetup. Before building the dashboard, I cleaned the existing tracker and redesigned it so it stays clean.

## Problems in the original tracker

- **Placeholder rows.** The full member list was pasted into the check-in sheet before every meetup, and attendees got a date. Over 100 rows were members who didn't attend.
- **Inconsistent labels.** Attendance type had eight spellings ("Member ", "Member (today)", "Day pass - Website", "day pass - cash" and more).
- **Name variations.** The same person appeared under several names (nicknames, typos, extra spaces and non-breaking spaces).
- **Emails out of sync with Stripe.** Two members had each other's emails, some emails didn't match Stripe, and the newest members had none.
- **Duplicate check-ins.** The same person was logged twice at one meetup.

## What I changed

1. Removed rows with no check-in date, since no date means the person didn't attend.
2. Standardized Type to **Member** or **Day Pass**, and moved payment method into its own **Paid Via** column.
3. Merged name variations into one name per person and removed duplicate check-ins.
4. Matched every member to Stripe by email and corrected the mismatches.
5. Matched day-pass guests to Stripe $10 payments where the name and email lined up.

## What the reconciliation found

- One member had **two active subscriptions** and was being charged twice a month.
- Two members had **stopped paying** but were still marked "Paid".
- One member signed up in Stripe with a **mistyped email**, so receipts weren't reaching them.

## How the tracker stays clean now

- At each meetup, add a row only for people who attend.
- Member names come from a dropdown, so they always match the Members list. Member ID fills in automatically.
- Type and Paid Via are dropdowns, so labels can't drift.
- Membership status is calculated from the Stripe export, not typed by hand.
- When someone cancels, they're marked **Former** instead of deleted, so their past visits still count.
