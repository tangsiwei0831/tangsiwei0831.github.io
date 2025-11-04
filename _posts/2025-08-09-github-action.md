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


# Note
When your trigger is `workflow_run` and the workflow is in your feature branch, it will not be working because this event will only trigger a workflow run if the workflow file exists on the default branch!!!

