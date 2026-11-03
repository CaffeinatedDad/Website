# Caffeinated Dad Website — Project Memory

```text
Memory-Contract-Version: 1
Memory-Revision: 4
Base-Revision: 3
Project: Caffeinated Dad Website
Repository: https://github.com/CaffeinatedDad/Website
Branch: main
```

Tier-0 core under [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md).
Git facts (exact commit, publication, remote equality) are derived at hydration, not recorded here.
This file grants no execution authority.

## Identity and mission

This repository is the source of caffeinateddad.dev: the Caffeinated Dad home page and the CryptoBeam product, privacy and support pages. The work here keeps that site online and correct on its Cloudflare Worker.

## Current state

- The Cloudflare Worker is the only host. It serves production through a route, and `www` redirects to the apex with 301 (D-CDW-011).
- Every push to `main` builds and deploys the Worker, so it changes the live site (D-CDW-010).
- GitHub Pages is off and the staging hostname is gone, both since 2026-11-02. The rollback for a bad deploy is the previous Worker version (D-CDW-011).
- Since 2026-11-03 the site carries the CryptoBeam 1.3 look: the home and CryptoBeam pages restyled, ten 1.3 screenshots, social preview tags, and no version number on the product page. The Worker serves 24 files.

## Architecture summary

- **Site:** static files at the repository root: `index.html`, `assets/` and `cryptobeam/`. No framework, package manager or build step. 24 served files: `index.html`, `assets/site.css`, `assets/cryptobeam-icon.png`, 17 files under `assets/launch/`, and the CryptoBeam page, stylesheet, support page and privacy page.
- **Cloudflare Worker `caffeinateddad-website`:** static assets from the repository root, no Worker script, `workers.dev` and preview URLs off ([wrangler.jsonc](wrangler.jsonc)). [.assetsignore](.assetsignore) lists every file it must not serve. Workers Builds deploys it from `main`.
- **Zone (Cloudflare):** the route `caffeinateddad.dev/*`; one proxied apex A record on the placeholder `192.0.2.0` and a proxied `www` CNAME to the apex, so no request reaches an origin; a redirect rule for `www`. Always Use HTTPS, minimum TLS 1.2, Email Obfuscation off, HSTS off, SSL Full (automatic SSL/TLS may change the mode; the site doesn't depend on it). The zone holds the iCloud Mail records.
- **Decisions:** [docs/DECISIONS.md](docs/DECISIONS.md).

## Rules and constraints

Universal rules live only in the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md). Project-specific rules:

- Every GitHub or Cloudflare change (DNS, Worker, route, rule, deployment, build settings, zone settings, tokens, hosting) needs its own owner-approved gate. Connected tools never imply approval (D-CDW-002).
- A push to `main` is a production deploy. Every push gate checks the Workers build and deployment it starts and the live files afterwards (D-CDW-010).
- The repository root is public: a new file is served unless `.assetsignore` lists it. Any gate that adds a file checks it (D-CDW-012).
- Never write the Cloudflare login email, the `workers.dev` subdomain, account, zone or token ids, or secrets here or in prompts and reports. Count other projects' resources; don't name them.
- Canonical Project Memory outranks conversation history and vendor memory.
- Memory never grants execution authority.

## Roles and current deployment preference

Master, Artifacts and Executor are portable roles. Current owner deployment preference (not
architecture or authority):

- **Master:** Caffeinated Dad Website — Claude Projects (beta). Plans, verifies read-only and recommends; never executes.
- **Artifacts:** Artifact Work - Global, the existing shared packaging worker, limited to each task the owner approves.
- **Executor:** CDW Executor, local Claude Code on the owner's Mac (LocalHostName `Caffeinated-MacBook`), in this repository's local checkout.

## Active objective

Keep the site online and correct on the Worker. The move to Cloudflare is complete; what remains is optional clean-up and content updates, one owner-approved gate at a time.

## Last closed gate

