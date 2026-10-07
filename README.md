# Mobile Money Nano-Credit

An educational guide to **nano-credit on a mobile money platform**: small, short-term loans and overdrafts delivered straight into a mobile money wallet, how they are scored, disbursed, and repaid, and the controls that keep them sustainable. Written for telecom operators, fintechs, and lenders evaluating platforms in the [EVD System](https://evdsystem.com/) and [MoboGage](https://evdsystem.com/about-mobogage/) family.

---

## What Is Mobile Money Nano-Credit?

**Nano-credit** (also called nano-lending or micro-loans on mobile) is a very small loan, typically for days or weeks, offered to a mobile money customer and paid into their wallet within moments of acceptance. Repayment is usually taken from the same wallet, either when the customer pays it back or automatically as money comes in.

It works because the operator already sees useful data: how long a customer has used the SIM and the wallet, how regularly they top up airtime, how money moves in and out. That data can support a lending decision for people who have no formal credit history.

### Common product shapes

- **Wallet overdraft** — the customer can complete a payment even when the balance is short, and the shortfall is recovered from the next inflow  
- **Fixed-term nano-loan** — a set amount for a set period with a single fee or interest charge  
- **Merchant and agent working capital** — short loans to buy stock or top up agent float  
- **Buy now, pay later at merchants** — the wallet pays the merchant, the customer repays in instalments  
- **Emergency airtime or data credit** — a close cousin that advances airtime rather than cash  

---

## Architecture Overview

```
Customer requests loan (USSD / app)
        │
        ▼
Eligibility check ──► KYC tier, account age, active status, existing loans
        │
        ▼
Credit scoring ─────► wallet and telecom behaviour, repayment history
        │
        ├──► Offer: amount, fee, due date ──► customer accepts with PIN
        │                                         │
        │                                         ▼
        │                          Disbursement into wallet (loan ledger + wallet ledger)
        │                                         │
        │                                         ▼
        │                Repayment: manual, auto-sweep on inflow, or scheduled
        │                                         │
        │                                         ▼
        │                   Overdue handling: reminders, limits, collections
        │
        └──► Decline ──► reason shown, retry rules
```

### Core components

| Component | Responsibility |
|-----------|----------------|
| **Eligibility service** | Hard rules: KYC level, account age, blacklist, active loans |
| **Scoring engine** | Produces a limit and risk band from behavioural data |
| **Offer service** | Turns a score into a concrete amount, fee, and term |
| **Loan ledger** | Tracks principal, fees, repayments, and status per loan |
| **Wallet integration** | Disburses into and collects from the mobile money wallet |
| **Lender settlement** | Moves funds between the operator and a partner bank or lender |
| **Collections** | Reminders, auto-recovery, write-off, and reporting |

See [docs/glossary.md](./docs/glossary.md) for terminology.

---

## How Nano-Credit Works, Step by Step

### 1. Agree who is the lender

In many markets, a licensed bank or lender holds the loan book and the mobile money operator provides the channel, data, and wallet. This split shapes the contracts, the ledgers, and the reporting.

### 2. Start small and earn limit growth

New customers usually start with a low limit. Repaying on time raises it. This "graduation" model is how lenders build a book safely when they have little prior data.

### 3. Disburse and record twice

A disbursement updates the loan ledger (new receivable) and the wallet ledger (new balance). Both must post together or not at all, so a failed step never leaves money moved without a loan record.

### 4. Make repayment easy

Customers repay more reliably when repayment is a single menu option, and when partial payments are allowed. Auto-sweep from incoming funds is common but should be explained clearly at the offer stage.

### 5. Handle overdue loans fairly

Reminders by SMS, then limit suspension, then collections. Clear fees and no surprise deductions help both regulators and customer trust.

---

## Design Considerations

- **Responsible lending** — show the full cost upfront, avoid pushing repeat borrowing, and respect local consumer credit rules  
- **Data consent** — use telecom and wallet data for scoring only with the consent and legal basis local law requires  
- **Model monitoring** — default rates shift with seasons and economic conditions; review scorecards regularly  
- **Fraud** — watch for SIM-swap takeovers that borrow and cash out immediately  
- **Reconciliation** — the loan ledger, wallet ledger, and lender bank account must reconcile daily  
- **Explainable declines** — customers and support teams need a reason they can understand  

Platforms such as EVD System / MoboGage provide mobile money, wallets, and charging and emergency credit capabilities, so nano-credit can sit alongside airtime advances on the same subscriber base.

---

## FAQ

**What is nano-credit in mobile money?**  
A very small, short-term loan delivered into a mobile money wallet, usually scored on telecom and wallet behaviour and repaid from the same wallet.

**How is nano-credit different from microfinance?**  
Microfinance loans are usually larger and handled through branches or loan officers. Nano-credit is fully digital, much smaller, and decided in seconds.

**Who funds mobile money loans?**  
Usually a licensed bank or lending partner, with the operator providing the channel and data. Arrangements depend on local regulation.

**What data is used for nano-credit scoring?**  
Typically account age, airtime top-up patterns, wallet inflows and outflows, and prior loan repayment, used within the consent rules of the market.

**How does this relate to MoboGage / EVD System?**  
MoboGage offers a mobile money platform within the EVD System family. See the [mobile money platform with merchant payment](https://evdsystem.com/mobile-money-platform-with-merchant-payment/) page and [about MoboGage](https://evdsystem.com/about-mobogage/).

---

## Further Reading / Related Industry Resources

- [Mobile money platform with merchant payment](https://evdsystem.com/mobile-money-platform-with-merchant-payment/) — MoboGage mobile money product context  
- [About MoboGage](https://evdsystem.com/about-mobogage/) — company background  
- Related repo: [mobile-money-platform-overview](https://github.com/iwtglobal/mobile-money-platform-overview) — the core building blocks of a mobile money platform  

---

## License

Documentation in this repository is provided under the MIT License. See [LICENSE](./LICENSE).

*Educational material only. Not legal, regulatory, credit, or financial advice. MoboGage and EVD System refer to offerings on evdsystem.com.*
