<img src="https://my-badges.github.io/my-badges/fix-5.png" alt="I did 5 sequential fixes." title="I did 5 sequential fixes." width="128">
<strong>I did 5 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/govql/govql/commit/1478bab4559b18e420f3148c8a5a399b90d11a7c">1478bab</a>: fix(bills): validate the budget env var as a raw digit string

parseInt's prefix parsing let '1e3' (meant: 1000) become budget 1 and '0.5'
become 0 without ever tripping the numeric fallback check — the loud-fallback
warning promised in the previous commit could not fire for exactly the values
it was added to catch. Validate /^\d+$/ on the raw string instead.

Part of #89
- <a href="https://github.com/govql/govql/commit/09f972d2c5233ab1e55a49379461e7e1d1df2000">09f972d</a>: fix(bills): apply round-4 review findings — TCP readiness probe, invariant pin, loud budget fallback

- The integration runner probes postgres over TCP (-h 127.0.0.1): the image's
  init phase runs a socket-only temp server a bare pg_isready would answer
  for, racing the real server and flaking the deploy-gating CI job
- workflow.test.js pins the new test:integration CI step, matching the repo's
  guarantees-and-invariant-tests-change-together convention
- The budget env fallback logs the rejected value, and 0 is now a legitimate
  explicit pause (the exhausted latch reports such runs as unverified
  zero-request runs, not dead-but-verified ones)

Part of #89
- <a href="https://github.com/govql/govql/commit/53854ffd37134a3e2d729b02ce294cf641c9ebcd">53854ff</a>: fix(bills): apply round-3 review findings — budget NaN latch, page-relative next, CI integration tests

- requestBudget latches exhausted on any refusal: a NaN limit (malformed
  CONGRESS_GOV_HOURLY_REQUEST_BUDGET) previously refused every request
  without latching, so dead runs reported as verified successes; fetch-bills
  also validates the env var and falls back to the default
- pagination.next resolves against the URL of the page that returned it, so
  path-relative links keep their /v3 segment instead of 404ing the run
- The pg-integration suite now runs in CI (new workflow step; ubuntu runners
  ship Docker) instead of being manual-only
- Sub-endpoint 404 counts (fanoutNotFound) surface in the fetch run summary
  and ingestion_runs outcome, symmetric with fanoutSkipped

Part of #89
- <a href="https://github.com/govql/govql/commit/237dbe99048f526389b7e1507bfe269078e7438f">237dbe9</a>: fix(bills): apply round-2 review findings — pagination 404 safety, backstop, pg integration tests

- A 404 on a pagination FOLLOW now fails the run (retried next tick) instead
  of storing an empty payload over real child rows; 404-as-empty applies only
  to an endpoint's first request
- pagination.next resolves relative URLs against the base URL before the
  origin check (a relative next link is origin-safe, not an error)
- Changed-check gains a has-detail backstop: a listed bill with no
  bill-detail raw fans out even when its list payload is unchanged
  (self-healing for list rows landed without sub-entities); V008 documents
  the deploy-order requirement
- Summaries loader batch size drops to 50 (full CRS text under the 64 MB
  heap cap — an OOM there is a crash loop)
- applyDetail/applyTitles get a no-op WHERE guard (explicit casts) so
  updated_at never bumps when no value changes
- fanoutSkipped propagates through fetchPagesUntilClean into the run summary
  and ingestion_runs outcome
- New npm run test:integration: *.pg-integration.test.js against a throwaway
  dockerized Postgres migrated with the real migrations — pins the jsonb
  changed-check and COALESCE/updated_at semantics (and caught the missing
  parameter casts on its first run)
- Docs generator unescapes SQL doubled quotes; stale 'committed page'
  wording updated to 'committed chunk'

Part of #89
- <a href="https://github.com/govql/govql/commit/07180fe4614f53a955eccd36cbb0ad81f74e0173">07180fe</a>: fix(bills): apply task-0011 review findings — backfill reset, txn-safe fan-out, hardening

- V008: one-off reset of bill-list raws + fetch cursors so the fan-out
  backfills bills already ingested by 0010 (the diff guard would otherwise
  see them as unchanged forever)
- Fan-out HTTP now runs before the chunk transaction (read-only changed
  check, in-memory payloads, DB-only txn) — slow requests can no longer
  hold fetched_at behind the load cursor's grace cap; 30s per-request timeout
- Sub-endpoint 404s store an empty payload (logged) instead of failing the
  run and wedging the pipeline on one bill; 5xx still fails loudly
- pagination.next is followed only on the configured API origin (the key
  header must not leak to a response-controlled host)
- transformDetail handles the null payload a 404 stores; transformCosponsors
  dedupes repeated bioguideIds; per-batch bill-existence probe replaces the
  per-row N+1; dead maxUpdateDate removed
- Fixtures replaced with recorded /v3/bill/119/s/5 responses; docs generator
  naming maps fixed (undefinedsConnection); CHANGELOG entries added;
  CONNECTORS.md admits per-entity replacement; summary_text documented as
  unsanitized upstream HTML; error-asymmetry + omitted-fields tests added

Part of #89


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>