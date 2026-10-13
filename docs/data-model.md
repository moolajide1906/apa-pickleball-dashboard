# Data model

## Tables

**Members** (from the Excel "Members" sheet, one row per member)

| Column | Notes |
|---|---|
| Member ID | Key, e.g. M001 |
| Name | |
| Email (same as Stripe) | Used to match the Stripe export |
| Pays By | Stripe or Former |
| Member Since | Date of first Stripe subscription |
| Ends On (former) | Last paid day for members who cancelled |
| Status | Excel formula: Active, Active (DUPLICATE), Former member, Not active in Stripe |
| Renews / Ends | Next Stripe renewal date |
| Total Visits, Last Visit | Excel formulas over the check-in log |

**CheckIns** (from the Excel "Check-Ins" sheet, one row per person per meetup)

| Column | Notes |
|---|---|
| Date | Meetup date |
| Name | Picked from a dropdown for members, typed for guests |
| Member ID (auto) | Excel lookup from Name to Members; blank for guests who never joined |
| Type | Member or Day Pass |
| Paid Via | Membership, Stripe, Cash or Not recorded |
| Amount ($) | $10 for a day pass, blank for members |

**Dates**: DAX calendar table from Aug 1, 2026 to today.

## Relationships

| From (many) | To (one) |
|---|---|
| CheckIns[Member ID (auto)] | Members[Member ID] |
| CheckIns[Date] | Dates[Date] |

Both are single direction. Filtering check-ins (for example by date or type) does not change the Active Members count, which always reflects who is paying today.

## Power Query steps

**Members**
1. Remove rows where Member ID is null or blank (the sheet has empty template rows below the data).
2. Set Member Since, Ends On (former), Renews / Ends and Last Visit to Date, then replace errors with null.
3. Set Total Visits to Whole Number.

**CheckIns** (renamed from "Check-Ins")
1. Remove rows where Name is null or blank.
2. Set Date to Date and Amount ($) to Decimal Number, then replace errors with null.

## Monthly refresh

1. Export Subscriptions from Stripe as CSV.
2. Paste it into the workbook's "Stripe Subs" tab, replacing the old rows.
3. Save the workbook and click Refresh in Power BI.
