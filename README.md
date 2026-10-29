# evolve-platform/.github

The organization's profile (`profile/README.md`) and the reusable workflows the
evolve-platform repositories call.

## `jira-on-merge`

Follows a pull request on the Jira issue whose key is in its head branch name,
such as `feat/EV-123-short-description`. It runs the
[Atlassian CLI](https://developer.atlassian.com/cloud/acli/) at a pinned version
and checks the download against a pinned SHA-256.

| Pull request                            | Issue                                                    |
| --------------------------------------- | -------------------------------------------------------- |
| opened or reopened                      | Comment with the URL; Nieuw → Actief, and its parent too |
| merged                                  | Comment with the URL; → Gereed                           |
| merged, labelled `partial`              | Comment with the URL only; the status stays              |
| merged, last open subtask of its parent | The parent → Gereed as well                              |
| closed without a merge                  | Nothing                                                  |

Only pull requests into the repository's default branch count, and a branch
without a key is skipped. Every step is a no-op when it was already done, so a
rerun neither duplicates the comment nor moves an issue out of a done status
(Gereed or WONTFIX).

### Calling it

```yaml
name: jira-on-merge

on:
  pull_request:
    types: [opened, closed]
    branches: [main]

permissions: {}

jobs:
  jira:
    uses: evolve-platform/.github/.github/workflows/jira-on-merge.yaml@<sha> # main
    secrets:
      JIRA_EMAIL: ${{ secrets.JIRA_EMAIL }}
      JIRA_API_TOKEN: ${{ secrets.JIRA_API_TOKEN }}
```

Leave `opened` out of `types` to only act on merges. Inputs: `key-pattern`
(default `EV-[0-9]+`) and `jira-site` (default `labdigital.atlassian.net`).

### Secrets

| Org secret       | What                                                    |
| ---------------- | ------------------------------------------------------- |
| `JIRA_EMAIL`     | Email address of the Jira bot user the workflow acts as |
| `JIRA_API_TOKEN` | A classic (unscoped) API token of that user             |

The user needs Browse, Add Comments and Transition Issues on the projects the
keys point at. It has to be a regular user account rather than an Atlassian
service account: `acli jira auth login` does not accept a service account's
scoped token yet ([CLOUD-12991](https://jira.atlassian.com/browse/CLOUD-12991)).

Both secrets have to be shared with every calling repository. An empty one
fails the job with an error naming them. The old `JIRA_MERGE_WEBHOOK_URL` and
`JIRA_MERGE_WEBHOOK_TOKEN` are still accepted and ignored, so a caller that
passes them keeps starting until it switches.

### Testing it

Run the workflow from the Actions tab of this repository (`workflow_dispatch`)
with a test issue's branch name, a pull request URL, the event and the
`partial` flag. The two secrets have to be shared with this repository for
that.

### Updating the CLI

Set `ACLI_VERSION` and `ACLI_SHA256` in the workflow together:

```bash
v=1.3.39-stable
curl -fsSL "https://acli.atlassian.com/linux/$v/acli_${v}_linux_amd64.tar.gz" | shasum -a 256
```
