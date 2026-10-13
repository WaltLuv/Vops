# PropBot and Vops feature parity audit

Audited source: PropBot `8ffb8d795987b7809115ed18ca5199ba96b4730e` and Vops
`92ec73d194a60601c17ae2542aac2c61761e48b0`. Findings below describe inspected
code, not fresh live-provider acceptance. See `INTEGRATION_DESIGN.md` for the
proposed combined product and remaining delivery gates.

| Capability | PropBot source | Vops source | Integration decision |
| --- | --- | --- | --- |
| Operations desk and navigation | `app/operations-console.tsx` | `components/app/bops-app.tsx`, `sidebar.tsx`, `panel-tabs.tsx` | PropBot operations navigation with Vops' task/chat/computer panels |
| Profiles and specialized employees | `lib/agent-profiles.ts`, `agent-profile-store.ts` | `lib/server/bots.ts`, `lib/types.ts`, `components/app/bot-panel.tsx` | Stable employee mapping; retain roles, models and permission policy |
| Private conversations | `app/api/agent-profiles/[id]/chat/route.ts`, `lib/agent-runtime.ts` | `lib/server/chat.ts`, `app/api/chats/`, `components/app/chat-view.tsx` | One visible conversation system; preserve owner/private histories |
| Live assistant/runtime | `lib/agent-runtime.ts`; configured endpoints still absent in the saved installation | `lib/server/chat.ts`, `sessions.ts`, `computer-task.ts` | Preserve selectable Hermes profiles and Vops execution paths behind the same governed tools |
| Handoffs and team chat | Operations UI contains team concepts; live orchestration is incomplete | `lib/server/chat.ts` task dispatch and teammate functions; pair/group Chat types | Reuse Vops primitives, add tested asynchronous delivery and bounded room policies |
| Computer provider | Browserbase default and optional `lib/orgo-client.ts` | `lib/server/orgo.ts`, `orgo/`, `vm/` | Orgo active/default; preserve historical Browserbase source/records without using it for new work |
| Agent computer execution | Approved single-action path in `lib/computer-use.ts` and assist route | `lib/server/computer-task.ts`, `sessions.ts`, `vm/screen_mcp.py` | Use actual agent execution with PropBot's governed job/result contract |
| Live computer UI | `app/live-computers.tsx` screenshot polling | `components/app/live-desktop.tsx`, `live-screen.tsx`, `rtc-desktop.tsx`, `mirror-screen.tsx` | Retain rich live viewing and add protected evidence/work context |
| Human takeover | `lib/computer-control.ts`; expiring input guard and late-failure protection | `app/api/takeover/route.ts`, `lib/server/sessions.ts` takeover/return | Durable physical-computer ownership and final dispatch checks; same-job resume |
| Queue and priority | `lib/workstation-queue.ts`, durable operator records | `lib/server/sessions.ts` oldest-first screen pump | One durable agent queue, explicit priority policy; claiming must actually execute work |
| Screenshots and artifacts | `lib/browser-evidence.ts`, `artifact-store.ts`, authenticated artifact file route | Screen/file/attachment components, recording packages and computer execution | One owner-scoped evidence library; verify recording availability and replay rather than inferring it |
| Routines and schedules | Workflow catalogue; durable wakes not complete | `lib/server/routines.ts`, screen watches and related API routes | Reuse routines; add restart-safe, bounded scheduled dispatch and event delivery |
| Memory | Isolated configured profile adapter; private chat persistence | `lib/server/memory.ts`, memory categories/components, Honcho workspace banks | Preserve workspace memory; add explicit private/shared grants and tests |
| Connected apps | `lib/business-apps.ts`, connection/audit routes | `lib/server/composio.ts`, account access/Vault/app approvals | Retain Vops app-account capability and PropBot business-service adapters |
| PropControl | `lib/propcontrol-client.ts`, gateway/actions/action store/approval routes | Generic app and browser tooling | Port the typed core adapter and canonical read-back; avoid model-built requests |
| VoiceOps | `lib/demo-services.ts`, `propcontrol-ecosystem.ts`, service task routes | Voice/call/phone/channel infrastructure, `lib/server/call.ts`, `cloud/voice.ts` | Keep VoiceOps service tasks and Vops user voice/channel surfaces distinct but coordinated |
| VisionOps | Standalone service/session routes and canonical ecosystem adapter | Attachment/computer/generic app facilities | Preserve authenticated inspections/findings/scope/estimate flows and external source IDs |
| Maintenance missions | `lib/maintenance-mission-orchestrator.ts`, mission store/events/routes | Sessions/tasks and connected tools | Link a real agent job to the mission state machine, approvals and evidence |
| Owner reports | `lib/owner-reports.ts`, `app/owner-report.tsx`, report approval/audit | Generic CRM, files, chat reports | Port reconciled reports, source files, prior comparisons and review warnings |
| Valuations | `lib/rentcast-client.ts`, valuation panel/routes | Business data tools | Retain RentCast readiness, provenance and source-backed valuations |
| Workflow catalogue | `app/propcontrol-catalog.ts`, workflow capability library | Routines/task dispatch | Connect runnable entries to governed tasks; keep unverified entries honest |
| Credits and billing | `lib/usage-credits.ts`, credit store/account/event tables | Usage tracking, account/credit routes, cloud credit/plans | Preserve PropBot ledger and paid intent; reconcile usage without adopting official Bops plans |
| CRM | Domain/source-record references | `lib/server/crm.ts`, `components/app/crm.tsx`, CRM tools/CSV | Retain Vops CRM and links to canonical business records |
| Channels | Ecosystem ingest and private service tasks | Slack, Telegram, Discord, phone, email and voice code | Preserve configured channels with authenticated callers and governed actions |
| Mobile/desktop | Responsive PropBot operations UI | Responsive conversation components, Electron/Mac and local-task paths | Cloud-first product; retain supported desktop capability and verify phone usability |
| Persistence | Workers D1/R2, Drizzle schema and migrations | File store, Postgres JSONB/message persistence, cloud/relay | Add portable PropBot repositories and a verified repeat-safe migration |
| Auth and isolation | Trusted ChatGPT identity and owner checks | Orgo sign-in, keychain/self-hosting, host/window-token proxy, pinned-user Postgres | Explicit authenticated owner mapping; do not equate in-process state with multi-user isolation |
| Restart/recovery | Ambiguous browser reservations retained; guarded preparation recovery | Active sessions marked failed on reload; restart persistence and provider healing | Reconcile external sessions/attempts and leases before dispatch; no blind duplicate write |
| Brand and licensing | PropBot name and operations UI | Upstream Bops branding, FSL license and trademark files | PropBot identity with retained notices; resolve competing-commercial-use licensing before paid launch |

## Verified audit limits

- Neither a Git clone nor a source-code feature proves live integration.
- PropBot's remaining acceptance gates are preserved in the recovered
  `docs/BACKEND_VERIFICATION.md` and `docs/PROJECT_HANDOFF.md`.
- Vops' test harnesses cover many domains but were not executed in this audit.
- No dependency install, provider action, new computer, source data import,
  pricing change, runtime merge, or deployment occurred in this design step.
- Both source histories remain intact. Subsequent work must record new test and
  live acceptance evidence against the actual combined commit.
