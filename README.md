# AegisRisk Pro — Web Plugin verification build

Version: 0.3.1  
UI update: compact grouped sections with collapsible advanced settings, account tools, logs and readiness checklist.  
Language: Indonesian UI  
Purpose: premium responsive risk calculator + cTrader Plugin SDK handshake/account/symbol verification.

## Important safety scope
This is a **verification build**, not a production trading release. BUY/SELL controls are deliberately disabled and no live orders are sent. The calculator requires user-supplied pip size and pip value; volume assumes 100,000 units per lot for display only. It does not yet verify broker volume min/max/step or calculate instrument-specific risk from live symbol metadata. Do not use the estimate as a production order size.

The page attempts to load the official cTrader plugin SDK ESM packages and handshake when opened inside a cTrader web-based plugin. Opening it directly in a normal browser should still let you test the calculator, but there is no connected cTrader host.

## Quick local preview
1. Extract the ZIP.
2. Open `index.html` in a browser.
3. Change risk/equity/entry/SL/pip inputs and verify the outputs.
4. The live BUY/SELL controls should remain disabled.

## Deploy as a web-based cTrader plugin
1. Host `index.html` on an HTTPS static host (GitHub Pages is suitable for this single-file preview).
2. Make sure the published URL serves the page directly and does not redirect to a different domain.
3. In cTrader Web, Windows or Mac, open the Plugins builder and create a web-based plugin using the HTTPS URL. Choose the placements you want.
4. Enable the plugin, then test the SDK status. Click **Refresh account info** and **Load symbols** to check whether the plugin-host connection and SDK requests work.
5. Test first on a demo account. A plain browser preview cannot verify host SDK actions.

## Suggested GitHub Pages deployment
1. Create a repository, for example `aegisrisk-pro-web`.
2. Upload `index.html` to the repository root.
3. Open repository **Settings → Pages**.
4. Under build/deployment, choose deploy from branch, select the main branch and `/ (root)`, then save.
5. Wait for GitHub Pages to publish the site. Open the generated HTTPS URL and verify the calculator.
6. Paste that URL into cTrader's web plugin builder.

## Verification checklist
- [ ] Page loads at HTTPS URL.
- [ ] Responsive layout works on desktop and mobile width.
- [ ] Changing input values recalculates lot estimate.
- [ ] Invalid/zero values do not crash the page.
- [ ] In cTrader, SDK modules load and host handshake reaches Connected.
- [ ] Account info request returns data.
- [ ] Symbol list request returns data.
- [ ] No real order is sent by this build.

## Before production trading
Production release still requires a dedicated integration pass:
- Parse cTrader SDK account and symbol response schemas explicitly (not recursive field guessing).
- Derive pip size, pip value, volume unit conversion, min/max/step from live symbol metadata and account currency.
- Use broker-accurate risk sizing and reject any result outside broker constraints.
- Build a confirmed market-order request with SL protection attached in the initial request where supported.
- Confirm execution result and confirm the resulting position actually has SL; handle rejected orders and protection failures.
- Add an explicit confirmation modal, a hard demo/live environment indicator and tested fail-closed behaviour.
- Run browser/device tests in cTrader Desktop, Web and Mobile; test symbols with different contract specifications.
- Do not expose tokens, credentials, API secrets or account identifiers in client-side source.


## Version 0.3.0 — Professional dashboard
- Refined responsive dashboard layout and navigation.
- Added overview, feature summary, and clear LIVE OFF safety banner.
- Preserved the existing risk calculator and disabled order buttons.
- Added runtime origin diagnostics to help verify the cTrader Builder URL.
- Host handshake still requires a response from the cTrader host; a timeout is not treated as a successful connection.


## Version 0.3.1 — SDK handshake verification
- Corrected the connection state: `Connected` is now shown only after the final `confirmEvent` returns a response.
- Increased the registration diagnostic timeout to 20 seconds and clarified host-registration failure logs.
- Added runtime context details (frame status, visibility and user agent) to help isolate mobile WebView issues.
- Kept BUY/SELL disabled; no order execution method is called.
- This build improves handshake correctness and diagnostics; it cannot force the cTrader host bridge to respond if the host/plugin environment does not provide registration.
