# Shammary App Hub Status

## What's done

- 2026-08-20: Created a responsive bilingual launcher for the quotation, invoice, appointment, and customer applications.
- 2026-08-20: Applied the Graphite & Porcelain identity, canonical company favicon, and deployment-local logo assets.
- 2026-08-20: Renamed the Vercel project to `shammary-center` and assigned `https://shammary-center.vercel.app`.
- 2026-08-20: Created the public GitHub repository `https://github.com/rassemalwan/shammary-center` and pushed the `main` branch.
- 2026-08-20: Added the customer directory and rebalanced the launcher grid for four applications.

## What's next

- P2: Add future applications only when they are ready for staff use.
- P3: Consider authenticated access if the hub later exposes sensitive operational information.

## Security status

- The page is static and stores no credentials, customer records, or application state.
- External application links use `noopener noreferrer` and open in separate tabs.
- The linked applications retain their own authentication and security boundaries.
- Vercel SSO is disabled with owner approval, so production and preview deployments are public. This is acceptable while the project remains a static launcher with no private data or application state.

## Key decisions

- The local folder remains `shammary`; the Vercel project is named `shammary-center`.
- A dependency-free static page is sufficient for the current four-link map.
- Identity assets are copied locally for independent Vercel deployment; canonical masters remain in `shammary-brand-assets`.

## QA / health score

- Health: 100/100 for the current static scope.
- Local checks: semantic navigation, desktop and 390px mobile layouts, no horizontal overflow, keyboard focus states, reduced-motion handling, exact destination URLs, and zero console errors.
- Production checks: HTTP 200, four exact application links, favicon HTTP 200, correct page title, and zero console errors.
- Alias check: `shammary-center.vercel.app` returns HTTP 200 with the correct title and all four exact application links.
- Repository check: `.env.local` and `.vercel` remain ignored; no credentials or Vercel project metadata are tracked.
