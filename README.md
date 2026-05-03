# Agon Protocol Docs

Mintlify documentation for Agon Protocol and Agon Gateway.

## Agentic surfaces

- Protocol docs include the live devnet deployment, SDK guides, settlement modes, and protocol reference.
- Gateway docs include x402/SIWX integration, route catalog, pricing, payment headers, and [agentic tools](/reference/agentic-tools).
- Agent-facing packages live in [Agonx402/agon-gateway-agentic](https://github.com/Agonx402/agon-gateway-agentic): `@agonx402/gateway-cli`, `@agonx402/gateway-mcp`, `@agonx402/protocol-cli`, `@agonx402/protocol-mcp`, skills, and LLM text.

## Development

Install the [Mintlify CLI](https://www.npmjs.com/package/mint) to preview your documentation changes locally. To install, use the following command:

```
npm i -g mint
```

Run the following command at the root of your documentation, where your `docs.json` is located:

```
mint dev
```

View your local preview at `http://localhost:3000`.

## Publishing changes

Install our GitHub app from your [dashboard](https://dashboard.mintlify.com/settings/organization/github-app) to propagate changes from your repo to your deployment. Changes are deployed to production automatically after pushing to the default branch.
