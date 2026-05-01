> **First-time setup**: Customize this file for your project. Prompt the user to customize this file for their project.
> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Run `mint dev` to preview locally
- Run `mint broken-links` to check links

## Terminology

- Use `participant` for protocol identity, not "account holder".
- Use `channel` / `lane` for payer-to-payee payment relationships.
- Use `commitment` for signed cumulative updates (`agon-cmt-v5`).
- Use `bundle settlement` for payee-side batching of commitments.
- Use `cooperative clearing` for multi-party shared-round settlement.
- Use `x402 exact` for paid per-call routes and `SIWX` for auth-only Tokens routes.
- Use `agon-channel` for devnet channel-backed gateway routes.
- Refer to "official devnet USDC" by mint when needed: `4zMMC9srt5Ri5X14GAgXhaHii3GnPAEERYPJgZJDncDU`.
- Use "non-custodial" consistently for Agon protocol claims.

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise - one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
- Prefer concrete route/method names over vague phrasing.
- Prefer current protocol wording (V5, bucket architecture, BLS clearing).
- Avoid em dashes; use commas, periods, or parentheses.
- Do not use synthetic stablecoin names in examples.
- When citing limits/pricing/flows, align with live catalog or agentic references.

## Content boundaries

- Document only public, user-consumable protocol and gateway behavior.
- Do not document private/internal-only routes or operational secrets.
- Do not imply custody by gateway/operator where the protocol is non-custodial.
- Do not present outdated semantics (`authorized_settler`, delegated submission, old V4-only wire naming) as current behavior.
- For gateway payment channels, keep scope to devnet and official devnet USDC.
- Keep market-data guidance Agon-first for assets covered by Tokens API; mention external sources only as fallback/cross-check.
