# CAOS — ITTT Business Rules and Workflows

ITTT means: **IF THIS, THEN THAT**.

The user enters or changes information once. CAOS updates every affected object, calculation, screen, warning, recommendation and report automatically.

## Global Processing Order

For every meaningful action:

1. Validate input.
2. Check account and business permissions.
3. Save or update the source record.
4. Run the relevant calculation engines.
5. Update every affected module.
6. Create or resolve alerts and recommendations.
7. Write an audit-log entry.
8. Refresh the UI without requiring manual reload.

---

# Workflow 1 — Open a Trade

## IF
The user enters a valid trade and selects **Save Trade** with status `Open`.

## Preconditions
- Account exists.
- Account is Active.
- Required fields are completed.
- Risk % is greater than zero.
- Risk does not violate the active account rules.
- Trading Permission is not `No Trading`.
- Long stop-loss is below entry.
- Short stop-loss is above entry.
- Possible duplicate is checked.

## THEN — Trade
- Create a unique Trade ID.
- Store account, date/time, instrument, direction, entry, stop, optional TP, risk %, setup, timeframe, session and notes.
- Set status to `Open`.
- Calculate risk amount when account size is known.
- Store optional Linked Hedge ID.
- Show a success message.

## THEN — Trading Dashboard
- Open Trades increases by 1.
- The trade appears in Open Trades and Recent Activity.
- Open Risk increases.
- Available Risk decreases.
- Closed-trade statistics do not change.
- Realised P&L does not change.
- Win Rate, Profit Factor and Average R do not change.

## THEN — Account Detail
- Trade appears under Open Positions.
- Account Open Risk increases.
- Remaining Risk decreases.
- Account Health is recalculated.
- Trading Permission is recalculated.

## THEN — Challenge Detail
- Trade appears under Open Positions.
- Open risk changes.
- Realised progress and remaining target do not change until the trade closes.
- Recommendation is recalculated.

## THEN — Risk Dashboard
- Portfolio and account Open Risk increase.
- Remaining Risk decreases.
- Permission and warnings are recalculated.

## THEN — CEO Dashboard
- Open Trades and Today's Risk update.
- Risk alerts and recommendations update if required.
- Monthly and YTD realised P&L do not change.

## THEN — Reports
- Open-position count, exposure and activity may update.
- Realised performance reports do not change.

## THEN — Audit
- Log Trade Created with timestamp, user and source values.

---

# Workflow 2 — Edit an Open Trade

## IF
The user edits stop-loss, take-profit, notes, tags or actual risk/position information.

## THEN
- Save permitted changes.
- Recalculate Open Risk if risk exposure changed.
- Update Account Detail, Risk Dashboard, CEO Dashboard and recommendations.
- Do not alter realised statistics.

## Restrictions
- Account, instrument, direction, entry and original open time define the trade identity.
- Changing those identity fields requires a clear warning.
- A trade cannot be moved to another account. Correct by cancelling the erroneous record and creating the correct trade.
- Raising risk must pass the same validation as a new trade.
- Lowering risk can remove warnings or improve permission.

---

# Workflow 3 — Close a Winning Trade

## IF
An Open trade is closed with Result R greater than 0.

## Required close fields
- Close Date
- Close Time
- Exit Price
- Result R or sufficient data to calculate it
- Fees
- Status Closed

## THEN — Trade
- Set status to `Closed`.
- Store close data.
- Calculate gross result.
- Subtract fees.
- Store net result.
- Classify as `Win`.
- Remove its Open Risk.

## THEN — Trading Dashboard
- Open Trades decreases by 1.
- Closed Trades and Total Trades increase by 1.
- Winning Trades increases by 1.
- Recalculate Win Rate, Total R, Average R, Gross Profit, Profit Factor, Expectancy and Trading P&L.
- Add a new equity-curve point.
- Show the trade in Recent Trades.

## THEN — Account
- Balance increases by the net result.
- Equity is recalculated.
- Current Drawdown and Remaining Drawdown are recalculated.
- Account P&L and health update.
- Payout readiness is reevaluated.

## THEN — Challenge
- Balance and progress increase.
- Remaining target decreases.
- Drawdown and recommended action update.

## IF target is reached
- Status becomes `Target Reached`.
- Recommendation becomes `Stop Trading / Await Confirmation`.
- Block new trades on this account until user review.
- Create CEO and Account alerts.
- Do not automatically mark Passed.

## THEN — Risk
- Open Risk decreases.
- Remaining Risk increases.
- Permission and warnings update.

