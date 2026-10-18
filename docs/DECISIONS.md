# Decision Log

This is Caffeinated Dad Website's Tier-1 durable decision log under
[Project Memory Contract v1](https://github.com/CaffeinatedDad/agent-governance/blob/main/docs/PROJECT-MEMORY.md).

## Rules

- The log is append-only. Never rewrite or delete a record.
- To change a decision, append a new record that names the old one in
  `Supersedes`, and only then set the old record's `Status` to `Superseded by
  <ID>`. This status change is the only permitted edit to an existing record.
- IDs are never reused. Records appear in the order they were logged, which is
  not necessarily the order the decisions were made.
- `Date` is the date of the decision. `Evidence/Source` names durable
  repository sources, commits or dated evidence ids. Tier-2 evidence is
  optional supporting proof.
- A decision grants no execution authority.

Each record has the fields: ID, Date, Status, Decision, Rationale, Supersedes,
Evidence/Source.

## D-CDW-001 — Move hosting to a Cloudflare Worker; keep GitHub Pages as the rollback

- Date: 2026-10-17
- Status: Active
- Decision: caffeinateddad.dev moves from GitHub Pages to a Cloudflare Worker
  that serves this repository's files as static assets, with no Worker script
  and with `workers.dev` and preview URLs off. GitHub Pages stays enabled until
  a separate decision retires it.
- Rationale: the owner's choice of host. Keeping GitHub Pages enabled leaves a
  working rollback.
- Supersedes: None
- Evidence/Source: commit `d0805d85a43d0bbb5995e5595e49705297ff60ad`
  (`wrangler.jsonc`, `.assetsignore`); the owner's instruction not to disable
  GitHub Pages, approved as this record in gate R5 on 2026-10-18.

## D-CDW-002 — Work runs under Agent Governance

- Date: 2026-10-18
- Status: Active
- Decision: work on this repository follows the Agent Governance universal
  policy and its gate flow: Master recommendation, owner approval, Artifacts
  package and receipt, Master verification and routing, Executor run and
  report, Master review. Commit and push are separate gates. A staging
  deployment or staging DNS change is an external mutation that needs its own
  approval. Connected GitHub or Cloudflare tools never imply permission. The
  owner designated Master, Artifacts and Executor as recorded in `PROJECT.md`.
- Rationale: work on 2026-10-17 and 2026-10-18 skipped the workflow (D-CDW-003).
- Supersedes: None
- Evidence/Source: the owner's project instructions and role designation of
  2026-10-18, approved as this record in gate R5 on 2026-10-18.

## D-CDW-003 — Out-of-process changes are recorded, not authorized

- Date: 2026-10-18
- Status: Active
- Decision: the changes made outside the workflow on 2026-10-17 and
  2026-10-18 (the PR #1 merge, the Worker and its two starter versions, the
  Workers Builds connection and its builds, and the staging DNS record and
  route) are recorded as having occurred outside the Agent Governance
  workflow. R1 made the reconciled state the baseline. That does not
  authorize them after the fact.
- Rationale: the owner asked for an honest record rather than a retroactive
  approval.
- Supersedes: None
- Evidence/Source: R1 report
  `2026-10-18_07-54-30-Website-Reconciliation-OutOfProcessReadOnly.txt`
  (SHA-256 `92488295e91ef4a6ca340d3b751fe00ad2a2b6b1f314c737101c2eb4f31367df`);
  R1 closed by the owner on 2026-10-18.

## D-CDW-004 — Protected scopes

- Date: 2026-10-18
- Status: Active
- Decision: no gate touches the iCloud Mail DNS records (MX, SPF and
  verification), changes the CryptoBeam privacy or support content, touches
  other projects' repositories, zones or applications, disables GitHub Pages
  or changes live traffic outside an approved cutover gate, or creates API
  tokens or widens permissions without asking the owner.
- Rationale: these protect mail, published policy pages and unrelated
  projects from side effects of the hosting work.
- Supersedes: None
- Evidence/Source: the owner's standing instructions of 2026-10-18, approved
  as this record in gate R5 on 2026-10-18.

## D-CDW-005 — Production cutover is on hold

- Date: 2026-10-18
- Status: Active
- Decision: no production cutover and no production DNS or traffic change
  until the owner lifts the hold in a later gate.
- Rationale: the out-of-process work had to be reconciled and governance put
  in place first.
- Supersedes: None
- Evidence/Source: the owner's instruction of 2026-10-18, approved as this
  record in gate R5 on 2026-10-18.

## D-CDW-006 — Workers Builds is contained; push-gate rule

- Date: 2026-10-18
- Status: Active
- Decision: Workers Builds keeps preview builds off, and its production watch
  paths exclude every file, so a push to this repository starts no build.
  Until Builds is set up again in a later gate, every push gate states that
  the push is non-empty and adds fewer than 20 commits (a squash merge counts
  as one), and Master reads the Worker's build list after each push.
- Rationale: an unapproved build would change the Worker as a side effect of
  a push. Cloudflare can ignore watch paths for an empty push or a push with
  many commits, so those pushes are avoided and every push is checked.
- Supersedes: None
- Evidence/Source: R2 report
  `2026-10-18_09-15-41-Website-BuildsContainment-AutoBuildsOff.txt`
  (SHA-256 `2b9d3bc982935a12a50cd61b348b243a0e1298d19fd7b2caa853a526518e4d53`);
  R2 closed by the owner on 2026-10-18.

## D-CDW-007 — The Workers Builds token keeps only what a deploy needs

- Date: 2026-10-18
- Status: Active
- Decision: the Workers Builds token for this Worker has exactly four
  permission groups: Account Settings Read, Workers Scripts Edit, User
  Details Read and Memberships Read.
- Rationale: the token was created with far more access than a static-assets
  deploy uses.
- Supersedes: None
- Evidence/Source: gate R3, approved and closed by the owner on 2026-10-18,
  with Master's read-only verification from the Cloudflare account audit log.

## D-CDW-008 — Adopt Project Memory and keep it off the site

- Date: 2026-10-18
- Status: Active
- Decision: this repository adopts Project Memory Contract v1 with
  `PROJECT.md`, this log and a root `CLAUDE.md` thin adapter. Because the
  repository root is the publication root of both hosts, `.assetsignore`
  lists these files for the Worker, and `_config.yml` excludes them from the
  GitHub Pages build. `_config.yml` also lists `CNAME`, which GitHub Pages
  excludes by default only when a site sets no exclude list.
- Rationale: a fresh session needs the workflow and current state in the
  repository, and memory files are not site content.
- Supersedes: None
- Evidence/Source: R4 report
  `2026-10-18_13-28-34-Website-Project-Memory-Adoption-Discovery.txt`
  (SHA-256 `0637ebacbf2795da8c5541c1174f43ad224a5e1a085f295fe628d179f3bfa653`);
  approved in gate R5 on 2026-10-18.
