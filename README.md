# Offer Copilot

A clickable prototype of an AI copilot inside a payments dashboard that helps a merchant decide **which offer to run, who pays for it, and whether it actually worked**.

Built by Akshay Pratap Singh (ISB PGP 2027) for the Razorpay AI x PM Build Challenge, Track 2: Recover and Grow with AI. This is a concept, not an official Razorpay product.

## What it does

1. **Suggestions**: spots growth openings in payment data (for example, Kotak cardholders before Diwali, carts stalling just under ₹2,000, lapsed repeat buyers) and shows the signals behind each one.
2. **Three plans per opening**: Careful, Balanced and Big push, each with extra profit, discount cost, and how much of the discount goes to people who would have bought anyway.
3. **Offer editor with live impact**: change discount, cap, minimum order, duration and redemption cap, and the profit estimate, notes and profit curve update instantly.
4. **Bank co-funding flow**: choose "Paid by me", "Shared with bank" or "Paid by bank". Bank-funded offers go to the bank for approval through Razorpay, with a status tracker, counter offers and a fallback the merchant picks in advance.
5. **Guardrails**: hard budget, autonomy level for the AI, one discount per card per day, refund spike pause, and a 10% holdout group.
6. **Live alerts**: the copilot flags offers running hot, card abuse and new openings. The merchant accepts, changes or ignores each one.
7. **Results**: real lift measured against the holdout, plus what the copilot learned for next time.

## Data and assumptions

All data is synthetic. The estimate models four things: buyers who would purchase anyway, buyers the discount brings in, buyers who grow their basket to reach the minimum, and fatigue after the first week. With real data, these would be calibrated on issuer-level payment history and validated with holdout tests.

The "Demo control" box on bank requests simulates the bank's reply, since there is no live bank integration.

## Run it

It is a single static file. Open `index.html` in a browser, or deploy the folder to any static host (it is hosted on Vercel).