## THEN — CEO
- Monthly P&L, YTD P&L and Net Worth update.
- Open Trades and Today's Risk decrease.
- Alerts may resolve.
- Target Reached or Payout Ready alert may appear.

## THEN — Finance / Capital
- Trading performance updates.
- Available company cash does not increase.
- Company revenue and cash change only after a payout is received.

## THEN — Reports and Audit
- Recalculate all affected trading, account and challenge reports.
- Log Trade Closed.

---

# Workflow 4 — Close a Losing Trade

## IF
A trade is closed with Result R below 0.

## THEN
- Classify as `Loss`.
- Remove Open Risk.
- Recalculate all trading statistics.
- Account balance and equity decrease.
- Current Drawdown increases.
- Remaining Drawdown decreases.
- Challenge progress may decrease and remaining target may increase.
- Account Health, Trading Permission, warnings and recommended action are recalculated.
- CEO and Reports update.

## Important
It is possible for Open Risk to decrease while Trading Permission worsens, because the realised loss increases drawdown.

---

# Workflow 5 — Close a Break-even Trade

## IF
Result R equals 0.

## THEN
- Status becomes Closed.
- Classification becomes `Break-even`.
- Total closed trades increases.
- Win and Loss counts do not increase.
- Win Rate uses Wins divided by all closed trades, including break-even trades.
- Total R remains unchanged.
- Fees may create a small negative monetary result.
- Open Risk is removed.
- Account and dashboards update.

---

# Workflow 6 — Cancel a Trade

## IF
A recorded trade was never executed.

## THEN
- Require a cancellation reason.
- Set status to `Cancelled`.
- Remove Open Risk.
- Do not affect realised statistics, account balance or challenge progress.
- Keep the record and audit trail.

## Restriction
An executed market position cannot be cancelled to remove a result. It must be closed with the actual result.

---

# Workflow 7 — Correct a Closed Trade

## IF
The user corrects a factual error in a Closed trade.

## THEN
- Keep the original values in the Audit Log.
- Store corrected values.
- Recalculate trading statistics.
- Recalculate account balance and drawdown.
- Recalculate challenge progress.
- Recalculate risk, CEO KPIs and reports.
- Show the consequences before confirming major corrections.

---

# Workflow 8 — Drawdown Warning

## IF
Account drawdown reaches or exceeds its configurable Warning Level.

## THEN
- Account Health becomes `Attention`.
- Recommended Action becomes `Monitor` or `Reduce Risk`.
- Show a warning on Risk Dashboard, CEO Dashboard and Accounts Overview.
- Recalculate the permitted risk level.

---

# Workflow 9 — Internal No-Trade Level

## IF
Drawdown reaches or exceeds the configurable internal Stop Level.

## THEN
- Account Health becomes `Critical`.
- Trading Permission becomes `No Trading`.
- Block New Trade for that account.
- Show a critical alert and `Protect Account / Review` recommendation.

---

# Workflow 10 — Official Failure Limit

## IF
Internal calculations indicate the provider maximum drawdown is reached or exceeded.

## THEN
- Mark the account `Potentially Failed`.
- Block new trades.
- Ask the user to confirm the provider's official outcome.

## IF user confirms Failed
- Set account and challenge status to `Failed`.
- Remove the account from Active Capital.
- Retain all history and expenses.
- Update CEO, Risk, Capital, Accounts and Reports.

CAOS does not automatically confirm official failure because provider calculations may differ.

---

# Workflow 11 — Create an Account / Challenge

## IF
The user creates an account.

## THEN
- Create a unique Account ID.
- Store provider, account type, size, start date, status and user-configurable risk values.
- If it is a challenge, create or link a Challenge record.
- Add it to Accounts Overview.
- Initialise Account Detail, Risk Profile and Challenge Detail.
- Update Active Accounts, Funded Capital or Challenge Capital.
- Write Audit Log.

---

# Workflow 12 — Challenge Target Reached, Passed or Failed

## IF
Calculated challenge progress reaches or exceeds target.

## THEN
- Set status to `Target Reached`.
- Block new trades until review.
- Recommend `Stop Trading / Await Confirmation`.
- Create CEO alert.

## IF user confirms Passed
- Set Challenge to `Passed`.
- Update Account status.
- Update CEO, Accounts and Reports.

## IF user confirms Funded
- Set Account type/status to Funded.
- Apply funded risk configuration defaults, while allowing user adjustment.
- Update Funded Capital and dashboards.

## IF user confirms Failed
- Set Challenge and Account to Failed.
- Set permission to No Trading.
- Retain all history.

