# GitHub App Inventory

GitHub App Inventory automatically audits GitHub App installations across your GitHub organization and generates a downloadable CSV report. It provides visibility into installed integrations for operational governance, security review, and compliance reporting.

This project is available as:

- ✅ A GitHub Action (Marketplace)
- 🐍 A standalone Python script (optional offline usage)

---

## 🚀 GitHub Action (Marketplace)

Run installation audits directly from GitHub Actions—no local setup required.


## How It Works

- Retrieves all installed GitHub Apps for the specified organization
- Collects installation metadata including:
    * App name & slug
    * Installation type (organization or user)
    * Target identifiers
    * Creation & update timestamps
    * Installation URLs
- Generates a structured CSV report
- Uploads it as a workflow artifact for download

##  Usage

```yml
- name: GitHub App Inventory
  uses: org-name/github-app-inventory@v1.0.0
  with:
    github_token: ${{ secrets.ORG_AUDIT_TOKEN }}
    org_name: acme


##  Upload Inventory Report
- uses: actions/upload-artifact@v4
  with:
    name: github-app-inventory-report
    path: "*.csv"

```

> ⚠️ **Security Requirement:**
> 
> This action requires an authentication token with appropriate permissions.
> 
> You must define the token as a repository secret (for example, ORG_AUDIT_TOKEN) and explicitly provide it to the action as shown below:
>
> ```yml
> github_token: ${{ secrets.ORG_AUDIT_TOKEN }}
> ```


### Example workflow

```yml
name: GitHub App Inventory Audit

on:
  schedule:
    - cron: "0 0 1 * *"
  workflow_dispatch:

jobs:
  audit:
    runs-on: ubuntu-latest
    steps:
      - name: GitHub App Inventory
        uses: org-name/github-app-inventory@v1.0.0
        with:
          github_token: ${{ secrets.ORG_AUDIT_TOKEN }}
          org_name: acme

      - name: Upload Report
        uses: actions/upload-artifact@v4
        with:
          name: github-app-inventory-report
          path: "*.csv"

```


## 🔐 Required Token Permissions

The token used with this action must have the following scopes:

- `read:org – to list organization members`
- `repo - to analyze repository activity`
  


## 🔧 GitHub Action Inputs

| Name              | Required | Default           | Description                |
| ----------------- | -------- | ----------------- | -------------------------- |
| `github_token`    | Yes      | –                 | Token with read:org access |
| `org_name`        | Yes      | –                 | GitHub organization name   |
| `output_filename` | No       | `github_apps.csv` | Output CSV file name       |


## Sample Output

| App Name   | App ID | Target Type  |
| ---------- | ------ | ------------ |
| dependabot | 32542  | organization |
| sentry     | 11221  | organization |
| codecov    | 22198  | user         |










  

