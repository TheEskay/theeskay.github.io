---
title: "Create an Issue - GitHub REST API Documentation"
date: 2026-05-22
summary: "Learn how to create GitHub issues programmatically using the GitHub REST API, including authentication requirements, request parameters, example responses, and common error handling scenarios."
showToc: true
TocOpen: false
---

# Create an Issue — GitHub REST API

*Sample API documentation, written independently as a self-directed practice exercise, based on GitHub's public REST API.*

## Overview

The **Create an Issue** endpoint lets you programmatically open a new issue on a specified repository. This is commonly used to automate bug reports, feature requests, or task tracking from external tools (e.g., CI pipelines, monitoring alerts, or internal ticketing systems).

---

## Endpoint

```
POST /repos/{owner}/{repo}/issues
```

| Parameter | Type   | Location | Required | Description                                  |
|-----------|--------|----------|----------|-----------------------------------------------|
| `owner`   | string | path     | Yes      | The account owner of the repository (case-insensitive). |
| `repo`    | string | path     | Yes      | The name of the repository (case-insensitive). |

---

## Authentication

This endpoint requires a **personal access token (classic)** or **fine-grained token** with `issues:write` permission, passed as a bearer token in the request header:

```
Authorization: Bearer YOUR_TOKEN
```

> **Note:** Fine-grained tokens must be scoped to the specific repository (or organization) where the issue will be created. Tokens without the correct scope will return a `403 Forbidden` response, not a `401` — this can be a common point of confusion during debugging.

---

## Request Body

| Field       | Type            | Required | Description                                       |
|-------------|-----------------|----------|-----------------------------------------------------|
| `title`     | string          | Yes      | The title of the issue.                            |
| `body`      | string          | No       | The contents of the issue (Markdown supported).     |
| `assignees` | array of string | No       | Usernames to assign to the issue.                   |
| `labels`    | array of string | No       | Labels to apply to the issue.                       |

### Example Request

```bash
curl -X POST \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Accept: application/vnd.github+json" \
  https://api.github.com/repos/octocat/hello-world/issues \
  -d '{
    "title": "Login page throws 500 error on submit",
    "body": "Steps to reproduce:\n1. Go to /login\n2. Submit valid credentials\n3. Observe 500 error",
    "labels": ["bug", "priority-high"]
  }'
```

### Example Response — `201 Created`

```json
{
  "id": 1,
  "number": 42,
  "title": "Login page throws 500 error on submit",
  "state": "open",
  "html_url": "https://github.com/octocat/hello-world/issues/42",
  "labels": [
    { "name": "bug" },
    { "name": "priority-high" }
  ],
  "created_at": "2026-07-21T10:15:00Z"
}
```

---

## Error Responses

| Status Code | Meaning              | Common Cause                                      |
|-------------|----------------------|-----------------------------------------------------|
| `401`       | Unauthorized         | Missing or invalid token.                          |
| `403`       | Forbidden            | Token valid but lacks `issues:write` scope for this repo. |
| `404`       | Not Found            | Repository doesn't exist, or token lacks read access to it. |
| `422`       | Validation Failed    | Missing required field (e.g., `title`) or invalid label/assignee name. |

---

## Notes

- Issues **cannot** be created on repositories with issues disabled in their settings — this returns a `410 Gone`, which is easy to misdiagnose as a permissions issue if you're not aware of it upfront.
- Rate limits apply per authenticated user (5,000 requests/hour for most token types). For high-volume automation (e.g., syncing issues from an external tracker), batch requests or use conditional requests to stay within limits.