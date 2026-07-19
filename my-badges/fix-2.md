<img src="https://my-badges.github.io/my-badges/fix-2.png" alt="I did 2 sequential fixes." title="I did 2 sequential fixes." width="128">
<strong>I did 2 sequential fixes.</strong>
<br><br>

Commits:

- <a href="https://github.com/govql/govql/commit/c50b2e26cf61cc970e37e77de009df70b8d266b1">c50b2e2</a>: fix(us-congress): drop dead shebang from health-check-run.js

The workflow invokes it via node; the file isn't executable, so the
shebang implied an invocation that fails.

Part of #43.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>
- <a href="https://github.com/govql/govql/commit/2e6af461b60ec21546b0ccda2edd65aec9a00672">2e6af46</a>: fix(us-congress): apply 0005 review findings

Parallel probes (a round costs the slower target, not the sum); loud
config error on malformed HEALTH_*_MS overrides instead of a NaN
deadline that never times out; invariant test asserts the SSH and
checkout steps exist and are ordered before comparing indexes;
persist-credentials off on the deploy job's read-only checkout; AGENTS.md
deploy bullet now names the external health check as the deploy verdict.

Part of #43.

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>


Created by <a href="https://github.com/my-badges/my-badges">My Badges</a>