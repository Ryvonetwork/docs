# OWS Demo Flow

Status: Working MVP  
Date: 2026-04-04

## One-Line Pitch

Agon Gateway is an OWS-powered RPC gateway that makes x402-style API payments
affordable through deferred settlement on Solana.

Instead of forcing exact per-call payment, Agon Gateway meters Solana RPC calls
at micro-prices such as `$0.000001` and settles later through Agon Protocol.

## Core Stack

- **OWS**: local-first wallet access and policy-gated signing
- **Agon Protocol**: deposits, lanes, locked funds, cumulative commitments, settlement
- **Agon Gateway**: metering, rate limits, pricing, upstream proxy
- **Alchemy**: upstream Solana RPC
- **x402**: refill path for buying AGON API credits

## Current Devnet Constants

- Program: `9Kwxe9mqisMPsyFknepXAahSodEymFGgkFwqYJHvp45K`
- Gateway payee wallet: `9418vqXsaVbwUEP5q8FGtRxEtzrDgDc2ECfHrWWthvPQ`
- Gateway payee participant id: `18`
- Accepted payment token: `AGON`
- AGON token id: `2`
- AGON mint: `5qyByZdW9LNCUHm5DBQSVb4XxTt7w8aVuj9BLgdBJP5y`

## Important OWS Note

On this Windows + WSL development machine, the official Linux OWS CLI is used
through WSL. The current npm-installed Windows shim reports
`unsupported platform win32-x64`, so the demo uses the official CLI where it
works cleanly out of the box.

## Demo Narrative

### Step 1: Install OWS

Show:

```bash
npm install -g @open-wallet-standard/core
```

Narration:

- user installs OWS
- OWS manages the local wallet vault
- the app does not ask the user to type payment signatures manually

### Step 2: Create or Import the Wallet

Show:

```bash
ows wallet import --name my-agent --private-key --chain solana
```

Narration:

- the wallet is now available inside the OWS vault
- the same wallet is used for Agon onboarding and OWS payment signing

### Step 3: Ask the Gateway for an x402 Top-Up Quote

Show:

- `GET /topups/x402/quote?walletAddress=<wallet>`
- `POST /topups/x402/confirm`

Narration:

- the gateway quotes AGON API credits
- the gateway funds the wallet after the quote is confirmed
- for the hackathon demo this is the explicit x402 refill step

### Step 4: Register with Agon and Deposit AGON

Show:

- register participant
- deposit AGON into the Agon vault

Narration:

- this creates the settlement identity inside Agon
- AGON is the access credit token accepted by the gateway

### Step 5: Call Agon Gateway Too Early

Show:

- user sends an RPC call to Agon Gateway after deposit
- gateway rejects it with `missing-channel`

Narration:

- the user has funds, but no reusable payment lane yet
- that makes the lane model understandable

### Step 6: Create the Lane but Do Not Lock Funds Yet

Show:

- create unilateral channel: `user -> gateway`
- attempt another RPC call
- gateway rejects it with `missing-locked-funds`

Narration:

- the lane exists now
- but no spending capacity has been reserved for streaming-like API use

### Step 7: Lock Funds and Make Paid RPC Calls

Show:

- lock AGON into the lane
- make repeated calls to wrapped Alchemy Solana RPC
- every request includes a new OWS-signed `agon-cmt-v3`
- in the live demo the cumulative amount advances from `0 -> 1 -> 2`

Narration:

- gateway reads lane state directly from Agon devnet
- gateway verifies the OWS-signed cumulative payment update
- gateway forwards the request to Alchemy
- gateway records the latest accepted cumulative amount off-chain

### Step 8: Show Deferred Settlement and Bundle Settlement

Show:

- `GET /settlement/preview`
- `POST /settlement/run`
- the channel `settled_cumulative` advancing on-chain

Narration:

- the gateway only stores the latest accepted commitment per lane
- it settles those latest commitments later with `settle_commitment_bundle`
- this is where affordability comes from: many calls, one on-chain settlement

### Step 9: Show Final Gateway State

Show:

- gateway session for the user
- settlement preview / usage records

Narration:

- after bundle settlement the pending set is empty
- the lane's `settled_cumulative` has advanced
- the gateway can keep serving calls and settle again later

## What the Gateway Should Say

The gateway should make the payment model explicit:

- no participant account found
- no payment lane found
- no locked funds in lane
- cumulative commitment is stale
- cumulative commitment is too small for this method

## Submission Message

Use this framing:

**OWS-powered affordable RPC API calls via Agon Protocol's deferred settlement**

Longer version:

Agon Gateway wraps Alchemy's Solana RPC behind an OWS-powered payment layer.
Users register a participant in Agon, deposit AGON, open a lane to the gateway,
lock funds, and then make RPC calls with OWS-signed cumulative payment updates.
Instead of exact per-call payments, Agon settles later, making x402-style API
commerce affordable.

## What This Proves

For OWS:

- OWS can securely power machine-native wallet access
- OWS can sign protocol-native payment messages
- OWS can gate repeated machine payments without exposing raw keys in the app

For Agon:

- Agon can make API calls affordable through deferred settlement
- cumulative commitments are practical for repeated RPC/API use
- locked lanes work as a first production-safe spending model

## Current Working Run Order

Gateway:

```bash
cd <repo-root>/agon-gateway
npm run build
npm start
```

Client:

```bash
cd <repo-root>/agon-client
npm run build
npm run demo:ows
```

The current successful demo script automatically:

1. creates a fresh demo wallet
2. imports it into OWS
3. requests and confirms an x402-style AGON top-up
4. registers the participant
5. deposits AGON
6. proves `missing-channel`
7. creates the lane
8. proves `missing-locked-funds`
9. locks funds
10. sends two accepted paid RPC calls
11. shows the pending bundle settlement preview
12. settles the latest commitments on-chain
13. prints the final gateway state and empty pending preview

## Recording Shortcut

```powershell
cd <repo-root>
.\run-ows-demo.ps1
```

## After OWS

Do not try to prove multilateral here. That is the Colosseum story.

For OWS, the win condition is:

- affordable API calls
- OWS-secured signing
- Agon deferred settlement
- real gateway-enforced cumulative state