R24, publish the CryptoBeam 1.3 website refresh, closed 2026-11-03. It pushed R23's commit, opened and squash-merged PR #8 as one commit on `main`, and checked the Worker build and deployment it started; all 24 served files matched the reviewed tree byte for byte, the five retired screenshot paths and every excluded path returned 404, and no rollback ran. Evidence: report `2026-11-03_08-57-20_Website_R24-Website-Refresh-Report_v01.txt`, SHA-256 `d5f7f3e41b3f34c3ca4973cd0fd9e7b3d54816e66de120a22eae19d8cb32702c`; R23's report `2026-11-03_08-14-29_Website_R23-Website-Refresh-Report_v01.txt`, SHA-256 `eb7bcc0f6bd51d34f4cb5010149370ac11bc310c21ed690b4287e73acb9ffda0`.

## Next gate

None is pending. The items under open work are candidates; each is a recommendation, not authority, and needs Master review and owner approval before execution.

## Open work and deferred items

- Clean-up, each optional and its own gate: merged branches on GitHub (now including `website-refresh-v1.3`), the owner's local checkout, and the stale Claude Code memory notes for it.
- Content updates when CryptoBeam changes: screenshots, copy and the privacy policy, each its own gate under D-CDW-004.
- Not planned: a Workers Custom Domain instead of the route. It would change DNS again for no visible gain.

## Open questions

None blocks the next gate. Unresolved:

- How the `cryptobeam/` pages relate to the separate `cryptobeam-privacy` repository, which Agent Governance records as ownership-unresolved. Don't guess ownership, migrate or change either.
- The purpose of the `codex/website-launch-phase-1-2` branch and its local Codex worktree.

## Cautions and protected scopes

- `main` is canonical and is production's source: a push to `main` deploys the live site. Commit and push are separate gates.
- The route, the `www` redirect rule, the two site records and the zone settings carry production, and there is no fallback host: deleting the route or setting the records DNS-only takes the site down. Change them only in their own gate.
- Turning GitHub Pages back on, or adding a staging hostname, needs its own decision and gate.
- `cryptobeam/` pages and stylesheet: protected; no change without its own approval (D-CDW-004). The refresh of 2026-11-03 was approved as-is by the owner and kept the support and privacy page text byte-identical.
- The iCloud Mail DNS records (MX, SPF, verification): never touched.
- Other projects' repositories, zones and Cloudflare resources: out of scope.
- **Other repositories:** this memory grants no authority over another repository.

## Recent meaningful changes

- 2026-11-03: R23 and R24 refreshed the home and CryptoBeam pages for CryptoBeam 1.3 (PR #8): new look, ten 1.3 screenshots, Open Graph and Twitter tags with a social preview image; served files went from 18 to 24. R22 published Memory Revision 3 (PR #7).
- 2026-11-02: R18 turned GitHub Pages off and removed the staging hostname; the site's records no longer point at GitHub (D-CDW-011).
- 2026-10-29: R20 and R21 updated `/cryptobeam/privacy/` to CryptoBeam's October 28, 2026 policy (PR #6).
- 2026-10-26: R17 decided to retire GitHub Pages; R15 and R16 published Memory Revision 2 (PR #5), the first automatic deploy.
- 2026-10-26: R14 moved production to the Worker.
- 2026-10-25: R13 set the zone's HTTPS and TLS settings and made Workers Builds deploy every push (D-CDW-010).
- 2026-10-18 to 2026-10-25: R5 to R8 adopted this Project Memory (PR #2) and passed a fresh-agent reconstruction; R9 to R12 updated `/cryptobeam/privacy/` (PRs #3 and #4).

## Load map

- **Tier 0 (always):** the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md) and this file.
- **Tier 1 (as the task requires):**
  - [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md): memory format.
  - [docs/DECISIONS.md](docs/DECISIONS.md): decisions.
  - [wrangler.jsonc](wrangler.jsonc), [.assetsignore](.assetsignore): hosting and publication rules.
- **Tier 2 (only to re-prove disputed facts):** the reports named with SHA-256 here and in docs/DECISIONS.md, kept in the owner's Agentic File Cabinet. Reports from before 2026-10-31 were renamed when filed there, so identify every report by its SHA-256. Never required for normal hydration.

To resume in any role on any provider: load Tier 0, verify repository, branch and HEAD read-only,
report a hydration receipt per the contract, then continue from the next gate.
