# Screenshot Guide (Evidence) — How to Capture and Store

## Rules
- Format: PNG
- Crop to the relevant proof (avoid full-screen clutter)
- Never capture credentials (username/password)
- Blur if needed: subscription ID, tenant ID
- Store all images in this folder: `screenshots/`
- Use exact filenames below so docs link correctly

---

## How to take each screenshot (where + what to show)

### 01-rg-overview-eastus.png
Where:
- Portal → Resource groups → `rg-pe-dns-eastus` → Overview
Show:
- RG name + Region (East US)

### 02-vnet-overview.png
Where:
- Virtual networks → `vnet-pe-dns-eastus` → Overview
Show:
- VNet name + address space 10.20.0.0/16

### 03-vnet-subnets-list.png
Where:
- `vnet-pe-dns-eastus` → Subnets
Show:
- snet-vm 10.20.1.0/24 and snet-pe 10.20.2.0/24

### 04-snet-pe-private-endpoint-policies-disabled.png
Where:
- `vnet-pe-dns-eastus` → Subnets → `snet-pe`
Show:
- Private endpoint network policies = Disabled

### 05-vm-overview-no-public-ip.png
Where:
- Virtual machines → `vm-pe-dns-01` → Overview
Show:
- VM name + RG + Region
- Preferably no public IP shown

### 06-vm-networking-no-public-ip.png
Where:
- `vm-pe-dns-01` → Networking
Show:
- Public IP: None
- Subnet: snet-vm

### 07-storage-overview.png
Where:
- Storage account → `stpednseastus001` → Overview
Show:
- Storage name + Region + RG

### 08-storage-private-endpoint-connection-approved.png
Where:
- Storage account → Networking → Private endpoint connections
Show:
- Connection status Approved / Succeeded for pe-st-blob-01

### 09-private-endpoint-overview.png (optional but strong)
Where:
- Search → Private endpoints → `pe-st-blob-01` → Overview
Show:
- Connected resource is storage
- Subnet snet-pe

### 10a-private-dns-vnet-link.png
Where:
- Private DNS zones → privatelink.blob.core.windows.net → Virtual network links
Show:
- Link to vnet-pe-dns-eastus present/linked

### 10b-private-dns-a-record-storage.png
Where:
- Private DNS zones → privatelink.blob.core.windows.net → Record sets
Show:
- A record for stpednseastus001 → 10.20.2.5

### 11-vm-runcommand-nslookup-tnc-443.png
Where:
- VM → Run command → RunPowerShellScript output
Show:
- nslookup result resolves to 10.20.2.5
- Test-NetConnection shows TcpTestSucceeded : True
- (Optional) source address 10.20.1.4 visible is good

### 12-storage-public-network-access-disabled.png
Where:
- Storage account → Networking
Show:
- Public network access = Disabled

### 99-teardown-rg-delete.png (optional)
Where:
- Resource group → Delete dialog
Show:
- RG name field (no IDs)

---

## Upload steps (GitHub web)
1) Open repo → open `screenshots/` folder
2) Add file → Upload files
3) Drag-and-drop PNGs
4) Confirm filenames match exactly
5) Commit message: "Add lab screenshots"

---

## Linking screenshots in markdown (example)
In docs, embed like:
```md
![RunCommand validation](../screenshots/11-vm-runcommand-nslookup-tnc-443.png)
# Screenshot Index (Evidence)

Place all images in this folder using these names.

## Deployment Evidence
- 01-rg-overview-eastus.png
- 02-vnet-overview.png
- 03-vnet-subnets-list.png
- 04-snet-pe-private-endpoint-policies-disabled.png
- 05-vm-overview-no-public-ip.png
- 06-vm-networking-no-public-ip.png
- 07-storage-overview.png
- 08-storage-private-endpoint-connection-approved.png
- 09-private-endpoint-overview.png (optional)

## DNS Evidence
- 10a-private-dns-vnet-link.png
- 10b-private-dns-a-record-storage.png

## Validation Evidence
- 11-vm-runcommand-nslookup-tnc-443.png

## Hardening Evidence
- 12-storage-public-network-access-disabled.png

(Optional)
- 11b-validation-after-public-disabled.png

