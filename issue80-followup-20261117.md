On the "long contexts and long runs" part of your comment, to keep the two failures apart:

The stuck case we can reproduce is sizing, not crossing. A 2310-step Antigravity conversation returns a trajectory over the 16 MB MAX_BODY, and delivery to it is refused with the quota-service error even though the quota is fine (numbers on #93). From the caller side that looks like "stuck", but it is a separate failure from a message landing in the wrong session.

What we have not seen on the current build is a conversation getting mixed up from long context alone: the wrong-session landing in the 11-13 record is the `reply_to` case above, and it does not reproduce on the current build.
