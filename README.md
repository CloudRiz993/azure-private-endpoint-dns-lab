# Azure Private Endpoint + Private DNS Lab (Storage Blob) — Portal Only

## Goal
Deploy an Azure Storage Account (Blob) with **Private Endpoint** + **Private DNS** so the blob endpoint resolves to a **private IP** and is reachable over HTTPS **without public network access**.

## Why this lab matters (Cloud Support / Cloud Ops)
This covers one of the most common enterprise incidents:
- Private Endpoint exists but **DNS resolution is wrong**
- Clients resolve to public endpoint or fail entirely
- Connectivity tests fail because name resolution / VNet links / zone group are misconfigured

## Environment (Lab Values)
- Region: East US
- Resource Group: rg-pe-dns-eastus
- VNet: vnet-pe-dns-eastus (10.20.0.0/16)
  - snet-vm: 10.20.1.0/24 (VM = 10.20.1.4)
  - snet-pe: 10.20.2.0/24 (Private Endpoint IP = 10.20.2.5)
- Storage account: stpednseastus001
- Private DNS zone: privatelink.blob.core.windows.net
- Public network access: Disabled (private-only posture)

## Evidence (Screenshots)
See: `screenshots/README.md`

Key proof points:
- PE connection Approved/Succeeded
- Private DNS zone linked to VNet
- A record resolves to 10.20.2.5
- VM Run Command shows DNS private resolution + TCP 443 success
- Storage public network access disabled

## Docs
- Deployment: `docs/deployment.md`
- Validation: `docs/validation.md`
- Troubleshooting Runbook: `runbooks/troubleshooting.md`
- Teardown: `teardown/teardown.md`

