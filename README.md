# Agent HUD delivery records (2026-11-17)

Field records for the Agent HUD delivery problem discussed in
[jazzenchen/agent-hud-open#79](https://github.com/jazzenchen/agent-hud-open/issues/79)
(closed) and [#80](https://github.com/jazzenchen/agent-hud-open/issues/80) (open).

## What was observed

On 2026-11-13 two Antigravity windows and several DeepSeek Harness sessions were
working in parallel. Two messages landed in a window that was not the intended
one; both are recorded here with their ids, the sender's and recipient's
clients, and what `get_message` returned.

- `m-41f3c66a8253` — caller named the wrong Antigravity window in `to` while also
  passing `reply_to` (caller-side).
- `m-13662e27b5a7` — only `reply_to` was passed; the reply landed in a different
  DeepSeek Harness session than the parent's sender (matches the pre-fix
  behaviour described in `e1d02a6`).

On the current build (2026-11-16) the same shape — Antigravity replying with
`reply_to` only, no `to` — was run three times against two Antigravity windows.
All three replies went back to the parent's sender. The cross-session landing
did not reproduce.

What still reproduces is the receipt problem: all three sends returned
"The local tools response did not complete" although the messages were recorded
and delivered, and one window retried, answering the same parent twice.

Codex is not involved: no message on either chain has Codex as sender or
recipient; it was only open concurrently.

## Files

- `repro-record.md` — the evidence table (ids, clients, times, states).
- `issue79-comment-20261117.md` — the comment posted on #79 on 2026-11-17.

Everything here is desensitized: no case numbers, window titles, or session
UUIDs. The unsanitized working copies stay on the local machine.
