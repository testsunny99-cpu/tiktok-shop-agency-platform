# AI Project Handoff

- Project: `tiktokshop-agency-multi-account`
- Current stage: `discovery`
- Session date: 2026-11-11
- Status: completed initialization, not implementation

## Verified progress

- Inspected the workspace; it contained only `tiktokshop_api.txt` and no existing project skeleton or Git history.
- Defined the project as an internal TSP agency platform for batch management of many Creator and Seller/Shop accounts.
- Documented account isolation, OAuth/Token lifecycle, upload/precheck/publish workflow, batch queue, idempotency, rate limiting, status tracking and audit requirements.
- Preserved official documentation URLs without placing credentials in tracked docs.
- Confirmed the copied callback `https://tts-callback.shop-oauth.workers.dev/callback` is an external Cloudflare `workers.dev` address, not an owned project endpoint; documentation now requires an owned callback and server-side `state` validation.

## Files changed

- Governance: `AGENTS.md`, `project-governance.yml`, `.env.example`, `CHANGELOG.md`
- Product: `README.md`, `docs/project/intake.md`, `docs/project/overview.md`, `docs/project/architecture.md`, `docs/project/decisions.md`, `docs/project/milestones.md`, `docs/project/api-reference.md`
- Operations: `docs/ops/runbook.md`, `docs/ops/deploy.md`, `docs/ops/troubleshooting.md`

## Evidence and validation

- Official URLs and API pages were opened/read during this session.
- No service code or runtime tests exist yet.
- `TBD`: runtime, database, queue, storage, deployment and test commands.

## Open risks / decisions

- Confirm target markets beyond US and whether Creator + Seller/Shop are both required in MVP.
- Validate app category and scopes for the actual registration region.
- Validate test-account OAuth, token refresh, video upload, precheck, publish and status behavior.
- Deploy an owned HTTPS OAuth callback, configure the exact URL in Partner Center, publish the change, and rotate any exposed App Secret before testing.
- Decide storage, queue, database, secret manager, RBAC and multi-tenant boundary.

## Exact next action

Run an API-only discovery spike with two test creator accounts: complete OAuth separately, obtain tokens, query products, upload one video per account, run precheck, publish one task, and record sanitized responses and limits. Do not use production accounts until separately approved.

## Open-source landscape survey (2026-11-11)

- Added `docs/project/open-source-landscape.md`.
- No mature open-source project was found that covers the full target: agency multi-account management, separate Creator/Seller authorization, shoppable-video upload/precheck/post, batch orchestration and audit.
- Best reuse candidates are SDK/reference layers (`EcomPHP/tiktokshop-php`, `SKIT.FlurlHttpClient.ByteDance`), multi-shop/task safety patterns (`Buer2333/tiktok-shop-mcp`, `codustry/marketeer`) and official-doc mirroring (`dmiller1006/tiktok-shop-docs-skill`).
- Browser automation (`yancyuu/tk-creator-binder`) is only a low-level operations reference and must not replace the official API path.

## Open-source report update and publication (2026-11-11)

- Expanded `docs/project/open-source-landscape.md` into a structured local report with table of contents, evaluation criteria, official capability boundaries, reuse/self-build boundaries, risk checklist and references.
- Local checkpoint: `2478026 docs: refine open source landscape report`.
- Created and published the public GitHub repository: https://github.com/testsunny99-cpu/tiktok-shop-agency-platform

## Open-source licensing (2026-11-11)

- Added `LICENSE` with the MIT License.
- Linked the license and open-source landscape report from `README.md`.
- Confirmed the GitHub repository is public and `.env.local` is not tracked.
