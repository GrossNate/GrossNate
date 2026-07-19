<img src="https://my-badges.github.io/my-badges/fix-3.png" alt="I did 3 sequential fixes." title="I did 3 sequential fixes." width="128">
<strong>I did 3 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/govql/govql/commit/53fb559871ac439abd62b16b662fc68adf4c6ae3">53fb559</a>: fix(ingester): strip SQL strings and comments in one pass (review N1)

Comment-look-alike text inside a string literal (V001 contains
'unitedstates/*') can no longer open a block comment that swallows real
CREATE TABLE statements once a future migration adds a real */.

Part of #56 (task 0002)
- <a href="https://github.com/govql/govql/commit/ed1a8ad66663b51c7acba0d436536d8df9cc2fd4">ed1a8ad</a>: fix(ingester): second-pass review hardening (S1, SP1-2, B1-B6)

- guard all required manifest fields (upstream/reads/writes/watermark), not
  just trigger.cron; renderer tolerates missing fields with an em dash
- collectTableNames: strip comments/string literals, honor DROP TABLE,
  flexible whitespace; comment now states the deliberate divergence from
  the docs-package parser
- dedupe per-node table-existence errors
- real-repo tests immune to a leaked PIPELINE_DOCS_ROOT (lazy root
  resolution + cleaned spawn env); drift fixture temp dir cleaned up
- readiness strings now state the fetch-cursor-set precondition (double-
  null bootstrap skips); PIPELINE.md regenerated
- PLAN.md: drift check is CI-enforced via the ingester suite, not
  'npm run only'

Part of #56 (task 0002)
- <a href="https://github.com/govql/govql/commit/7920e41ab7d9dd8720f4bd2f1d7b3ef7d7676cae">7920e41</a>: fix(ingester): harden pipeline --check per task-0002 review (findings 1-7)

- error when two manifest nodes claim the same crontab line (was silent)
- validate the fixed set of known crontabs, not just manifest-referenced ones
- parse @-keyword cron shorthands instead of silently dropping them
- CREATE TABLE regex: case-insensitive, schema-qualified, quoted names
- report a node missing trigger.cron as drift instead of crashing
- readiness strings now include the load.cursor-unset bootstrap case
- CLI tests cover --check exit codes (0 consistent, 1 drifted) via
  PIPELINE_DOCS_ROOT fixture override

Part of #56 (task 0002)


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>