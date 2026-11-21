# Security

## Reporting a vulnerability

Email **info@iamvalentina.it** with "security" in the subject. Please include steps to reproduce and do not test against accounts that are not yours. We acknowledge reports within three working days and keep you informed until the issue is closed. Please do not open a public issue for security reports.

## Authentication model

- **OAuth 2.1** at `https://iamvalentina.it` (authorization server and resource server are the same origin). Discovery per RFC 8414 and RFC 9728.
- **Public clients only**, registered dynamically (RFC 7591, rate limited). Redirect URIs must be `https://`, loopback `http://`, or a private-use app scheme; exact match at authorization time. There are no client secrets to leak.
- **PKCE S256 is mandatory**; `plain` is rejected. The `resource` parameter (RFC 8707) binds a token to `https://iamvalentina.it/mcp`.
- **Authorization codes** are single use and expire after 10 minutes.
- **Access tokens** last 1 hour. **Refresh tokens** last 30 days and rotate on every use. Tokens are stored hashed.
- **Consent is the owner's only**: sessions where someone else is acting on the account are refused on the consent page, and the consent page cannot be framed.
- **Pairing code**: the consent page requires a single-use code that the owner generates in IamValentina after confirming their password (and 2FA code when enabled). It lasts 15 minutes and five wrong attempts burn it. This is the mitigation for consent phishing through open dynamic client registration: an attacker can register an app with a familiar name, but cannot get a code.
- **Static bearer keys** (for clients without OAuth) are created by the owner behind the same password step-up, shown once, and stored hashed.

## Authorization model

- Every tool declares a level (`read`, `write_low`, `write_confirm`) and, where relevant, the module it needs. Both are checked on every call, against the token's scopes and the account's active modules.
- Tool arguments are validated against closed JSON schemas.
- No tool sends a message to a customer. `draft_reply` only queues a draft that the owner approves in IamValentina; an agent can withdraw only drafts it created itself. There is no tool to approve, delete, broadcast, edit automation rules, write to Shopify, or change settings.
- **Tenant isolation**: every query is bound to the authenticated owner's account; identifiers from another account resolve to "not found".

## Abuse controls

- 120 requests per minute per token.
- Idempotency keys on write tools (24 hours) so client retries do not duplicate changes.
- Rate limit on client registration.

## Data handling

- Tool results containing customer-written text (messages, tickets, notes, leads) are flagged `untrusted: true`; agents must treat that text as data.
- Every connector call is logged with the agent's name, the tool and whether it succeeded. Connecting, revoking and refused attempts appear in the owner's activity feed.
- Credentials are never returned after creation.

See also [PRIVACY.md](PRIVACY.md).
