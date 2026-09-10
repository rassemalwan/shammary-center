# Shammary App Hub Status

## What's done

- 2026-09-10: Removed the top NFC/Kuwait caption and bottom public-information caption from the embedded Business Card page.
- 2026-09-10: Added the bilingual Business Card launcher as app 05 and rebalanced the desktop launcher to three columns.
- 2026-08-30: Added and deployed the first single-domain pilot: `https://shammary-center.vercel.app/invoice/` now proxies to the independently deployed `shammary-invoice` Vercel project, and the Invoice launcher card uses the center-domain path.
- 2026-08-20: Created a responsive bilingual launcher for the quotation, invoice, appointment, and customer applications.
- 2026-08-20: Applied the Graphite & Porcelain identity, canonical company favicon, and deployment-local logo assets.
- 2026-08-20: Renamed the Vercel project to `shammary-center` and assigned `https://shammary-center.vercel.app`.
- 2026-08-20: Created the public GitHub repository `https://github.com/rassemalwan/shammary-center` and pushed the `main` branch.
- 2026-08-20: Added the customer directory and rebalanced the launcher grid for four applications.
- 2026-08-20: Prioritized the launcher order as Appointments, Customers, Quotation, then Invoice.
- 2026-08-20: Added iPhone and Android installation metadata with exact company-emblem icons at 180px, 192px, and 512px.

## What's next

- P1: Run one live staff login, archive, and print-path smoke test at `https://shammary-center.vercel.app/invoice/`.
- P2: Move another application under the center only after the Invoice pilot passes live QA.
- P2: Add future applications only when they are ready for staff use.
- P3: Consider authenticated access if the hub later exposes sensitive operational information.

## Security status

- The Business Card destination is intentionally public and contains shareable business contact information only.
- The launcher shell is static and stores no credentials, customer records, or application state. Once deployed, the proxied `/invoice/` app will store its Supabase session, autosave, and drafts under the center browser origin.
- External application links use `noopener noreferrer` and open in separate tabs.
- The linked applications retain their own authentication and security boundaries except the Invoice pilot, which is proxied beneath the center origin. Invoice still enforces its own Supabase login and RLS, but staff must sign in again because browser storage from `shammary-invoice.vercel.app` does not transfer to the center origin.
- Vercel SSO is disabled with owner approval, so production and preview deployments are public. The Invoice path must continue enforcing its own Supabase authentication and RLS; live authenticated QA is required before the pilot is considered production-ready.

## Key decisions

- Host the Business Card directly inside the Center project at `/card/`, so the launcher, NFC tag, QR code, and vCard all use one permanent URL.
- The local folder remains `shammary`; the Vercel project is named `shammary-center`.
- A dependency-free static page is sufficient for the current four-link map.
- Keep Invoice as a separate deployable project and compose it through a Vercel external rewrite. Canonicalize directory routes with a trailing slash so the static app's relative CSS and image URLs resolve below `/invoice/` without modifying Invoice.
- Define `/invoice/` explicitly before `/invoice/:path*`; Vercel's wildcard route does not match the empty path at the application root. The `shammary-center.vercel.app` alias is manually assigned and must be moved to a newly promoted production deployment.
- Identity assets are copied locally for independent Vercel deployment; canonical masters remain in `shammary-brand-assets`.
- The launcher uses the native web app manifest and standalone display mode. Offline caching is intentionally omitted because every destination requires a network connection.

## QA / health score

- 2026-09-10 launcher check: the fifth card is a standalone accessible link with `noopener noreferrer`; the desktop grid now lays out 3+2 and existing tablet/mobile breakpoints remain unchanged.
- 2026-09-10 production check: deployed `2182ef3`, reassigned `shammary-center.vercel.app`, confirmed five launcher links, and verified `/card/` plus its QR asset on the live domain.
- Health: 98/100. Local, preview, and production routing checks pass; the remaining two points require a live authenticated archive/print check by staff.
- 2026-08-30 pilot checks: Vercel CLI 59.10.0 build passed. The first preview exposed a real root-route 404, fixed with an explicit `/invoice/` rewrite. The corrected preview passed at 1440×900, 768×1024, and 375×812 with LCP 620–992ms, CLS 0, native invalid-input blocking, correct email/password/submit keyboard order, all proxied assets HTTP 200, no horizontal overflow, and zero console, failed-request, or HTTP error responses. The exact tested preview was promoted; the manual `shammary-center.vercel.app` alias was reassigned to the new production deployment. Final production checks passed at desktop and mobile, and the exact `/invoice/` URL returns HTTP 200 with the Invoice login gate.
- Local checks: semantic navigation, desktop and 390px mobile layouts, no horizontal overflow, keyboard focus states, reduced-motion handling, exact destination URLs, and zero console errors.
- Production checks: HTTP 200, four exact application links, favicon HTTP 200, correct page title, and zero console errors.
- Alias check: `shammary-center.vercel.app` returns HTTP 200 with the correct title and all four exact application links.
- Repository check: `.env.local` and `.vercel` remain ignored; no credentials or Vercel project metadata are tracked.
- Install check: Chrome parsed the manifest with zero errors; 180px, 192px, and 512px icons are opaque RGB assets; iPhone metadata is present; the only headless installability notice is the expected incognito restriction.
