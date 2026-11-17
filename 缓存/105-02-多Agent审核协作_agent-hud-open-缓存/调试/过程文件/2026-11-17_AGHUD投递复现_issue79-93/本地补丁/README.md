# Local patches (reference only, not rebased)

These were written against upstream `b4fcb0e` and replayed in filename order.
They are not rebased onto current main, so treat them as reference
implementations rather than a PR.

- `02-message-only-session-reads.patch` — core/sessions/detail.rs: a
  `messagesOnly` path returns the message page without loading token totals,
  billing or tool-call history. Motivation: message reads and sends were
  stalling while large session data was rebuilt.
- `04-first-visible-quota-row.patch` — hud/vendors.rs: pick the HUD quota row
  from the first visible row (issue #91), with legacy window-id matching.
- `05-refuse-conflicting-targets.patch` — messaging/send.rs: refuse when `to`
  and `reply_to` resolve to different sessions (reason names both); the inbound
  header gains `· sender <sessionId>`.
- `06-sent-outbox-tool.patch` — messaging: `list_sent_messages` read view over
  the journal (issue #92): id, recipient client/session, state, sent time,
  replyTo.
