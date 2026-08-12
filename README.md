# sync-repo

GitHub Actions workflow for syncing every public repository in the `zitzhen`
GitHub organization to same-name repositories on Gitee and GitLab once per hour.

## Required setup

Create the same repositories under these organizations or groups before the
workflow runs:

- GitHub source organization: `zitzhen`
- Gitee target organization: `zitzhen`
- GitLab target group: `zitzhen`

Configure these repository secrets in this repository:

- `GITEE_USERNAME`: Gitee username used for HTTPS pushes
- `GITEE_TOKEN`: Gitee personal access token or password/token with push access
- `GITLAB_USERNAME`: GitLab username used for HTTPS pushes
- `GITLAB_TOKEN`: GitLab personal access token with push access

Optional configuration:

- Repository secret `GH_PUBLIC_REPO_TOKEN`: GitHub token used to list source
  repositories. If omitted, the workflow uses the built-in `GITHUB_TOKEN`.
- Repository variable `GITLAB_HOST`: GitLab host, defaults to `gitlab.com`.

## Schedule

The workflow runs hourly via cron and can also be started manually from the
GitHub Actions page.

## Notes

- The workflow syncs branches and tags only. GitHub pull request refs such as
  `refs/pull/*` are intentionally not pushed because target platforms may reject
  hidden refs.
- Archived GitHub repositories are skipped.
- This repository is skipped by default through `SKIP_REPOS: sync-repo` in
  `.github/workflows/sync-public-repos.yml`.
- Target repositories must already exist. If a target repository is missing,
  that repository sync will fail so the error is visible in Actions.
