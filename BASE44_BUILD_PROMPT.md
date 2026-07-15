# Base44 Master Build Prompt

Build **CAOS — Capital Allocation Operating System** as a real persistent web application.

## Sources of truth

1. The files in `/prototype` are the exact visual and interaction reference.
2. `SCREEN_SPECIFICATIONS.md` defines the approved 14-screen scope.
3. `ITTT_WORKFLOWS.md` defines the required business behaviour.

Use all three sources together.

## Non-negotiable instructions

- Recreate the prototype UI, navigation, layout, colours, cards, tables, forms and modals as closely as possible.
- Build exactly the 14 approved screens.
- Do not rename screens, objects, fields or status terminology without explicit approval.
- Do not invent extra workflows or modules.
- Do not replace manual decisions with automation.
- Risk percentage per trade must remain user-configurable.
- Hedge amount must remain manually entered.
- Every hedge must store its Exchange: OKX, Hyperliquid, Bluefin or Other.
- Every payout allocation must be manual and specific to that payout.
- Income from hedges and later income streams must remain available for manual payment/allocation decisions.
- A prop trade and a hedge are separate records.
- Official challenge pass/fail/funded status requires user confirmation.
- Use persistent database storage, not browser-only demo state.
- All affected screens must update after a source action according to the ITTT rules.
- Add an immutable audit log for important changes.
- Use archive/cancel states instead of destructive deletion.

## Build sequence

1. Read and summarise all repository files.
2. Confirm that all 14 screens and the major workflows were understood.
3. Build the visual shell and navigation first.
4. Create the database entities and relationships.
5. Implement Accounts and Challenges.
6. Implement Trade creation, editing and closing.
7. Implement Risk and all cross-screen recalculations.
8. Implement Hedges, including Exchange.
9. Implement Payouts and manual allocation.
10. Implement Expenses and other realised income.
11. Connect CEO, Capital, Finance, Reports and all dashboards to real data.
12. Implement validation, alerts, recommendations and audit logging.
13. Run the acceptance scenarios below.
14. Do not make unapproved product decisions when a specification is unclear; ask.

## Minimum database entities

- User
- Provider / Prop Firm
- Account
- Challenge
- Trade
- Hedge
- Exchange
- Payout
- Payout Allocation
- Income
- Expense
- Capital Bucket / Allocation Transaction
- Setting
- Alert
- Recommendation
- Audit Log
- Attachment / Note where supported

## Initial acceptance scenarios

### A. Open trade
Create an Open trade. Verify Open Trades and risk increase, while realised P&L and Win Rate do not change.

### B. Winning close
Close it at +2R. Verify all trading KPIs, account balance, challenge progress, risk, CEO and reports update.

### C. Losing close and warning
Close a trade that moves account drawdown above its warning level. Verify Attention / Reduce Risk and alerts.

### D. No Trading
Move drawdown above the configured internal stop level. Verify new trades are blocked for that account.

### E. Hedge location
Open a hedge on Hyperliquid. Verify the Hedge record and table clearly show Hyperliquid.

### F. Linked hedge remains open
Close the prop trade while its linked hedge remains Open. Verify a warning appears and the hedge is not automatically closed.

### G. Received payout
Change a payout from Pending to Received. Verify it becomes realised revenue/cash and opens manual allocation.

### H. Flexible allocation
Allocate one payout 40/30/20/10 and another payout using different amounts. Verify both are accepted and stored independently.

### I. Hedge income / other income
Register realised hedge income. Verify Finance and Cash update without automatically spending or allocating it.

### J. Historical integrity
Change a default risk setting. Verify historical trades and reports do not change.

## Completion condition

The build is complete only when the UI matches the prototype, all 14 screens exist, data is persistent, and every ITTT workflow works across all affected screens.
