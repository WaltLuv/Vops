# PropBot on Vops with Orgo

Status: proposed integration design for owner review; application code has not
yet been integrated. Audit date: 2026-10-13 UTC.

## Product intent

Build the existing PropBot operations product into `WaltLuv/Vops`, preserving
both applications' useful features and strongest interaction patterns. Orgo
replaces Browserbase as the active computer provider. The supplied Grok Bot
blueprint remains an architectural acceptance reference, not the starting
application or a reason to rebuild completed work.

The primary experience is:

1. The user gives PropBot a job in text or voice.
2. A director assigns it to an appropriate employee and creates durable work.
3. The employee uses direct service tools first and an Orgo computer when useful.
4. The user sees current activity, opens the live computer, and can take control.
5. Handing control back resumes the same job from the actual current state.
6. PropBot verifies source records or observable results and saves evidence.
7. The job reports completed, needs approval, blocked, cancelled, or failed honestly.

A manually operated computer and an agent's narrative are not sufficient proof
that a job was completed. Existing price/credit policy must not be replaced by
Bops' advertised free plan. The user requested a ceiling of approximately
10–13 implementation slices across the four remaining phases; this design
budgets eleven and reuses existing systems throughout.

## Authoritative source checkpoints

| Source | Audited commit | Current source shape |
| --- | --- | --- |
| PropBot, saved Sites version 19 | `8ffb8d795987b7809115ed18ca5199ba96b4730e` | 215 tracked files, 34 API routes; Vinext/React, Workers, D1, R2 |
| `WaltLuv/Vops` main | `92ec73d194a60601c17ae2542aac2c61761e48b0` | 588 tracked files, 83 API routes; Next.js/React, Node, file/Postgres persistence, Electron and cloud services |

The PropBot checkout removed by workspace maintenance was recovered from its
saved remote source at the exact commit above. Its deployed application and
hosted records were not changed. Vops was cloned from its own repository and
inspected with its repository guidance. No reset, force push, or replacement of
Vops main is part of this design.

The PropBot record of 64 passing tests and a successful build is historical
verification from September 13. Vops' test scripts are present in source. No
new baseline or live acceptance pass is claimed by this document.

## Selected architecture

Use Vops' existing Next.js/Node application as the target runtime. Retain its
chat, agent/session execution, Orgo, computer viewing, takeover, connected-app,
voice/channel, CRM, routine, and memory components. Port PropBot's domain logic
and operations views into those systems; do not put two separate applications
behind tabs or run a second director, queue, or billing ledger.

Port under explicit domain boundaries, such as `lib/propbot/` for service and
operations logic, `components/propbot/` for operations views, and namespaced
PropBot routes. These are proposed boundaries, not a requirement to move Vops'
existing implementation wholesale. Existing route contracts and stored IDs
remain compatible until their replacement has a tested migration.

The target requires a persistent Node runtime: it uses filesystem/process
operations, background execution, WebSockets, and Postgres. Do not convert
Vops into the old Sites Workers runtime merely to preserve the prior host.
Keep the existing PropBot deployment available as the recovery reference until
the combined application passes acceptance. Deployment destination and provider
credentials must be checked at implementation time; no new paid infrastructure
or live computer has been created by this audit.

### Alternatives considered

- Selected: port PropBot's operations into Vops' working workforce/runtime.
  This makes Orgo and the richer agent experience the foundation while reusing
  PropBot's domain code. It requires a deliberate data/auth migration.
- Put the old PropBot application next to Vops: quick access to both interfaces,
  but duplicate state, identity, conversations, queues, and credits. This does
  not satisfy a combined product and is not the implementation direction.
- Replace Vops with the old PropBot shell: would discard the mature computer,
  chat, channels, and task work that motivated selecting Vops. Not selected.

## UI and UX composition

Keep PropBot as the product name and use its own branding. Vops' upstream
copyright and attribution remain in legal/about documentation; Bops' name,
logo, and mascots are not the combined product's branding.

- Today becomes the default operations desk: one clear job composer, active
  work, items needing a decision, recent verified results, and service status.
