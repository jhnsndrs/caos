# CAOS — 14 Screen Specifications

This document describes the approved screen set represented by the interactive prototype.  
The prototype is the visual source of truth. Base44 must preserve the layout, navigation, terminology, colour system, cards, tables, forms, modals and interaction pattern unless the business owner explicitly approves a change.

## 1. Home / CEO Dashboard

**Purpose:** Daily business cockpit.

**Shows:**
- Net Worth
- Funded Capital
- Cash Available
- Today's Risk
- Open Trades
- Monthly P&L
- YTD P&L
- Equity Overview
- Capital Allocation
- Account Overview
- Recent Alerts
- Cash Flow
- North Star Progress
- Daily, weekly and monthly review cards

**Interactions:**
- Account rows may open Account Detail.
- Alerts link to the affected module.
- Quick actions open the relevant create flow.
- The dashboard is primarily read-only.

## 2. Trading Dashboard

**Purpose:** Analyse realised and open trading activity.

**Shows:**
- Total P&L
- Win Rate
- Profit Factor
- Expectancy
- Average R
- Average Win
- Average Loss
- Equity Curve
- Daily Performance
- Recent Trades

**Interactions:**
- New Trade opens Trade Entry.
- Trade rows may open/edit/close a trade.
- Filters update the displayed metrics and table.
- Realised KPIs use closed trades only.

## 3. Trade Entry

**Purpose:** Create, edit and close trades.

**Fields:**
- Trade Date
- Open Time
- Close Time
- Account
- Instrument
- Direction
- Session
- Timeframe
- Market Condition / Setup
- Entry Price
- Exit Price
- Position Size
- Stop Loss
- Take Profit
- Risk %
- Result
- R Multiple
- P&L
- Fees
- Notes
- Optional screenshots / links

**Interactions:**
- Save Trade validates and creates an Open or Closed trade.
- Live summary reflects selected account, instrument, direction and risk.
- Invalid risk, inactive account or invalid stop placement blocks saving.
- Closing a trade triggers all linked recalculations.

## 4. Capital Dashboard

**Purpose:** Show where business capital is located and how much remains available.

**Shows:**
- Net Worth
- Funded Capital
- Challenge Capital
- Cash Available
- Hedge Capital
- Reserve
- North Star Progress
- Capital Growth
- Capital Allocation
- Account allocation overview

**Important rule:** Allocation is manual. CAOS may calculate and display, but the user decides where money goes.

## 5. Finance Dashboard

**Purpose:** Show realised company finances.

**Shows:**
- Total Revenue
- Total Expenses
- Net Profit
- Cash Flow
- Tax Reserve
- Fixed Costs
- Variable Costs
- Revenue trend
- Income sources
- Expense breakdown
- Tax overview

**Important rule:** Prop-account trading profit is performance, not business cash. A received payout becomes realised company income.

## 6. Risk Dashboard

**Purpose:** Determine whether trading is currently safe and permitted.

**Shows:**
- Trading Permission
- Daily Risk Used
- Remaining Risk
- Open Risk
- Maximum Drawdown
- Rule Violations
- Accounts at Risk
- Risk by Account
- Warnings and recommendations

**Permission states:**
- Trading Allowed
- Reduced Risk
- No Trading

**Important rule:** Risk percentage per trade remains user-configurable.

## 7. Hedge Dashboard

**Purpose:** Manage hedge positions outside prop firms.

**Shows / stores:**
- Exchange: OKX, Hyperliquid, Bluefin or Other
- Linked prop account
- Optional linked trade
- Asset
- Direction
- Hedge amount
- Coverage
- Result
- Status
- Hedge performance and active hedges

**Important rules:**
- Hedge amount is entered manually.
- CAOS must always store where the hedge capital is located.
- A hedge and a prop trade remain separate records.
- Closing one never automatically closes the other.

## 8. Accounts Overview

**Purpose:** List and manage all prop accounts.

**Shows:**
- Total Accounts
- Active Accounts
- Funded Accounts
- Challenges
- Accounts at Risk
- Payouts Ready
- Total P&L
- Account table with firm, type, status, size, equity, drawdown, risk, health and action

**Interactions:**
- Clicking an account opens Account Detail.
- New Account / New Challenge opens the relevant create form.
- Accounts are archived rather than permanently deleted.

## 9. Account Detail

**Purpose:** Complete operational view of one account.

**Shows:**
- Balance
- Equity
- Current Drawdown
- Remaining Drawdown
- Next Payout
- Risk per Trade
- Account P&L
- Equity history
- Account health
- Open and closed trades
- Linked hedges
- Challenge information where relevant

**User-configurable:**
- Preferred risk %
- Internal warning level
- Internal stop level

## 10. Challenge Detail

**Purpose:** Manage one challenge or phase.

**Shows:**
- Account Size
- Current Balance
- Target
- Progress
- Maximum Drawdown Used
- Drawdown Remaining
- Days Active
- Challenge Progress
- Rule Compliance
- Recommended Action

**Statuses:**
- Created
- Active
- Target Reached
- Passed
- Failed
- Funded
- Archived

**Important rule:** Target Reached is not automatically Passed. Official pass/fail requires user confirmation.

## 11. Payout Management

**Purpose:** Register payouts and manually assign the received money.

**Shows / stores:**
- Payout ID
- Source account / income source
- Request Date
- Received Date
- Gross Amount
- Fees
- Net Amount
- Status
- Manual allocations
- Notes

**Allocation destinations may include:**
- Cash
- Reserve
- Tax Reserve
- Challenge Budget
- Hedge Budget
- Investment Budget
- Future/custom destinations

**Important rule:** No fixed automatic allocation. The user decides the destination and amount for every payout separately.

## 12. Expense Management

**Purpose:** Register and analyse business expenses.

**Shows / stores:**
- Date
- Category
- Type
- Description
- Amount
- Payment method
- Linked account where relevant
- Recurring Yes/No
- Status
- Notes

**Example categories:**
- Prop Firm Fees
- Software
- Exchange Fees
- Data
- Hardware
- Education
- Tax
- Other

## 13. Reports & Analytics

**Purpose:** Historical analysis across the business.

**Reports:**
- Trading Performance
- Account Performance
- Challenge Performance
- Capital History
- Cash Flow
- Revenue
- Expenses
- Payout History
- Hedge Performance
- Risk History
- Activity / Audit Log

**Interactions:**
- Filters by date, account, provider, challenge, exchange, instrument, status and category.
- Reports are read-only.
- CSV/PDF export can be added.

## 14. Settings

**Purpose:** Manage configurable values and reference lists.

**Groups:**
- General
- Prop Firms
- Exchanges
- Risk Defaults
- Income Categories
- Expense Categories
- Notifications
- Future Integrations

**Important rules:**
- Settings provide defaults, not forced decisions.
- Changing a setting does not rewrite historical records.
- Strategic values are stored as data and never hardcoded.
