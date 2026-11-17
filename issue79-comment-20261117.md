Thanks — one correction and one repro result.

Codex ↔ DeepSeek Harness is not involved. In the 2026-11-13 records, no message on either affected chain has Codex as sender or recipient; Codex was only open concurrently. That pair can be dropped from the repro.

The Friday case is two distinct deliveries:

1) m-41f3c66a8253 — sender client deepseek, recipient client antigravity.
   The call carried `to` = the other Antigravity window, plus `reply_to` = a message from the window that was actually being answered. HUD delivered to `to`, as recorded: get_message -> state=done, to.client=antigravity, answer=null.
   This is caller-side: the wrong window was named explicitly.

2) m-13662e27b5a7 — sender client antigravity, recipient client deepseek.
   The call carried only `reply_to` = a message sent by a different DeepSeek session. get_message -> state=started, to.client=deepseek, answer=null.
   The parent's sender was not that session.

The second one matches the pre-fix behaviour your e1d02a6 describes: "a reply now goes back to the session that sent the message it answers". The build in use on 11-13 predates e1d02a6; the current build does not reproduce it.

Repro on the current build: two Antigravity windows, `reply_to` only, no `to`, three round trips. All three replies went back to the parent's sender (m-151fe0022e9e, m-0cb0b1c0b315, m-a198a5843bd1). So the misroute is not reproducible on e1d02a6 plus your later changes.

What still reproduces is the receipt problem. All three sends returned "The local tools response did not complete" although the messages were recorded and delivered. One Antigravity window retried after that error and answered the same parent twice (m-a198a5843bd1 and m-8b35214f2766). That false failure is what produced the duplicate copies in the original record.
