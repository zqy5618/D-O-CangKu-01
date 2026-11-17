# Reproduction record (desensitized)

## Field case, 2026-11-13

| Time (UTC+8) | Message id | Sender client | Recipient client | reply_to | State | get_message |
|---|---|---|---|---|---|---|
| 17:24:55 | `m-41f3c66a8253` | deepseek | antigravity | set (message from the other Antigravity window) | done | `to.client=antigravity`, `answer=null` |
| 17:26:22 | `m-cb9c84b6f02e` | deepseek | antigravity | same parent | queued | `readNotice=Application data is busy…` |
| 17:39:46 | `m-13662e27b5a7` | antigravity | deepseek | set (message sent by a different DS session) | started | `to.client=deepseek`, `answer=null` |

In the first case the call carried both `to` (the wrong Antigravity window) and
`reply_to` (a message from the right one); delivery followed `to`.

In the second case the call carried only `reply_to`; the parent's sender was a
different DeepSeek Harness session from the one that received the reply.

## Current build, 2026-11-17

| Sent | Reply | Reply landed in | Result |
|---|---|---|---|
| `m-d5a78102c67c` → Antigravity window A | `m-151fe0022e9e` | parent's sender | correct |
| `m-89a200319041` → Antigravity window A | `m-0cb0b1c0b315` | parent's sender | correct |
| `m-f85dee7a9679` → Antigravity window B | `m-a198a5843bd1` | parent's sender | correct |
| same | `m-8b35214f2766` (second reply to the same parent) | parent's sender | correct landing, duplicate content |

All three sends returned `The local tools response did not complete`; all three
messages were still recorded and delivered. Window B retried after that error,
producing two replies to the same parent.

## Conclusion

The cross-session reply landing does not reproduce on the current build. The
reproducible defect is the false failure on send, which keeps producing
duplicates because callers retry.
