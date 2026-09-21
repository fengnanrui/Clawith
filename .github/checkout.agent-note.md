# Drone event checkout

The `clone` step in `drone.yml` owns the checkout used by image, migration,
deployment and upgrade checks. It must materialize the exact `DRONE_COMMIT`
from the event's `DRONE_REF`, including fork-only `refs/pull/*/head` commits.
Cloning upstream branches and tags alone does not fetch those pull refs.

Fetch the event ref before checkout. A missing ref or commit fails the clone
step; do not silently test a branch tip instead. Keep full history and tags for
the existing previous-release selection. This does not change triggers,
secrets, privileges or deployment permissions.

Verify with `python3 -m unittest discover -s .github/tests -v`. The tests run
the configured fetch/checkout against temporary local Git repositories,
including a commit reachable only through a pull ref, branch/tag events,
missing refs and missing commits. Drone remains responsible for live CI.
