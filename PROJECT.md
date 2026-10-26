# Caffeinated Dad Website — Project Memory

```text
Memory-Contract-Version: 1
Memory-Revision: 2
Base-Revision: 1
Project: Caffeinated Dad Website
Repository: https://github.com/CaffeinatedDad/Website
Branch: main
```

Tier-0 core under [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md).
Git facts (exact commit, publication, remote equality) are derived at hydration, not recorded here.
This file grants no execution authority.

## Identity and mission

This repository is the source of caffeinateddad.dev: the Caffeinated Dad home page and the CryptoBeam product, privacy and support pages. The work here keeps that site online and correct now that its hosting has moved from GitHub Pages to Cloudflare.

## Current state

- Since 2026-10-26 the Cloudflare Worker serves production through a route on the proxied site records, and `www` redirects to the apex with 301 (D-CDW-009).
- Every push to `main` builds and deploys the Worker, so it changes the live site (D-CDW-010).
- GitHub Pages is still enabled and still builds `main`, but no visitor reaches it. It is the rollback path until a separate decision retires it.
- The staging hostname still serves the same Worker.

## Architecture summary

- **Site:** static files at the repository root: `index.html`, `assets/` and `cryptobeam/`. No framework, package manager or build step.
- **Cloudflare Worker `caffeinateddad-website` (production):** static assets from the repository root, no Worker script, `workers.dev` and preview URLs off ([wrangler.jsonc](wrangler.jsonc)). [.assetsignore](.assetsignore) lists every file it must not serve. Workers Builds deploys it from `main`.
- **Zone (Cloudflare):** the four apex A records and the `www` CNAME are proxied but keep GitHub Pages' targets, so rollback is deleting the route (works while SSL stays Full) or switching them back to DNS-only (works only until 2026-12-01). Always Use HTTPS, minimum TLS 1.2, Email Obfuscation off, SSL Full. The zone holds the iCloud Mail records.
- **GitHub Pages (rollback):** legacy Jekyll build of `main` at `/`, custom domain in [CNAME](CNAME), HTTPS enforced. [_config.yml](_config.yml) only lists what Jekyll must not publish.
- **Decisions:** [docs/DECISIONS.md](docs/DECISIONS.md).

## Rules and constraints

Universal rules live only in the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md). Project-specific rules:

- Every GitHub or Cloudflare change (DNS, Worker, route, rule, deployment, build settings, zone settings, tokens, hosting) needs its own owner-approved gate, staging included. Connected tools never imply approval (D-CDW-002).
- A push to `main` is a production deploy. Every push gate checks the Workers build and deployment it starts and the live files afterwards (D-CDW-010).
- The repository root is public on both hosts. A new file is served unless `.assetsignore` lists it and Jekyll skips it (dotfiles, `_` files, the `_config.yml` exclude list). Any gate that adds a file checks both (D-CDW-008).
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

Close out the move to Cloudflare: decide the GitHub Pages rollback window before 2026-11-21, then retire what is no longer needed, one owner-approved gate at a time.

## Last closed gate

R14, production cutover to the Worker, closed 2026-10-26. It added the route and the `www` redirect rule and switched the five site records to proxied; every check passed and no rollback ran. Evidence: report `2026-10-26_12-00-11-Website-Cutover-Switch.txt`, SHA-256 `a3a3ad8a91ff2ec12262713a2fbc7d1e980490e213097d9a246471cf72e384d5`.

## Next gate

A decision gate on the rollback window, before 2026-11-21: retire GitHub Pages, or keep it and pin SSL to Full. HSTS and removing the staging hostname are decided with it. This is a recommendation, not authority; it needs Master review and owner approval before execution.

## Open work and deferred items

- Rollback window: GitHub's certificate for the site expires 2026-12-01 and won't renew while the records are proxied. Automatic SSL/TLS scans on 2026-11-21 and may raise SSL to Full (strict), which would break the route-deletion rollback once that certificate lapses.
- GitHub Pages retirement (D-CDW-001), HSTS, and removing the staging hostname (one DNS record, one Worker route): each its own gate.
- The CryptoBeam product page still says "CryptoBeam v1.0 · Available now". Changing it is protected content (D-CDW-004).
- Clean-up, each optional and its own gate: merged branches on GitHub, the owner's local checkout, and the stale Claude Code memory notes for it.

## Open questions

None blocks the next gate. Unresolved:

- How the `cryptobeam/` pages relate to the separate `cryptobeam-privacy` repository, which Agent Governance records as ownership-unresolved. Don't guess ownership, migrate or change either.
- The purpose of the `codex/website-launch-phase-1-2` branch and its local Codex worktree.

## Cautions and protected scopes

- `main` is canonical and is production's source: a push to `main` deploys the live site. Commit and push are separate gates.
- The route, the `www` redirect rule, the five site records and the zone settings carry production; change them only in their own gate.
- [CNAME](CNAME) binds GitHub Pages' custom domain; change it only in a hosting gate.
- `cryptobeam/` pages and stylesheet: protected; no change without its own approval (D-CDW-004).
- The iCloud Mail DNS records (MX, SPF, verification): never touched.
- The staging DNS record, route and Worker: no silent change, rollback, recreation, deletion or repair.
- Other projects' repositories, zones and Cloudflare resources: out of scope.
- **Other repositories:** this memory grants no authority over another repository.

## Recent meaningful changes

- 2026-10-26: R14 moved production to the Worker (D-CDW-009).
- 2026-10-25: R13 set the zone's HTTPS and TLS settings and made Workers Builds deploy every push (D-CDW-010).
- 2026-10-25: R11 and R12 updated `/cryptobeam/privacy/` to CryptoBeam's October 24, 2026 policy (PR #4).
- 2026-10-19: R9 and R10 updated it to the October 13, 2026 policy (PR #3).
- 2026-10-18: R5 to R8 adopted this Project Memory (PR #2); a fresh-agent reconstruction passed, and Agent Governance's registry records the adoption.

## Load map

- **Tier 0 (always):** the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md) and this file.
- **Tier 1 (as the task requires):**
  - [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md): memory format.
  - [docs/DECISIONS.md](docs/DECISIONS.md): decisions.
  - [wrangler.jsonc](wrangler.jsonc), [.assetsignore](.assetsignore), [_config.yml](_config.yml), [CNAME](CNAME): hosting and publication rules.
- **Tier 2 (only to re-prove disputed facts):** the reports in the owner's Diff_Reviews folder named with SHA-256 in docs/DECISIONS.md. Never required for normal hydration.

To resume in any role on any provider: load Tier 0, verify repository, branch and HEAD read-only,
report a hydration receipt per the contract, then continue from the next gate.
