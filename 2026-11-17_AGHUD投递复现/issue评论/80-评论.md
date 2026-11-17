I can now give ids and a repro result for this case.

Field case, 2026-11-13 — two Antigravity windows and several DeepSeek Harness sessions working in parallel:

A) `m-41f3c66a8253` — sender client deepseek, recipient client antigravity.
The call named `to` = the other Antigravity window, and also `reply_to` = a message from the window that was actually being answered. Delivery followed `to`. get_message -> state=done, to.client=antigravity, answer=null. Caller-side.

B) `m-13662e27b5a7` — sender client antigravity, recipient client deepseek.
The call carried only `reply_to` = a message sent by a different DeepSeek Harness session. get_message -> state=started, to.client=deepseek, answer=null. The parent's sender was not that session.

B is the pre-fix behaviour your e1d02a6 describes ("a reply now goes back to the session that sent the message it answers"). The build in use on 11-13 predates it.

Current build (2026-11-16): I ran the same shape three times — Antigravity replying with `reply_to` only, no `to`, against two different Antigravity windows. All three replies went back to the parent's sender (`m-151fe0022e9e`, `m-0cb0b1c0b315`, `m-a198a5843bd1`). So the cross-session landing does not reproduce on e1d02a6 plus your later changes.

What still reproduces is the receipt problem: all three sends returned "The local tools response did not complete" although the messages were recorded and delivered. One window retried after that error and answered the same parent twice (`m-a198a5843bd1` and `m-8b35214f2766`). That false failure is how the duplicates in the original record appeared.

Codex is not involved: no message on either chain has Codex as sender or recipient; it was only open concurrently.
