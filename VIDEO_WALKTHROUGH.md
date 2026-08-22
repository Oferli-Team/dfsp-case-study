# DFSP Visual Assets and Walkthrough Plan

All visuals must use an isolated demo environment and synthetic files, names, accounts, addresses, CIDs, capability IDs, and metrics. Never show real link tokens or decryption-key URL fragments.

## Recorded Local Demo Assets

The recommended `visuals/dark-4k/` directory contains dark-mode desktop and Telegram Mini App captures of the file dashboard, encrypted upload form, recipient and public-link policies, grant management, confirmed transaction state, local integrity verification, and Mini App file/verification views. It also contains a 29-second silent walkthrough mastered at 3840×2160 and 60 FPS, a 1920×1080/60 FPS delivery copy, and a 1280×720 animated preview.

Desktop and mobile screens are presented inside compact device frames on a dark visual system. Cursor travel uses a short eased motion lasting approximately half a second; click indicators were checked against the visible centers of all nine targets in the rendered 4K frames.

The captures use generated local data only. Landing, health, key-management, incomplete Mini App grant-management, and live monitoring screens were deliberately omitted from the recommended public set.

## Visual Assets Checklist

1. **Landing page and authentication options** — Demonstrates that the product presents a polished entry point and supports local signing, MetaMask, WalletConnect, and TON paths without exposing a real wallet.
2. **Encrypted upload flow** — Show file selection, encryption progress, and successful registration using a harmless synthetic document to demonstrate browser-side processing before upload.
3. **Authenticated file dashboard** — Show several synthetic files with identifiers and storage references blurred or replaced to demonstrate list, search/navigation, status, and responsive UI quality.
4. **File details and version history** — Show metadata, integrity fields, versions, and grant status using test data to demonstrate traceability across application and contract records.
5. **Recipient sharing workflow** — Show dummy recipient addresses, expiry, maximum downloads, per-recipient key wrapping, and queued/confirmed grant states to demonstrate controlled access.
6. **Public-link creation and download** — Show expiry, optional download limit, optional proof-of-work, revocation, and local decryption, while keeping the token and `#k=` fragment completely off screen.
7. **Local file verification** — Show a successful checksum comparison and a controlled mismatch example to demonstrate integrity verification without exposing a client file.
8. **Grant management** — Show received and granted views, status filters, download usage, and revocation in the main web app; do not use the placeholder Mini App grants screen.
9. **Key backup and restore** — Show the existence of encrypted backup and restore controls without displaying private-key material, backup contents, passwords, or a real backup filename.
10. **Telegram bot flow** — Use a dedicated test account to show link/unlink, file listing, verification, notification preferences, quiet hours, address switching, and a one-time-link notification with all identifiers redacted.
11. **Telegram Mini App** — Show the mobile-oriented home, files, verification, rename, and public-link experiences; avoid presenting grant management as complete.
12. **Observability dashboard** — Use synthetic or reset metrics to show API latency/error panels, relayer queues, proof-of-work/quotas, and alerts without revealing live usage or infrastructure labels.
13. **High-level architecture graphic** — Convert the Mermaid diagram into a branded graphic showing client, API, PostgreSQL, Redis, IPFS, workers, EVM contracts, Telegram, and monitoring.
14. **Short end-to-end GIF** — Capture encrypt → upload → share → recipient decrypt/verify in a demo environment to give a potential client one clear product narrative.

## Code Walkthrough Video Outline

Target length: approximately 5–6 minutes.

### 0:00–0:35 — Project introduction

- Introduce DFSP as previous collaborative work by a person who is now an Oferli technical team member.
- Describe it as a web/API/Telegram file-sharing platform with browser-side encryption, IPFS storage, and EVM-backed metadata/access grants.
- State that the client is confidential and that the demo uses synthetic data.

Safe areas to show:

- The public portfolio README.
- A sanitized product screenshot or local demo landing page.

### 0:35–1:15 — Architecture

- Show the high-level architecture diagram.
- Explain the boundary between local cryptography, the FastAPI coordination layer, encrypted-object storage, database/cache, asynchronous workers, contracts, Telegram, and observability.
- Note that the scheduled Merkle worker currently computes and stores roots; do not describe on-chain submission as complete.

Safe areas to show:

- The sanitized Mermaid or branded architecture diagram.
- Top-level directory names only: `frontend`, `backend`, `contracts`, `bot`, `deploy`, `docker`, and `docs`.

### 1:15–2:15 — Browser cryptography and file lifecycle

- Demonstrate the synthetic upload flow.
- Explain that the client computes identifiers/checksums, encrypts chunks with AES-GCM in a worker, and uploads ciphertext.
- Explain at a high level that file keys are wrapped per recipient with RSA-OAEP and decrypted locally.
- Mention password-encrypted key backup and restore.

