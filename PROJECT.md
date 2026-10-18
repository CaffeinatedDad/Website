# Caffeinated Dad Website — Project Memory

```text
Memory-Contract-Version: 1
Memory-Revision: 1
Base-Revision: 0
Project: Caffeinated Dad Website
Repository: https://github.com/CaffeinatedDad/Website
Branch: main
```

Tier-0 core under [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md).
Git facts (exact commit, publication, remote equality) are derived at hydration, not recorded here.
This file grants no execution authority.

## Identity and mission

This repository is the source of caffeinateddad.dev: the Caffeinated Dad home page and the CryptoBeam product, privacy and support pages. The work here keeps that site online and correct while its hosting moves from GitHub Pages to Cloudflare.

## Current state

- GitHub Pages serves production from `main`. Every change to `main` republishes the live site.
- The Cloudflare Worker serves the same files only on `staging.caffeinateddad.dev`; production DNS does not point at it.
- Workers Builds is connected but contained: a push starts no build (D-CDW-006).
- Production cutover is on hold (D-CDW-005).

## Architecture summary

- **Site:** static files at the repository root: `index.html`, `assets/` and `cryptobeam/`. No framework, package manager or build step.
- **GitHub Pages (production):** legacy Jekyll build of `main` at `/`, custom domain in [CNAME](CNAME), HTTPS enforced. [_config.yml](_config.yml) only lists what Jekyll must not publish.
- **Cloudflare Worker `caffeinateddad-website` (staging; the production target):** static assets from the repository root, no Worker script, `workers.dev` and preview URLs off ([wrangler.jsonc](wrangler.jsonc)). [.assetsignore](.assetsignore) lists every file it must not serve.
- **DNS:** the zone is on Cloudflare; its production records still point at GitHub Pages. It holds the iCloud Mail records.
- **Decisions:** [docs/DECISIONS.md](docs/DECISIONS.md).

## Rules and constraints

Universal rules live only in the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md). Project-specific rules:

- Every GitHub or Cloudflare change (DNS, Worker, route, deployment, build settings, tokens, hosting) needs its own owner-approved gate, staging included. Connected tools never imply approval (D-CDW-002).
- Until Workers Builds is set up again, every push gate states that the push is non-empty and adds fewer than 20 commits (a squash merge counts as one), and Master reads the Worker's build list after each push (D-CDW-006).
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

Move caffeinateddad.dev from GitHub Pages to the Cloudflare Worker one owner-approved gate at a time, keeping GitHub Pages enabled until a separate decision retires it; cutover is on hold, and Project Memory adoption comes first.

## Last closed gate

R4, Project Memory preservation discovery (read-only), closed 2026-10-18. It inventoried the repository, protected scopes and publication exposure, and changed nothing. Evidence: report `2026-10-18_13-28-34-Website-Project-Memory-Adoption-Discovery.txt`, SHA-256 `0637ebacbf2795da8c5541c1174f43ad224a5e1a085f295fe628d179f3bfa653`.

## Next gate

A fresh-Master reconstruction test of this Project Memory (Agent Governance adoption step 7), then the registry update in Agent Governance's own gate (step 8). This is a recommendation, not authority; it needs Master review and owner approval
before execution.

## Open work and deferred items

- Production cutover: on hold (D-CDW-005); its own gate. Before it, review the zone's HTTPS, TLS, redirect and charset settings recorded in R1.
- The staging hostname (one DNS record, one Worker route): kept; removing it is a later gate.
- Workers Builds: contained (D-CDW-006); setting it up again or removing it is a later gate.
- GitHub Pages retirement: only after cutover, by its own decision (D-CDW-001).
- `/wrangler.jsonc` has been public on GitHub Pages since 2026-10-18. It holds no secrets.
- Optional clean-up of the owner's local checkout (merged `cloudflare-hosting` branch, stale remote-tracking ref, `.wrangler/`): its own gate.
- The owner's Claude Code memory notes for this checkout are partly stale (R4). They are not canonical.

## Open questions

None blocks the next gate. Unresolved:

- How the `cryptobeam/` pages relate to the separate `cryptobeam-privacy` repository, which Agent Governance records as ownership-unresolved. Don't guess ownership, migrate or change either.
- The cutover method (a Worker route on the existing records, or a Workers Custom Domain).
- The purpose of the `codex/website-launch-phase-1-2` branch and its local Codex worktree.

## Cautions and protected scopes

- `main` is canonical and is GitHub Pages' production source: a push to `main` changes the live site. Commit and push are separate gates.
- [CNAME](CNAME) binds the custom domain; change it only in a hosting gate.
- `cryptobeam/` pages and stylesheet: protected; no change without its own approval (D-CDW-004).
- The iCloud Mail DNS records (MX, SPF, verification): never touched.
- Production DNS and live traffic: unchanged until a cutover gate. GitHub Pages stays enabled.
- The staging DNS record, route and Worker: no silent change, rollback, recreation, deletion or repair.
- Other projects' repositories, zones and Cloudflare resources: out of scope.
- **Other repositories:** this memory grants no authority over another repository.

## Recent meaningful changes

- 2026-10-18: R4 found that root files are public on both hosts (D-CDW-008).
- 2026-10-18: R3 narrowed the Workers Builds token (D-CDW-007).
- 2026-10-18: R2 contained Workers Builds (D-CDW-006).
- 2026-10-18: R1 recorded the out-of-process changes as the baseline, without authorizing them (D-CDW-003).
- 2026-10-18: PR #1 (`5f27440`) added the Worker configuration; then, outside the workflow, the Worker, its Builds connection and the staging hostname were created.

## Load map

- **Tier 0 (always):** the [universal policy](https://github.com/CaffeinatedDad/agent-governance/blob/main/UNIVERSAL-POLICY.md) and this file.
- **Tier 1 (as the task requires):**
  - [Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md): memory format.
  - [docs/DECISIONS.md](docs/DECISIONS.md): decisions.
  - [wrangler.jsonc](wrangler.jsonc), [.assetsignore](.assetsignore), [_config.yml](_config.yml), [CNAME](CNAME): hosting and publication rules.
- **Tier 2 (only to re-prove disputed facts):** the R1, R2 and R4 reports in the owner's Diff_Reviews folder, named with SHA-256 in docs/DECISIONS.md. Never required for normal hydration.

To resume in any role on any provider: load Tier 0, verify repository, branch and HEAD read-only,
report a hydration receipt per the contract, then continue from the next gate.
