# Privacy

This note covers the MCP connector specifically. The general IamValentina policies apply in full:

- Privacy policy: https://iamvalentina.it/privacy
- Terms of service: https://iamvalentina.it/termini

## Whose data

A connection acts **only on the account of the shop owner who authorized it**, within the permission level they chose. The connector never aggregates data across accounts and never exposes one shop's data to another.

## What an agent can access

Depending on the granted level (see [README.md](README.md#permission-levels)):

- **Read**: the owner's customers (names, contact details, spend, segments, tags, notes, orders), Shopify orders as synced into IamValentina, leads, support tickets with their conversations, inventory, sales and margin figures, and messages waiting for approval.
- **Write (small, reversible)**: ticket status, priority and AI on/off; customer and lead tags; lead details, custom fields and stage; pausing a lead's follow-ups; inventory movements and stock counts.
- **Write (drafts)**: draft replies to customers, which are sent only if the owner approves them in IamValentina.

Personal data of the owner's customers therefore flows to the owner's chosen AI client. The owner remains the data controller for their customers' data; the AI client's provider is the owner's choice and their processor. IamValentina does not send account data to any AI provider on its own as part of this connector.

## What we store about the connection

- The connection itself: agent name, permission scopes, creation, expiry, last-use and revocation timestamps, and (for OAuth grants) the registered client id.
- Hashes of access tokens, refresh tokens, keys and pairing codes (never the plaintext).
- A log of every tool call: tool name, agent name, outcome and timestamp.
- Idempotency records for write calls, kept 24 hours.

## Deletion

Revoking a connection stops all access on the next call. Deleting an IamValentina account deletes its connections, codes and idempotency records.

## Contact

info@iamvalentina.it
