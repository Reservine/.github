# Protected branch policy

Reservine uses organization-owned rulesets for the development and production
branches in both application repositories. The JSON files beside this document
are the reviewable source of truth applied through the GitHub API.

## Branch intent

| Repository | Development | Production |
| --- | --- | --- |
| `Reservine/Reservine` | `main` | `release` |
| `Reservine/ReservineBack` | `dev` | `master` |

## Access model

Every protected branch:

- accepts changes through pull requests only;
- requires every review conversation to be resolved;
- currently requires zero approving reviews;
- blocks force-pushes and deletion;
- gives organization owners a pull-request-only bypass for recovery.

The frontend `main` and `release` branches additionally require the
`Cross-repo PR dependencies` GitHub Actions check. Backend checks are not yet
required because the current Laravel quality gate can remain cancelled and
would deadlock merges.

Feature branches remain directly writable by members with repository write
access. Owners cannot bypass the pull-request path with a direct push. An
emergency direct push requires explicitly changing the organization ruleset,
leaving an auditable settings change.

## Eve review policy

The installed GitHub App is `eve-bot-lovinka` (`eve-bot-lovinka[bot]` when it
reviews). GitHub's specific required-reviewer rule targets teams, not Apps, and
Eve currently submits advisory `COMMENTED` reviews on ReservineBack rather than
an approvable status check. Requiring an approval now would therefore create an
unsatisfiable merge gate.

Keep Eve advisory until it emits a dedicated successful check run for backend
pull requests. At that point, add that App-owned context as a required status
check to the backend rulesets; this pins the gate to Eve without adding a human
review requirement.
