# Grafana Provisioning Assets

This directory stores Grafana configuration and provisioning files only.

K3s manifests, Helm values, Ingress, PVC, and Service resources are expected
to be added later by the infrastructure owner. When those manifests are added,
they can mount this directory's files into the Grafana container as ConfigMaps
or volume files.

## Directory Layout

- `grafana.ini`
  - Base Grafana server configuration
- `provisioning/datasources/datasources.yaml`
  - PostgreSQL datasource auto-registration for RDS
- `provisioning/dashboards/dashboardproviders.yaml`
  - Dashboard file provider registration
- `dashboards/admin-dashboard.json`
  - Deployment-ready admin dashboard derived from the exported draft
- `dashboards/user-dashboard.json`
  - Deployment-ready user dashboard derived from the exported draft
- `dashboards/exported/`
  - Raw dashboard JSON exports from the local Grafana workspace

## Expected Container Mount Paths

- `grafana.ini` -> `/etc/grafana/grafana.ini`
- `provisioning/datasources/datasources.yaml`
  -> `/etc/grafana/provisioning/datasources/datasources.yaml`
- `provisioning/dashboards/dashboardproviders.yaml`
  -> `/etc/grafana/provisioning/dashboards/dashboardproviders.yaml`
- `dashboards/*.json`
  -> `/var/lib/grafana/dashboards/*.json`

## Assumptions

- Grafana runs inside the K3s cluster later.
- Grafana can reach the private RDS endpoint from inside the VPC.
- Admin credentials and secrets are injected by the deployment owner, not kept
  in this folder.

## Recommended Workflow

1. Build or edit dashboards in your local Grafana instance.
2. Export each dashboard as JSON.
3. Save the exported files into `dashboards/exported/`.
4. Promote the exported draft into `dashboards/` for deployment use.
5. After Terraform work for S3 and RDS is finished, update:
   - datasource host, database, username, and password handling
   - panel SQL queries
   - image URLs pointing to S3 objects or CDN paths
6. Mount the final dashboard directory into Grafana in K3s.

## Current Dashboard Set

- `admin-dashboard.json`
  - copied from the current admin draft export
  - datasource UID normalized to `rds-postgresql`
- `user-dashboard.json`
  - copied from the current user draft export
  - datasource UID normalized to `rds-postgresql`

The original exported files are kept under `dashboards/exported/` so the raw
local outputs remain available for reference.
