# Klisten FailBox for Gemini CLI — v0.1.0

Simulate retries and failures using private synthetic balance sessions.

Create bounded simulations, apply steps with caller-selected idempotency keys, read/replay transitions and delete sessions. Simulated timeout-after-commit helps inspect retry behavior. No real transfers, production fault injection, proxying or wallet charges.

## What you need

Use your own dedicated Klisten API key with `failbox:session`, `failbox:step`. Sign in at https://klisten.co.kr/app. Store the key only in the sensitive extension setting `KLISTEN_FAILBOX_API_KEY`. Never put it in a prompt, tool arguments, source code or public repository. No shared operator credentials or OAuth client are included.

A non-expired session is required for reads; new session/step must be explicitly authorized. Delete only the reviewer-created session afterwards.

Endpoint: https://api.klisten.co.kr/mcp/failbox
Tools: `failbox_create_session`, `failbox_step`, `failbox_read`, `failbox_replay`, `failbox_delete_session`.

## Install and configure

Install the published source with `gemini extensions install https://github.com/daehoonkwon-hash/klisten-failbox-gemini-extension`. If using a downloaded copy, the local alternative is `gemini extensions install ./klisten-failbox`. Supply the key through the sensitive setting when prompted.

Gemini CLI 0.63 redacts API_KEY environment values in MCP headers by default. Merge `GEMINI_SETTINGS_FRAGMENT.json` into your chosen isolated Gemini profile settings.json, retaining existing settings. It explicitly allows ONLY `KLISTEN_FAILBOX_API_KEY`. It contains the variable name, not a secret. Never disable environment redaction globally or allow all variables. Without the narrow exception, the credential is not transmitted correctly.

Inspect `/mcp` after configuration. Tool discovery by itself does not establish authorization: first perform an appropriate safe call against your own eligible data. Do not invent record IDs or treat missing records as a successful demo.

## Examples and expected results

- Create a session with synthetic balances. Tools: failbox_create_session. Expected: Private bounded session with supplied synthetic balances.
- Simulate timeout after commit, then retry the exact same operation/key. Tools: failbox_step. Expected: Committed operation is not duplicated; observed timeout is simulated.
- Read and replay my existing simulation. Tools: failbox_read, failbox_replay. Expected: Authorized session state and replay verification.

## Scope and charges

The extension package itself is an integration, not a payment or service guarantee. Existing service prerequisites and pricing apply. Installing never replenishes credit or authorizes a paid scan. Do not perform a monetary transfer or paid operation for a smoke test. Data creation, license acceptance and deletion require the account holder's authorization. There is no model execution within this MCP adapter.

FailBox is a free limited simulation beta. No wallet charge, real money transfer, real ledger or infrastructure fault injection is implemented by these tools. Use synthetic account names only.

## Verified evidence and limits

On 2026-10-13, local installation/list/removal checks passed using an isolated Gemini CLI 0.63 profile. A separate scripted harness then used the actual Gemini native MCP transport, tool discovery and DiscoveredMCPTool invocation against the existing HTTPS service. One synthetic session was created; timeout-after-commit retry was deduplicated; persisted balances and one replayed transition matched; the session was deleted. No real money moved.

Local invalid-input validation rejected the empty request. The native wrong-scope call returned a tool error but did not expose a typed authentication code, so that native classification remains unverified. A separate request to the corresponding production REST route returned HTTP 403 / INSUFFICIENT_SCOPE. This separate REST check used a nonexistent session ID to verify the scope precheck, before lookup; it did not recreate the deleted session. This is direct REST evidence and must not be presented as an interactive Gemini conversation.

Interactive Gemini model conversation: NOT_VERIFIED. Public gallery listing: NOT_VERIFIED. These checks do not promise zero defects, uptime, safety certification or future performance. Trial keys and parent-created fixtures are temporary, not shared customer access.

## Removal and data handling

Run `gemini extensions uninstall klisten-failbox`. Uninstalling does not revoke an API key or delete hosted records. Revoke the dedicated key separately when it is no longer needed. Delete only your own test sessions using the supported delete tool. Never expose private records or customer identifiers in public demos.

Documentation: https://klisten.co.kr/failbox.html
Privacy: https://klisten.co.kr/privacy.html
Terms: https://klisten.co.kr/terms.html
Support: klisteninc@gmail.com
