<img src="https://my-badges.github.io/my-badges/fix-4.png" alt="I did 4 sequential fixes." title="I did 4 sequential fixes." width="128">
<strong>I did 4 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/govql/govql/commit/ae34d6ea03df7fe7223393eb8c7f6d7dbce6d24f">ae34d6e</a>: fix(us-congress): surface non-converged fetch runs to monitoring

A fetch run that gives up unverified (persistent anomalous API page)
used to record success and ping the dead-man's-switch healthcheck,
leaving a wedged fetch invisible. succeedRun now merges an outcome
object into ingestion_runs.source_params (fetch runs record verified +
passes, so wedge streaks are queryable), the completion log is
warn-level when unverified, and the healthcheck ping is withheld on
non-converged runs so persistent wedging trips the monitor. The jsonb
merge uses '{}' rather than NULL for the no-outcome case — jsonb || NULL
is NULL and would have wiped the params recorded at openRun.
- <a href="https://github.com/govql/govql/commit/299b2ab7358545abdb3e335798c6c0afbab793bf">299b2ab</a>: fix(us-congress): round-3 review closeout — load serialization, truncation-aware verification, docs truth

Serialize load runs with their own advisory lock and make the load
cursor monotonic so an overlapping manual run can't rewind it; teach the
fetch walk to report whether it reached the API's true end so a pass
truncated by an anomalous empty-page-with-next never counts as a clean
verification; reject non-numeric bill numbers in transform before they
can clobber a real bill row. True up every stale documentation surface:
bills and source_state table COMMENTs restated in V005/V006 (schema docs
regenerated), fetch-bills header describes the two-cursor design,
PLAN.md's Staged cursors decision records the API-source watermark pair,
and the cheap-no-op acceptance note carries the grace-window exception.
- <a href="https://github.com/govql/govql/commit/d831598214a8272880464a18c28aee28b0f0df83">d831598</a>: fix(us-congress): crash-safe fetch verification, run serialization, changelog — review round 2

Split the bills fetch watermark into a monotonic per-page resume cursor
and a fetch_verified cursor (source_state gains the stage via V006) that
only advances after a clean verification re-walk, so neither a crash nor
the pass cap can strand a boundary-skipped bill behind the cursor. Fetch
runs are serialized with an advisory lock and the load advance is
grace-capped five minutes so in-flight fetch transactions cannot commit
rows behind the load cursor. Adds the [Unreleased] changelog entry for
the new Bill fields and data source, corrects the run-log helper's
atomicity comment, counts distinct upserts across passes, and stamps
page logs with the pass number.
- <a href="https://github.com/govql/govql/commit/4f9aecec4df815847c7b1fb254b7592ec9a0023d">4f9aece</a>: fix(us-congress): harden bills connector per review — cursor safety, batching, header auth

Apply the four major review findings plus the security minor: load-stage
DB errors now fail the run instead of being skipped past (cursor safety);
the load reads raws in keyset batches so a full backfill fits the 64 MB
heap; the fetch cursor is keyed per congress so retargeting the knob
backfills instead of silently no-oping; multi-page fetch runs re-walk
from the starting cursor until clean, closing the offset-pagination skip
window; and the API key moves from the URL to the X-Api-Key header.


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>