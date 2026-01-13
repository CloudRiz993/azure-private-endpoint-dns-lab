# Validation (Portal Only)

## A) Private Endpoint Connection Health
Portal → Storage account → Networking → Private endpoint connections
Expected:
- Connection state/status: Approved
- Provisioning: Succeeded

Screenshot:
- 08-storage-private-endpoint-connection-approved.png

## B) Private DNS Zone VNet Link
Portal → Private DNS zones → privatelink.blob.core.windows.net → Virtual network links
Expected:
- vnet-pe-dns-eastus is linked/completed

Screenshot:
- 10a-private-dns-vnet-link.png

## C) Private DNS A Record Exists
Portal → Private DNS zones → privatelink.blob.core.windows.net → Record sets
Expected:
- A record for stpednseastus001 points to 10.20.2.5

Screenshot:
- 10b-private-dns-a-record-storage.png

## D) Validate From Inside VM (Run Command)
Portal → VM (vm-pe-dns-01) → Run command → RunPowerShellScript

Run:
```powershell
$sa   = "stpednseastus001"
$fqdn = "$sa.blob.core.windows.net"
nslookup $fqdn
Test-NetConnection $fqdn -Port 443

