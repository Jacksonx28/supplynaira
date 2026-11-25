# SupplyNaira

A Lagos importer who earns in dollars pays his local suppliers the exact naira amount on their invoices, straight from USD₮ — and only when the exchange rate meets his floor.

SupplyNaira is an agent for the **Stablecoin Corridors** track of Celo's *Agents Open for Business* hackathon (Loops House). It converts USD₮ to cNGN on **Textile FX** (Celo mainnet), pays the supplier's wallet, and issues a public receipt for every payment.

## The job, in one flow

1. The importer enters three things: the supplier's wallet, the invoice amount in cNGN (the exact amount that must land), and his floor rate (the minimum NGN per USD₮ he will accept).
2. The agent requests a firm Textile FX quote (exact-out, bound to the taker wallet).
3. **Floor check, in deterministic code — no LLM touches money.** If the effective rate is below the floor, the agent refuses, records the refusal with the quote, and stops. If the rate meets the floor, it proceeds.
4. The agent executes the tagged Textile FX conversion (attribution tag appended to the swap calldata) and pays the exact cNGN amount to the supplier.
5. A public receipt is issued: invoice amount, rate achieved vs floor and vs benchmark, conversion tx hash, payment tx hash, supplier wallet, timestamp.

The refusal is the point: an agent that will not move your money at a bad rate is one you can trust with a real invoice.

## Why this job

- Supplier payments for a small importer is a sponsor-named idea for this track, and the emptiest lane in the published field: every published Corridors build found at scan time (25 Nov 2026) was freelancer/contractor payroll.
- People get paid and spend in naira. Importers earn in dollars and owe in naira; the gap between the two is where their margin leaks.

## Track requirements — how this entry meets them

| Requirement | How SupplyNaira meets it |
|---|---|
| At least one tagged Textile FX conversion on Celo mainnet in a listed corridor, used in the product flow | USD₮ → cNGN conversions are the product flow; the attribution tag is appended to `transactions.swap.data` before sending |
| Payments in a local stablecoin reach a recipient outside the team | cNGN is paid to the named supplier's wallet, outside the team and not funded by the team |
| ERC-8004 identity | The agent registers on the ERC-8004 Identity Registry (`0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`) before its first counted transaction; see `agent/registration.json` |
| No arbitrage, self-trading, or acting as a Textile maker | None. Every conversion settles a real supplier invoice |

## Judging (Stablecoin Corridors, 25% each)

| Criterion | Evidence in this repo / entry |
|---|---|
| Quality of execution | Deterministic floor guard, exact-out quotes, one-transaction settlement, public receipts |
| Clear value proposition | One sentence, above: exact naira invoice paid from dollars, only at your rate or better |
| Initial traction with tagged onchain activity | Live proof table below, filled as real payments land |
| A named user with a real job done | Named importer and supplier, added when the first real payment is made |

## Live proof

_Status as of 25 Nov 2026: design complete, build not yet started. This table is filled only with real mainnet transactions — nothing here is simulated._

| # | Date | Invoice (cNGN) | Rate achieved | Floor | Conversion tx | Payment tx | Recipient |
|---|---|---|---|---|---|---|---|
| — | — | — | — | — | — | — | — |

## Corridor facts (measured live, 25 Nov 2026)

- USDT→cNGN on Textile FX: last 1441.57 NGN per USD₮, depth to ~45,547 USD₮, minimum 1 USD₮
- Preview: 5 USD₮ → 7,207.83 cNGN at 1441.57, fee 0.000999 USD₮ (2 bps)

## Key addresses (Celo mainnet, chain 42220)

- USD₮: `0x48065fbBE25f71C9282ddf5e1cD6D6A887483D5e`
- cNGN: `0xF6829D7393dAe24509eb1E52eE8e572e2E271a4f`
- Textile LimitOrderReactor: `0xa9AA0a64769cBed4d3B1Ceb4Df01CdE915C235b3`
- ERC-8004 Identity Registry: `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`

## Not built yet

- Agent code (quote → floor check → tagged swap → payment → receipt)
- ERC-8004 registration transaction
- First live supplier payment
- Demo video

Build window: 1–4 Dec 2026, per the project plan. Hard kill point 4 Dec: if no live tagged mainnet conversion exists by then, this entry is dropped.

## Licence

Apache-2.0 — see `LICENSE`.