- Work shows durable jobs in list/Kanban/calendar views, including priority,
  assignee, current action, approval state, evidence, and source record links.
- Team keeps PropBot's specialized employee roles and profiles while using
  Vops' private chats, task threads, model controls, and profile panels.
- Chats and Rooms use Vops' conversation components and explicit membership.
  Private chats remain private; group work receives only shared context.
- Computers uses Vops' live desktop, screen selection, activity, Watch/Take
  control/Return controls, and reconnect behavior, with server-enforced leases.
  PropBot's screenshot/evidence features sit in the same work context.
- Workflows retains PropBot's catalogue and service actions and connects runnable
  entries to actual agent/tool jobs. Unverified entries show setup or readiness.
- Approvals and Evidence are first-class work filters and detail panels. A
  decision identifies its action, target, expiry, source, and result. Evidence
  remains accessible after a computer session ends.
- Settings preserves Vops' app accounts/Vault, channels, memory, routines and
  usage views, adding PropBot's business-service connections and permission
  policy. Provider setup must show real readiness rather than placeholder success.
- CRM, voice, email, SMS, Slack/Telegram/Discord, local Mac capability and
  desktop distribution remain available where actually supported. Cloud-first
  PropBot completion does not require expanding the Mac packaging work.

On a phone, the job/chat is primary, with activity, approval, and computer
controls reachable without a desktop-only sidebar. Preserve the best existing
layouts and components; avoid duplicating controls or inventing a new design
system. Personal greetings come from the authenticated profile, not hard-coded
operator names. UI success states must derive from durable/backend status.

## One execution and ownership model

Use stable IDs for employee, workspace, conversation, job, attempt, approval,
computer assignment, and artifact. Map original PropBot agent IDs to target
employees without rewriting their conversation history.

The same job record owns its service calls, Orgo request, evidence, approval,
and verification. Vops' session/thread IDs remain linked to it rather than
creating an unrelated operator task. A human changing a queue row must not
create a false completed result.

Vops currently executes computer tasks through OpenAI Responses computer use
and supports Agents API/Codex task paths. PropBot has a selected Hermes profile
adapter, but no connected Hermes endpoints in the inspected installation.
Preserve Hermes as an explicit supported profile/harness integration and retain
Vops' working execution paths as alternatives. Do not silently switch an
existing employee's chosen harness or claim that Hermes is live. All harnesses
must use the same governed business tools, computer leases, approvals, and
artifact/verification contracts before being permitted to mutate systems.

Durable states distinguish queued, planning, running, waiting for approval,
waiting for human input, stopping, verifying, completed, failed, and cancelled.
A restart reconciles an attempt and its external session before dispatching
again. Ambiguous external acceptance stays explicit; it never becomes a blind
retry of a side effect.

## Storage, identity, and migration

Retain Vops' file storage for supported local installs and Postgres capability
for persistent hosting. Put PropBot's structured domains in additive tables
and repository interfaces rather than introducing D1 inside the Node app.
Existing Vops state/message schemas and IDs stay readable.

Create a tested export/import path from PropBot's D1 records and R2 objects.
The migration includes agent profiles, private transcripts/runs, business
connections and encrypted sessions, credit accounts/events, reports/audit,
missions/events, actions/approvals, queue records, artifacts, and valuations.
Preserve IDs, ownership, timestamps, source record IDs, credit amounts, object
references, and audit history. An import is dry-run first, repeat-safe, and
reconciles record counts, balances, and file hashes. Provider encryption keys
stay private and use an explicit decrypt/re-encrypt or retained-key procedure;
never treat an encrypted token as portable without its original key.

Old ChatGPT owner-email identities need an explicit mapping to target account
IDs. Neither matching a display name nor accepting a client-provided email
establishes ownership. Artifact retrieval, service tokens, approvals, private
chats, and memory are authorized on every server request.

Vops' hosted server can hold one user's state in process memory; its current
host/proxy checks are not a general multi-tenant login boundary. The initial
combined deployment remains private to the authorized operator with a pinned,
authenticated identity. Public/company rollout requires verified per-request
account routing and isolation; a workspace switcher alone must not imply it.

