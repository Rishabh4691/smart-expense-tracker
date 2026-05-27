# smart-expense-tracker 💸

Forward any receipt or expense photo to a Telegram bot. AI reads it, extracts the amount, category, and merchant, and logs it automatically to a Google Sheet. At the end of the week you get a summary sent to your phone.

No manual data entry. Ever.

---

## The problem it solves

You buy a coffee, get a receipt, take a photo, forget about it. Repeat 40 times a month. By the end of the month you have no idea where your money went.

This fixes that. You forward the receipt photo to a Telegram bot. Done. Everything else is automatic.

---

## How it works

```
You forward receipt photo to Telegram bot
        ↓
n8n Telegram Trigger receives image
        ↓
Image sent to Groq Vision / OpenAI Vision
Extracts: amount, merchant, category, date
        ↓
Data written to Google Sheets row
        ↓
Telegram confirms: "Logged €4.50 — Coffee ☕"

Every Sunday 18:00:
        ↓
n8n reads full week from Google Sheets
        ↓
Groq summarises spending by category
        ↓
Weekly report sent to Telegram
"This week: €340 total
 🍔 Food: €120
 🚗 Transport: €45
 ☕ Coffee: €38 (you drink a lot of coffee)"
```

---

## Stack

| Tool | Purpose |
|---|---|
| n8n | Workflow orchestration |
| Telegram Bot | Input (you send receipts) + output (confirmations) |
| Groq LLM with vision | Extract expense data from receipt images |
| Google Sheets | Store all expense records |
| Google Sheets API | Read data for weekly summary |

---

## Google Sheet structure

| Date | Merchant | Amount | Currency | Category | Raw text |
|---|---|---|---|---|---|
| 2026-05-27 | Rewe | 23.40 | EUR | Groceries | Rewe Schweinfurt... |
| 2026-05-27 | Starbucks | 5.20 | EUR | Coffee | Starbucks receipt... |

---

## Setup

### 1. Create a Telegram bot
Message @BotFather on Telegram → `/newbot` → save the token

### 2. Create a Google Sheet
Create a new sheet with columns: Date, Merchant, Amount, Currency, Category, Notes

### 3. Import workflow into n8n
Upload `workflow.json` → add credentials → activate

### 4. Required credentials
- `TELEGRAM_BOT_TOKEN`
- `GROQ_API_KEY`
- Google Sheets OAuth credentials

### 5. Start logging
Forward any receipt image to your Telegram bot and watch it appear in the sheet instantly.

---

## Expense categories (auto-detected by AI)

- 🍔 Food & Restaurants
- ☕ Coffee
- 🚗 Transport
- 🛒 Groceries
- 🏥 Health
- 🎮 Entertainment
- 📱 Subscriptions
- 🏠 Home
- 💼 Work / Education
- ❓ Other

---

## Weekly summary example

```
💸 Weekly Expense Report — Week 21

Total spent: €287.40

By category:
🛒 Groceries    €89.20  ████████░░  31%
🍔 Food         €67.50  ██████░░░░  23%
☕ Coffee        €38.00  ████░░░░░░  13%
🚗 Transport    €34.70  ███░░░░░░░  12%
📱 Subscriptions €22.00  ██░░░░░░░░   8%
🎮 Entertainment €18.00  █░░░░░░░░░   6%
💼 Education     €18.00  █░░░░░░░░░   6%

Most visited: Rewe (4x), Starbucks (3x)

vs last week: +€23 📈 You spent more on food.
```

---

## Why I built this

Manually logging expenses takes discipline. Forwarding a photo takes zero discipline. The activation energy difference is what makes this actually work long-term.

---

*Vibe coded with n8n + Groq vision. Data stays in your own Google Sheet — nothing stored externally.*
