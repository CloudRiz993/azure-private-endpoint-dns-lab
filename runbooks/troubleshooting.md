# Troubleshooting Runbook — Private Endpoint + Private DNS (Storage Blob)

## Scope
This runbook applies to:
- Storage account: stpednseastus001
- Endpoint tested: stpednseastus001.blob.core.windows.net
- Private endpoint subnet: 10.20.2.0/24 (expected PE IP: 10.20.2.5)
- VM subnet: 10.20.1.0/24 (VM: 10.20.1.4)

Primary validation is done from inside the VM using **Run command** (no public RDP required).

---

## Symptoms (what users report)
1) “Private endpoint is created but the app can’t reach storage”
2) “DNS still resolves to public IP”
3) “Name doesn’t resolve / NXDOMAIN”
4) “TCP 443 fails even though DNS looks correct”
5) “Everything worked yesterday, now it doesn’t”

---

## Fast triage: 60-second checklist (do in this order)
### 1) Confirm the hostname is correct (most common mistake)
Correct format:
- `<storageaccount>.blob.core.windows.net`

If you test placeholders or a wrong name, you will see **NXDOMAIN**.

### 2) Confirm Private Endpoint connection state
Portal → Storage account → **Networking** → **Private endpoint connections**
- Must show: **Approved** and **Succeeded**

### 3) Confirm DNS zone is correct for Blob
Correct private DNS zone:
- `privatelink.blob.core.windows.net`

### 4) Confirm VNet is linked to the Private DNS zone
Portal → Private DNS zones → `privatelink.blob.core.windows.net` → **Virtual network links**
- Must show link to: `vnet-pe-dns-eastus`

### 5) Confirm A record exists for the storage account
Portal → Private DNS zones → `privatelink.blob.core.windows.net` → **Record sets**
- A record for `stpednseastus001` must point to **10.20.2.5** (or another 10.20.2.x PE IP)

### 6) Confirm PE subnet setting (required)
Portal → VNet → Subnets → `snet-pe`
- **Private endpoint network policies: Disabled**

### 7) Re-run validation inside VM (authoritative test)
Portal → VM → Run command → RunPowerShellScript:

```powershell
$sa   = "stpednseastus001"
$fqdn = "$sa.blob.core.windows.net"
nslookup $fqdn
Test-NetConnection $fqdn -Port 443