## Orgo computer contract

Orgo is the only active/default provider in the combined product. Use Vops'
existing Orgo provisioning and live-screen stack. Preserve old Browserbase
source/history and historical records for recovery; do not create, terminate,
or migrate a provider session merely by importing source.

Default to one shared Orgo computer, not one paid computer per employee. The
budget and configured Orgo capacity determine provisioning. Keep multi-screen
viewing and Vops' larger-capacity support available where explicitly configured.
Screen visibility is not proof of filesystem or user isolation.

A durable lease covers the physical computer and its input assignment, with
an owner, job/attempt, control epoch, expiry, and heartbeat. The default policy
allows one controller at a time. Direct API work can run concurrently. Computer
work enters a priority/FIFO queue; claims and lease transitions are atomic and
visible. Optional concurrency must be an explicit tested resource policy,
never an accidental result of several screens existing.

Human takeover revokes pending agent input before granting input. Check that
lease immediately before external dispatch, after slow screenshot/preparation
steps. Already-dispatched input may drain; uncertain results require inspection.
Return resumes the existing job against the actual screen, not a replacement
computer or stale proposal. Expired leases, lost clients, provider sleep/wake,
restart, cancellation, and confirmed termination have distinct recovery paths.
Do not clear an unknown creation or reuse a controller token blindly.

Protect live-view/input connections with owner-scoped, short-lived authority.
Hiding iframe pointer events is not authorization. Credentials and reusable
remote control tokens must not appear in public pages, logs, or artifacts.
Retain screenshot/video/recording capabilities where the provider or existing
implementation actually supplies them, with protected playback and retention.
Recordings must not be promised merely because rrweb is installed.

## Business tools and the real maintenance mission

Prioritize PropControl, VoiceOps, VisionOps, owner reports, and valuations.
Retain Vops' Composio/app integrations and CRM without moving the core ecosystem
behind a generic connector. Later PM connectors remain optional additions.

Port the existing PropBot clients and domain validation; do not replace them
with model-generated HTTP requests. Tools have typed input, stable idempotency
keys, owner/workspace scope, structured results, permission checks, and source
record IDs. Sending, creating, scheduling, closing, paying, deleting, and
publishing respect the configured approval ceiling. A prompt is not the gate.

The critical acceptance mission is authorized intake → durable job → agent
plan → direct tools/Orgo as needed → approval where required → source read-back
→ saved evidence → verified completion. Reuse the current maintenance
orchestrator's canonical IDs, retry keys, and review/blocked behavior. Do not
require a cosmetic stage animation to pretend that an external record exists.
Pending credentials or service review must remain visible and resumable.

Owner reports retain reconciliation and review warnings. Valuations retain
their source/time and setup requirements. Catalogue descriptions are not
treated as 57 working workflows before per-capability acceptance evidence.

## Workforce and automation

Reuse Vops' specialist bots, group/pair chats, task dispatch, routines, memory,
and channels. Add the blueprint guarantees where the audit has not proved them:

- Separate profiles, private transcripts, scoped memory, task state, and models.
- Explicit shared-memory grants; private material is not automatically injected
  into rooms or handed to another employee.
- Asynchronous handoffs acknowledge delivery; replies arrive as later events.
- Persistent wakes and priority lanes survive restart and do not let a peer
  preempt active user work without a recorded policy/reason.
- Rooms bound members, rounds, messages, history, elapsed time, and spending;
  stale room epochs cancel outdated work and acknowledgements cannot loop.
- Workflows/routines invoke the same governed executor, with timezone-aware
  scheduling, cancellation, retry policy, and actual outcome evidence.

Do not replace working Vops capabilities simply because their internal names
refer to Bops; preserve contracts and attribution while rebranding public UX.

## Credits and external dependencies

Preserve PropBot's existing balances, grants, debits, compensation keys, and
paid-product intent. Keep one authoritative product credit ledger. Vops'
provider cost/usage data remains useful but is not a second spendable balance
or permission to adopt its official Free/Pro/Max plan terms.