Safe and useful source areas, only if source-screen permission is confirmed:

- `frontend/src/components/pages/UploadPage.tsx` — component flow and state names, not long source excerpts.
- `frontend/src/lib/cryptoClient.ts` — exported function signatures for chunked encrypt/decrypt.
- `frontend/src/workers/crypto.worker.ts` — worker message boundary and algorithm names.
- `frontend/src/lib/keychain.ts` — backup/restore API names, with all stored material and implementation details kept off screen.

### 2:15–3:20 — Grants, public links, and relaying

- Show the recipient-sharing screen with dummy addresses and policy values.
- Explain deterministic capability IDs, expiry, maximum downloads, usage, and revocation.
- Explain EIP-712/ERC-2771 at a high level: the user signs, and a queued relayer submits.
- Show public-link policy and local decryption, but never the complete URL.

Safe and useful source areas, only with permission:

- `backend/app/routers/files.py` — route names and share-flow structure, not logs or payload values.
- `backend/app/routers/grants.py` — list/revoke route names.
- `backend/app/routers/meta_tx.py` — enqueue boundary.
- `backend/app/relayer.py` — high-level queue, idempotency, lock, retry, and receipt-reconciliation structure.
- `contracts/src/AccessControlDFSP.sol` — contract function/event signatures only.
- `contracts/src/FileRegistry.sol` — registry/versioning function signatures only.

### 3:20–4:05 — Telegram and asynchronous notifications

- Show the test bot menu, file list, verification result, notification preferences, and a redacted one-time-link message.
- Explain Telegram WebApp session verification and linked-wallet selection.
- Explain stream consumption, deduplication, quiet hours, coalescing, daily limits, and retries.

Safe areas to show:

- `bot/app/main.py` — command registration names only; do not show webhook/logging sections.
- `bot/app/services/notifications/consumer.py` — class/function outline only.
- `backend/app/services/notification_publisher.py` — event type names and publisher boundary.

### 4:05–4:50 — Testing and engineering quality

- Show test directory structure across backend, bot, frontend, and contracts.
- Call out verified categories: negative API paths, auth signatures, share/revoke/download flows, Telegram linking, notification deduplication, crypto round trips, deterministic capability IDs, forwarded calls, and contract race behavior.
- Say that tests exist in the repository; do not claim a current pass rate unless a clean run is recorded immediately before filming.

Safe areas to show:

- Test filenames and test names.
- The CI workflow’s job/step names.
- No fixtures containing identifiers, tokens, or environment values.

### 4:50–5:30 — Deployment and observability

- Show only the sanitized service-level deployment diagram.
- Explain containerized API, workers, database, Redis, IPFS, chain, reverse proxy, and monitoring.
- Show a synthetic Grafana dashboard or dashboard titles.
- Mention health/liveness/readiness checks, request metrics, relayer metrics, quotas, and alerts.

Safe areas to show:

- Dockerfile base/runtime stages.
- Compose service names only.
- Grafana panel titles and sanitized synthetic panels.
- GitHub Actions job names.

### 5:30–6:00 — Final result

- Recap the user outcome: encrypt, upload, share, revoke, download/decrypt, verify, recover keys, and use selected Telegram workflows.
- State that production scale and business results are not disclosed.
- End with the Oferli portfolio link and contact call to action.

## Do Not Show on Screen

- Any `.env`, `.env.*`, secret, credential, private-key, key-backup, or local configuration file.
- `backend/app/security.py` logging sections, because they include token/secret prefixes and user identifiers.
- `backend/app/routers/storage.py` and `backend/app/routers/files.py` logging sections, because they can emit database connection information and identifiers.
- `backend/app/routers/public_links.py` logging sections, because they can emit link tokens, access-key names, hashes, and CIDs.
- `bot/app/main.py` webhook handler/log output, because it can expose webhook secrets or raw Telegram update data.
- Live terminal output, application logs, queue contents, database rows, Redis keys, or monitoring data.
- Real hostnames, IP addresses, SSH targets, registry names, service-account identifiers, contract addresses, explorer links, or deployment paths.
- Complete public links, one-time links, link tokens, URL fragments, JWTs, wallet signatures, Telegram `initData`, chat IDs, addresses, CIDs, capability IDs, or transaction hashes.
- Contact emails in application pages or proxy configuration.
- The client name, internal organization names, issue links, private pull requests, contributor emails, or raw Git history.
- Client/user files, names, metadata, screenshots, analytics, or database contents.
- The DFSP logo, favicon, wallet-brand assets, Figma-derived design, or any Unsplash-derived media until publication rights and attribution are confirmed.
- Large proprietary source excerpts or any cryptographic/private-key material.
