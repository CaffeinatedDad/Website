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
- Status: Superseded by D-CDW-011
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
- Status: Superseded by D-CDW-009
- Decision: no production cutover and no production DNS or traffic change
  until the owner lifts the hold in a later gate.
- Rationale: the out-of-process work had to be reconciled and governance put
  in place first.
- Supersedes: None
- Evidence/Source: the owner's instruction of 2026-10-18, approved as this
  record in gate R5 on 2026-10-18.

## D-CDW-006 — Workers Builds is contained; push-gate rule

- Date: 2026-10-18
- Status: Superseded by D-CDW-010
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
- Status: Superseded by D-CDW-012
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

## D-CDW-009 — Production runs on the Worker through a zone route

- Date: 2026-10-26
- Status: Superseded by D-CDW-011
- Decision: the cutover hold is lifted. caffeinateddad.dev is served by the
  Worker through the zone route `caffeinateddad.dev/*`, not a Workers Custom
  Domain. The four apex A records and the `www` CNAME are proxied and keep
  their GitHub Pages targets. A zone redirect rule sends
  `www.caffeinateddad.dev` to the apex with 301, keeping path and query. The
  zone uses Always Use HTTPS, minimum TLS 1.2, Email Address Obfuscation off
  and SSL Full. GitHub Pages stays enabled as the rollback (D-CDW-001).
  Accepted differences from GitHub Pages: directory redirects are 307 and
  relative, `/index.html` paths redirect to their directory, pages revalidate
  on every request instead of being cached for 10 minutes, unknown paths get
  a 404 with an empty body, and content types carry no charset (the pages
  declare UTF-8). HSTS is off, as it was on GitHub Pages; browsers already
  force HTTPS for `.dev` domains. GitHub Pages may warn that its DNS check
  fails; that is expected while it is the rollback.
- Rationale: no DNS record's content changes, so the mail records are never
  involved, and rollback is deleting the route, which works while SSL stays
  Full, or switching the five records back to DNS-only, which works only
  until GitHub's certificate for the site expires on 2026-12-01. With Email
  Address Obfuscation off, the pages served equal the repository's bytes.
- Supersedes: D-CDW-005
- Evidence/Source: the owner's approval of the cutover plan and R13 on
  2026-10-25 and of R14 on 2026-10-26; R13 report
  `2026-10-25_19-00-25-Website-Cutover-Prepare.txt`
  (SHA-256 `2e6b0871fc4e5602ef229552d3417b9e64fa7b76fd23f9c96735fbe3e4a3b52b`);
  R14 report `2026-10-26_12-00-11-Website-Cutover-Switch.txt`
  (SHA-256 `a3a3ad8a91ff2ec12262713a2fbc7d1e980490e213097d9a246471cf72e384d5`);
  approved as this record in gate R15.

## D-CDW-010 — Every push to `main` deploys the Worker

- Date: 2026-10-25
- Status: Active
- Decision: Workers Builds' production watch paths exclude nothing and
  preview builds stay off, so every push to `main` builds and deploys the
  Worker that serves production. The push-gate rule of D-CDW-006 ends. Every
  push gate instead checks the build and deployment the push starts and the
  live files afterwards.
- Rationale: the Worker is the production host, so `main` must deploy to it
  as it did to GitHub Pages.
- Supersedes: D-CDW-006
- Evidence/Source: R13 report
  `2026-10-25_19-00-25-Website-Cutover-Prepare.txt`
  (SHA-256 `2e6b0871fc4e5602ef229552d3417b9e64fa7b76fd23f9c96735fbe3e4a3b52b`),
  step B2; R13 approved by the owner on 2026-10-25 and closed on 2026-10-26;
  the push-gate checks approved as this record in gate R15.

## D-CDW-011 — The Worker is the only host; GitHub Pages and staging are retired

