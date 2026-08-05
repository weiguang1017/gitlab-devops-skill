---
name: gitlab-skills
description: Manage GitLab projects from the command line — list/inspect projects, list and create issues, list/create/merge merge requests, and list or trigger CI/CD pipelines. Use this whenever the user wants to work with a GitLab repo, file an issue, open or merge an MR, or kick off / check a pipeline on GitLab.com or a self-hosted GitLab.
license: MIT
---

# GitLab Skills

Operate GitLab through its REST API v4 with one self-contained Python CLI
(`scripts/gitlab_cli.py`). Works with GitLab SaaS and self-hosted instances.

## When to use

Trigger this skill when the user asks to:
- find or inspect a project ("show my projects", "details of group/app")
- list or create issues
- list, create, or merge merge requests
- list or trigger CI/CD pipelines ("run pipeline on main", "is the pipeline green")

## Setup (once)

Reads connection info from environment variables OR `~/.devops-skills/gitlab.json`.

```bash
export GITLAB_URL="https://gitlab.com"   # or your self-hosted URL
export GITLAB_TOKEN="<access-token>"      # scope: api
```

Create a token at: User Settings → Access Tokens (scope `api`).
Requires Python 3.8+ and `requests`. Full guide: `使用手册.md`.

## Compatibility

Supported targets:
- GitLab.com
- Self-managed GitLab 9.0+ with REST API v4

The CLI checks `/api/v4/version` before running commands. If the detected
self-managed GitLab version is below 9.0, it exits with a clear compatibility
message. Set `GITLAB_SKIP_VERSION_CHECK=1` or
`DEVOPS_SKILLS_SKIP_VERSION_CHECK=1` only when you deliberately need to bypass
this guard.

## Usage

Projects can be a numeric ID or a path like `group/subgroup/project` (auto-encoded).

```bash
python scripts/gitlab_cli.py list-projects --search app
python scripts/gitlab_cli.py get-project group/app
python scripts/gitlab_cli.py list-issues group/app --state opened
python scripts/gitlab_cli.py create-issue group/app --title "Bug" --description "..."
python scripts/gitlab_cli.py list-mrs group/app --state opened
python scripts/gitlab_cli.py create-mr group/app --source feat --target main --title "Add feature"
python scripts/gitlab_cli.py merge-mr group/app 42
python scripts/gitlab_cli.py list-pipelines group/app --ref main
python scripts/gitlab_cli.py trigger-pipeline group/app --ref main
```

Every command prints JSON to stdout and exits non-zero on failure.

## Notes

- `merge-mr` requires the MR to be mergeable (approvals/pipeline rules apply).
- Triggering pipelines requires the project to have a `.gitlab-ci.yml`.

## Support

For GitLab, CI/CD, DevOps platform, or engineering efficiency consulting and
troubleshooting, contact RestartX: https://service.restartx.top/