Retries do not double-charge. Approval, passive viewing, and inbound ecosystem
events retain their existing credit policy. Failed work records the correct
compensation separately, and an ambiguous operation is not refunded as though
it certainly never ran. Payment integrations and pricing changes need explicit
product configuration; do not point PropBot customers at Bops' official checkout.

Self-hosted/independently hosted execution uses the operator's own provider
accounts and keys. Do not assume that upstream Bops Cloud credentials, branded
email domains, signed desktop releases, free computers, or plan provisioning
are available to this fork. Preserve those code paths as supported integrations
only where correctly configured and permitted.

## Four remaining phases, eleven bounded slices

| Phase | Slice | Concrete outcome |
| --- | --- | --- |
| 2 — Real mission execution | 1 | Source/domain import, PropBot brand and unified navigation in Vops; preserve upstream notices and existing capabilities |
| 2 | 2 | Auth/storage repositories and repeat-safe source-record/credit/artifact migration |
| 2 | 3 | Governed ecosystem tools, owner reports, valuations, and connected workflow catalogue |
| 2 | 4 | First durable maintenance job executes, reads back canonical results, and saves evidence |
| 3 — Orgo computer agent | 5 | One shared physical computer, durable priority/FIFO queue, agent/human leases and cancellation |
| 3 | 6 | Protected live view, watch/takeover/return, reconnect, and pending-input revocation on the same job |
| 3 | 7 | Screenshots/recording evidence, protected retrieval, lifecycle and restart/sleep recovery |
| 4 — Multi-agent workforce | 8 | Specialized profiles, bounded rooms, asynchronous handoffs, and verified private/shared context |
| 4 | 9 | Durable wakes, routines/workflows, priority policy, and restart-safe coordination |
| 5 — Production/launch hardening | 10 | Credits/approvals, permission boundaries, recovery, security, observability, and licensing/configuration gates |
| 5 | 11 | Mobile/PWA/accessibility validation, full product acceptance, deployment and operational handoff |

These are delivery checkpoints, not a fresh 30-slice rebuild or a claim that
all provider dependencies are already available. UI, tools, jobs, and computer
controls are wired vertically as each applicable slice lands. New scope is not
added just to exhaust the slice allowance. A failed check is repaired within
its slice, and genuinely owner-gated credentials/permissions are reported with
the exact dependency and completed work preserved.

## Verification and launch gates

Carry over meaningful PropBot domain tests and Vops' existing regression
harnesses. Respect `AGENTS.md` and read the installed Next.js version's docs
before implementation; do not change the lockfile gratuitously.

Require ownership/isolation, atomic claims, retry/idempotency, credit
reconciliation, approval revocation/expiry, no-input-after-pause, preservation
of old records, and migration round-trip tests. Use real database constraints
for concurrency cases. Baseline/typecheck/build results must be recorded for
the actual merged source; inherited comments or the old 64-test result are
not new verification.

Live acceptance must demonstrate a real agent task, authenticated Orgo watch
and takeover/return, approval behavior, independent service results with source
IDs, private artifact access after shutdown, and recovery after restart without
duplicated writes or unnecessary paid computers. Browser/phone validation must
cover usable controls, streaming/voice setup, and failures the user can recover.
Verify pricing/credit behavior and a tested backup/restore before launch.

The repository's FSL-1.1-ALv2 license permits internal use but restricts making
the software available as a competing commercial offering. `TRADEMARK.md`
requires a different name/logo for modified distribution, and the license
requires retaining copyright/license notices. Preserve all those files. A
paid competing PropBot launch based on this code needs the licensing basis
resolved with the rights holder or qualified review before release. The audit
does not grant permission or claim that rebranding removes the restriction.

## Immediate next step

Review this written integration design. After approval, write a concrete
implementation plan against these pinned sources, then implement in dependency
order on the integration branch, with coherent verified commits pushed to
`WaltLuv/Vops`. Preserve the original PropBot recovery source and Vops main until
the new application and migration have been verified.