- Date: 2026-10-26
- Status: Active
- Decision: GitHub Pages is turned off, and the staging hostname (its DNS
  record and Worker route) is removed. caffeinateddad.dev is served only by
  the Cloudflare Worker `caffeinateddad-website`, which serves this
  repository's files as static assets, with no Worker script and with
  `workers.dev` and preview URLs off, through the zone route
  `caffeinateddad.dev/*`, not a Workers Custom Domain. The apex has one
  proxied A record on the documentation placeholder `192.0.2.0`, and `www` is
  a proxied CNAME to the apex, so neither points at GitHub. A zone redirect
  rule sends `www.caffeinateddad.dev` to the apex with 301, keeping path and
  query. The zone uses Always Use HTTPS, minimum TLS 1.2, Email Address
  Obfuscation off and SSL Full; Cloudflare's automatic SSL/TLS may change the
  SSL mode, which the site doesn't depend on because no request reaches an
  origin. HSTS stays off: browsers already force HTTPS for `.dev` domains.
  The rollback for a bad deploy is the previous Worker version. There is no
  fallback host: deleting the route or setting the site records DNS-only
  takes the site down. Turning GitHub Pages back on needs its own decision.
  Differences from GitHub Pages accepted at the cutover still hold: directory
  redirects are 307 and relative, `/index.html` paths redirect to their
  directory, pages revalidate on every request, unknown paths get a 404 with
  an empty body, and content types carry no charset (the pages declare
  UTF-8).
- Rationale: GitHub Pages was kept only as the rollback, and that rollback
  was about to stop working. Cloudflare's automatic SSL/TLS scan on
  2026-11-21 could raise SSL to Full (strict), which would break deleting the
  route once GitHub's certificate for the site lapsed. That certificate was
  due to expire on 2026-12-01 and could not renew while the records were
  proxied, which would break switching them back. A previous Worker version
  restores exactly the old files at once. Retiring GitHub Pages also ends a
  second build on every push and DNS records pointing at GitHub. The staging
  hostname, first made for the Phase 3 rehearsal, had no use left once R14
  moved production to the Worker. With Email Address Obfuscation off, the
  pages served equal the repository's bytes.
- Supersedes: D-CDW-001, D-CDW-009
- Evidence/Source: R17, decided by the owner on 2026-10-26 ("Retire from
  Nov 2"); R14 report `2026-10-26_12-00-11-Website-Cutover-Switch.txt`
  (SHA-256 `a3a3ad8a91ff2ec12262713a2fbc7d1e980490e213097d9a246471cf72e384d5`)
  for the route, redirect rule and zone settings; R18 report
  `2026-11-01_22-56-09_Website_Retire-GitHub-Pages-Review_v01.txt`
  (SHA-256 `04af0a069eeefb74a88dcc7ae4c7cdd923c3748963f23654a426afa012689aa3`),
  run on 2026-11-02; approved as this record in gate R19.

## D-CDW-012 — Project Memory stays in the repository and off the site

- Date: 2026-10-26
- Status: Active
- Decision: this repository keeps Project Memory Contract v1 with
  `PROJECT.md`, this log and a root `CLAUDE.md` thin adapter, as adopted on
  2026-10-18. The repository root is the Worker's publication root, so
  `.assetsignore` lists these files and every other file that is not site
  content. `CNAME` and `_config.yml`, which only GitHub Pages used, are
  removed; `.assetsignore` still lists both, so neither is served if it
  comes back.
- Rationale: a fresh session needs the workflow and current state in the
  repository, and memory files are not site content. With GitHub Pages off
  (D-CDW-011), the Jekyll exclude list and the custom-domain file have no
  reader.
- Supersedes: D-CDW-008
- Evidence/Source: R4 report
  `2026-10-18_13-28-34-Website-Project-Memory-Adoption-Discovery.txt`
  (SHA-256 `0637ebacbf2795da8c5541c1174f43ad224a5e1a085f295fe628d179f3bfa653`)
  for the adoption; R17, decided by the owner on 2026-10-26, for removing
  both files; approved as this record in gate R19.
