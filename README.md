# Overview
Webhook Tester is a zero-backend web app for performing a final test against any live webhook URL. It lets you:

- Send HTTP requests with custom method, headers, and body
- Use predefined payload templates or your own JSON/text
- Automatically compute and attach HMAC signatures
- View detailed response status, headers, and body (when CORS allows)
- Retry with exponential backoff and fire multiple events with delays
- Fall back to no-cors mode to trigger webhooks even when responses can’t be read
- Generate and copy an equivalent cURL command
- Persist settings locally and export response logs

This is ideal for validating live endpoints like webhook.site, Pipedream, RequestBin, or your production webhook receiver.

# Setup
No installation required.

- Option 1: Open index.html directly in your browser.
- Option 2: Serve with any static server (recommended for some browsers):
  - Using Node: npx serve
  - Using Python: python3 -m http.server 8080

Note on CORS:
- Many webhook receivers do not allow cross-origin reads from browsers.
- When CORS headers are missing, the app cannot read the response. Enable the “no-cors fallback” option to still send the request; the destination will receive it, but the response will be “opaque”.
- If you need full response visibility for such endpoints, use a proxy you control or test endpoints that send permissive CORS headers (e.g., webhook.site configured appropriately, or your own server).

# Usage
1. Paste your live webhook URL
   - Example: https://webhook.site/your-unique-id

2. Choose Method and Content-Type
   - Default is POST with application/json.

3. Add Headers or Authorization (optional)
   - Use the Auth helper for Bearer or Basic.
   - Content-Type is set automatically from the selector.

4. Compose Payload
   - Pick a template or write your own.
   - For JSON, use “Format JSON” to pretty print.
   - For application/x-www-form-urlencoded, a JSON object is converted to key=value pairs; or write raw key=value&key2=value2.

5. Optional: Add HMAC Signature
   - Enable “Add HMAC signature”, choose algorithm and header name (default X-Signature).
   - Set an optional prefix (e.g., sha256=) and include a timestamp header.
   - Secret is never persisted.

6. Send
   - Click “Send request” to send once with optional retries on failure.
   - “Quick Ping (GET)” performs a one-off GET to your URL.
   - Use “Send xN” with count and delay for repeated events.

7. Inspect Responses
   - Responses show status, duration, sizes, headers, and body.
   - If CORS blocks reads, enable “Fallback to no-cors” to still fire the webhook (response will display as “opaque (no-cors)”).

8. Extras
   - Copy cURL: creates a ready-to-run cURL for the current request.
   - Export log: saves the current response list to JSON.
   - Import headers from JSON: paste an object like {"X-Key":"abc"}.
   - Persist settings: toggle to store the current configuration in localStorage.
   - Reset form: clears localStorage and resets the UI.

Tips for Live Webhooks:
- Use webhook.site or similar to quickly verify payload format and headers.
- If your production receiver requires a signature, set the same HMAC scheme and secret to match verification logic.
- Some endpoints might rate-limit; use the delay field for “Send xN”.

# If Round 2
Not applicable (this is Round 1).