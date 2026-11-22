# Schematic Translator record anchors

One file per day, `anchors/YYYY-MM-DD.json`, written by the `anchorChainHead`
function. Each file records where every session's critical-value hash chain
ended on that day.

## Why this repo exists

The record layer is append-only: every confirmation, reopen and dispute is a
new event whose `content_hash` is `sha256(prev_hash + canonical content)`.
Altering any historical row breaks the chain at that point.

A chain only the platform can verify is not independent of the platform. The
party with motive to alter the record also has datastore access. Committing
the head here daily means the GitHub commit timestamp is third-party evidence
of where each chain stood, and a later rewrite disagrees with an anchor that
was already published.

## What is and is not here

Only hashes and sequence numbers. No document text, no technician names, no
values. A hash reveals nothing about its input, which is why a public anchor
is safe.

## The honest limit

A head-only manifest proves where a chain ended, not that its first event
still hashes as it did. A rewrite of early events followed by new appends is
invisible to a head comparison. The cross-day check does catch disappeared
sessions, shrank chains and same-length rewrites. Setting
`ANCHOR_INCLUDE_HASHES=1` publishes every `content_hash` and enables true
prefix comparison; a Merkle tree is the answer at scale.
