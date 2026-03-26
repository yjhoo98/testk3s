# Exported Dashboards

Put dashboard JSON files exported from your local Grafana environment here.

## Suggested Usage

- Export the dashboard JSON from local Grafana
- Save it in this folder
- Copy the draft into `../` when you want a deployment-ready version
- Review and adjust datasource references
- Replace local image URLs with S3 object URLs after the Terraform S3 work is finished
- Keep one JSON file per dashboard

## Common Follow-up Changes

- Datasource UID
- PostgreSQL connection target
- SQL query text
- Panel links
- Text panel image URLs
- Public or signed S3 image paths