---

# Workflow 13 — Open a Hedge

## IF
The user saves a new hedge.

## Required
- Exchange: OKX, Hyperliquid, Bluefin or Other
- Linked Account
- Asset
- Direction
- Hedge Amount
- Open Date
- Status

## THEN
- Create unique Hedge ID.
- Store the Exchange so CAOS knows where the money is.
- Store optional Linked Trade.
- Increase active hedge capital.
- Update Hedge Dashboard, Capital Dashboard, CEO activity and Reports.
- Write Audit Log.

## Important
- Hedge Amount is manually chosen.
- Hedge and Trade are independent records.

---

# Workflow 14 — Close a Hedge

## IF
A hedge is closed.

## THEN
- Store close data and realised hedge result.
- Set status to Closed.
- Release active hedge capital.
- Update Hedge P&L, Capital and Finance.
- Update CEO and Reports.
- Do not alter the linked prop-trade result.
- Write Audit Log.

## IF linked prop trade is closed while hedge remains open
- Warn: `The linked hedge is still open.`
- Offer to open the hedge record.
- Never close it automatically.

---

# Workflow 15 — Register a Payout

## IF
A payout is created as Pending / Requested.

## THEN
- Store the payout.
- Show it in Payout Management.
- Do not recognise revenue or cash yet.

## IF status becomes Received
- Store Received Date and final Net Amount.
- Recognise it as realised company revenue and cash.
- Open a manual allocation step.
- Update Finance and CEO.

---

# Workflow 16 — Manually Allocate a Received Payout

## IF
The user assigns amounts to destinations.

## Destinations may include
- Cash
- Reserve
- Tax Reserve
- Challenge Budget
- Hedge Budget
- Investment Budget
- Custom / future categories

## Validation
- Every amount is zero or greater.
- Total allocation must equal the payout's net amount.
- Allocation cannot exceed available unallocated payout amount.

## THEN
- Store the exact manual allocation.
- Update each capital bucket.
- Update Capital, Finance, CEO and Reports.
- Write Audit Log.

## Important
- No fixed percentages.
- No automatic allocation.
- Each payout can have a completely different allocation.

---

# Workflow 17 — Add Income from Hedge or Future Sources

## IF
The user registers realised non-payout income.

## THEN
- Store source, date, amount and notes.
- Increase Revenue and Available Cash.
- Let the user manually decide later where to allocate/pay the money.
- Update Finance, Capital, CEO and Reports.

CAOS does not automatically pay expenses, taxes, challenges or investments from that income.

---

# Workflow 18 — Add an Expense

## IF
A Paid expense is saved.

## THEN
- Create unique Expense ID.
- Reduce Available Cash by the amount.
- Increase Expenses.
- Recalculate Net Profit and Cash Flow.
- Update Finance, Capital, CEO and Reports.
- Write Audit Log.

## IF status is Planned
- Show it as planned.
- Do not reduce cash until Paid.

---

# Workflow 19 — Edit or Archive Financial Records

## IF
A payout, income or expense is corrected.

## THEN
- Require confirmation for material changes.
- Preserve previous values in Audit Log.
- Recalculate all dependent finance, capital, CEO and report values.

## Deletion rule
Business records are not permanently deleted. Use Cancelled or Archived.

---

# Workflow 20 — Change Settings

## IF
A configurable default is changed.

## THEN
- Apply it to new records and future decisions.
- Do not rewrite historical records.
- Recalculate only current derived permissions/recommendations where appropriate.
- Write Audit Log.

Examples:
- Preferred risk defaults
- Warning levels
- Stop levels
- Exchanges
- Categories
- Provider rules

---

# Global Business Rules

1. Open trades affect Open Risk, not realised performance.
2. Closed trades affect realised performance, account balance and challenge progress.
3. Prop trading profit is not business cash until a payout is received.
4. A received payout is realised company revenue and cash.
5. Risk % per trade is chosen/configured by the user.
6. Hedge amount per trade is chosen by the user.
7. Payout allocation is decided manually for each payout.
8. Income from hedges and future sources is registered manually and may be spent/allocated manually.
9. Trade and Hedge remain separate records.
10. CAOS never automatically closes a hedge or prop trade.
11. Official pass/fail/funded status requires user confirmation.
12. Configurable values are data, never hardcoded.
13. Historical records retain the configuration and facts that applied at that time.
14. Important changes create an immutable Audit Log entry.
15. Business records are archived/cancelled rather than permanently deleted.
