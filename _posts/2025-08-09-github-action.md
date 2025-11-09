---
title: 'Github Action'
date: 2025-08-09
permalink: /posts/gha
tags:
  - Software
  - Devops
---

# Trigger
If a workflow trigger is like that in A repo
```
on:
  repository_dispatch:
    types: [coverage-upload]
  workflow_dispatch:
    inputs:
        coverage_artifiact_url:
          description:
          required:
          type
        repo_token:
        ...
```

In order to trigger it in B repo, first you need to set up Personal Access Token in Github and set it as github action secret `WORKFLOW_PAT` for repo B,then your account needs to be added as an member of A repo.
Then in your repo workflow, you can trigger like below
```
run: |
    payload=$(cat <<EOF
    {
        "event_type": "coverage_upload",
        "client_payload": {
            "coverage_artifact_url": $(jq -n --arg url "${{steps.artifact.outputs.url}}" 'url'),
            "repo_token": $(jq -n --arg url "${{secrets.WORKFLOW_PAT}}" 'token')
        }
    }
    EOF
    )

    RESPONSE= $(curl -s -w "\n%{http_code}" -X POST \
      -H "Authorization: token ${{secrets.WORKFLOW_PAT}}" \
      -H "Accept: application/vnd.github.v3+json" \
      -H "Content-Type: application/json" \
      "${{github.api_url}}/repos/telus/tech-health-dashboard/dispatches" \
      -d "$payload")

      HTTP_STATUS = $(echo "RESPONSE" | tail -n1)
      BODY = $(echo "RESPONSE" | sed '$d')

    if [["$HTTP_STATUS" == "2"*]]; then
        echo "Success"
    else
        echo "Failure"
```


# Matrix
A matrix is a way to tell GitHub Actions:Run this same job multiple times — but with different parameters (like OS, language version, or dependencies).

If you want to test your app on multiple versions of Python, you might write:
```
jobs:
  test_py38:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: 3.8
      - run: pytest

  test_py39:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: 3.9
      - run: pytest
```
You can replace that with one job using a matrix:
```
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: [3.8, 3.9, 3.10]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - run: pytest
```
The matrix field defines combinations of variables. GitHub Actions expands them into all possible combinations(Cartesian product).
```
matrix:
  os: [ubuntu-latest, windows-latest]
  node: [16, 18]
```
# Note
1. When your trigger is `workflow_run` and the workflow is in your feature branch, it will not be working because this event will only trigger a workflow run if the workflow file exists on the default branch!!!
2. The reusable workflow from other repos can only be used at top level of the Github action, which is job level, you cannot set it in step level.
3. The best way to pass the file between jobs would be artifact url. However, in order to download file from artifact link, you need to use curl command and token to do that, like blow
    ```
    curl -L -H "Authorization: Bearer ${Token} {ARTIFACT_URL}" -o "<path>"
    ```

