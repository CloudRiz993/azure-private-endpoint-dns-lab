# Deployment (Portal Only) — Private Endpoint + Private DNS (Storage Blob)

## 1) Resource Group
Portal → Resource groups → Create
- Name: rg-pe-dns-eastus
- Region: East US

Screenshot:
- 01-rg-overview-eastus.png

## 2) Virtual Network + Subnets
Portal → Virtual networks → Create
- Name: vnet-pe-dns-eastus
- Address space: 10.20.0.0/16
- Subnets:
  - snet-vm = 10.20.1.0/24
  - snet-pe = 10.20.2.0/24

Screenshots:
- 02-vnet-overview.png
- 03-vnet-subnets-list.png

## 3) Disable Private Endpoint Network Policies (Required)
Portal → vnet-pe-dns-eastus → Subnets → snet-pe
- Private endpoint network policies: Disabled
- Save

Screenshot:
- 04-snet-pe-private-endpoint-policies-disabled.png

## 4) Test VM (No Public IP)
Portal → Virtual machines → Create
- Name: vm-pe-dns-01
- Region: East US
- VNet/Subnet: vnet-pe-dns-eastus / snet-vm
- Public IP: None
- Resulting VM private IP (observed): 10.20.1.4

Screenshots:
- 05-vm-overview-no-public-ip.png
- 06-vm-networking-no-public-ip.png

## 5) Storage Account
Portal → Storage accounts → Create
- Name: stpednseastus001
- Region: East US
- Standard / LRS

Screenshot:
- 07-storage-overview.png

## 6) Private Endpoint for Storage Blob + Private DNS Integration
Portal → Storage account (stpednseastus001) → Networking → Private endpoint connections → + Private endpoint

Basics:
- Name: pe-st-blob-01

Resource:
- Target sub-resource: blob

Virtual network:
- VNet: vnet-pe-dns-eastus
- Subnet: snet-pe

DNS:
- Integrate with private DNS zone: Yes
- Create or use: privatelink.blob.core.windows.net
- Ensure VNet link to: vnet-pe-dns-eastus

Screenshots:
- 08-storage-private-endpoint-connection-approved.png
- (optional) 09-private-endpoint-overview.png

