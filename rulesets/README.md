# Protected branch policy

The `Reservine/Reservine` repository uses two organization-owned rulesets. The
JSON files beside this document are the reviewable source of truth used to
create them through the GitHub API.

## Access model

| Action | Feature branches | `main` | `release` |
| --- | --- | --- | --- |
| Push commits | Members with repository write access | Pull request only | Pull request only |
| Merge a pull request | Members with repository write access after all rules pass | Same | Same |
| Bypass approval/check rules | Not applicable | Organization owners, from a pull request only | Organization owners, from a pull request only |
| Force-push | Allowed unless another rule prevents it | Blocked | Blocked |
| Delete branch | Allowed unless another rule prevents it | Blocked | Blocked |

Both protected branches require:

- one approving review, with approvals dismissed when reviewable commits change;
- every review conversation to be resolved;
- the `Cross-repo PR dependencies` check from GitHub Actions;
- a pull request, including for organization owners.

Owners may bypass approval or check requirements while merging a pull request.
They cannot bypass the pull-request path with a direct push. An actual emergency
requires an owner to explicitly change the organization ruleset, leaving an
auditable settings change.

## Branch intent

- `main` is the development and dev-deployment branch.
- `release` is the production release branch. Production hotfixes may target it
  directly through a pull request; it is not assumed to be a mirror of `main`.

The full lint and E2E workflows are intentionally not required by these
rulesets yet because they are not consistently green. Add them only after their
reliability is high enough that branch protection cannot deadlock delivery.
