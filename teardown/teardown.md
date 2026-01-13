# Teardown — Portal Only (Clean and Verifiable)

## Goal
Remove all lab resources to stop consumption/billing and avoid orphaned assets.

Preferred method: **delete the Resource Group**.

---

## Option 1 (Recommended): Delete the Resource Group

### Steps
1) Portal → **Resource groups**
2) Open: `rg-pe-dns-eastus`
3) Click **Delete resource group**
4) Type the RG name exactly to confirm: `rg-pe-dns-eastus`
5) Click **Delete**

### Post-delete verification (do not skip)
After deletion completes:
1) Portal global search → search for each of these:
   - `stpednseastus001`
   - `pe-st-blob-01`
   - `privatelink.blob.core.windows.net`
   - `vnet-pe-dns-eastus`
   - `vm-pe-dns-01`
2) Confirm none of them still exist.
3) Portal → Resource groups → confirm `rg-pe-dns-eastus` is gone.

---

## Option 2: If RG deletion fails (common causes + fixes)

### A) Resource locks
**Symptom:** Delete fails with “scope is locked” / “delete not permitted”.

**Fix:**
1) Resource group → **Locks**
2) Remove any **Delete** / **ReadOnly** locks
3) Retry RG delete

### B) Stuck resources
If RG deletion stalls, do manual cleanup (Option 3) and then retry RG delete.

---

## Option 3: Manual cleanup (only if required)

Delete in this order to reduce dependency errors:

1) **VM**
   - Portal → Virtual machines → `vm-pe-dns-01` → **Delete**
   - Ensure you also remove: OS disk, NIC, (and public IP if any exists)

2) **Private Endpoint**
   - Search “Private endpoints” → `pe-st-blob-01` → **Delete**

3) **Storage account**
   - Storage accounts → `stpednseastus001` → **Delete**

4) **Private DNS zone**
   - Private DNS zones → `privatelink.blob.core.windows.net` → **Delete**

5) **VNet**
   - Virtual networks → `vnet-pe-dns-eastus` → **Delete**

6) Finally delete the **Resource Group**
   - Resource groups → `rg-pe-dns-eastus` → **Delete**

### Manual cleanup verification
Before deleting the RG, open it and confirm the resource list is empty or only contains items you are deleting.

---

## Cost hygiene (important for trial subscriptions)
- When pausing work, stop the VM until it shows **Stopped (deallocated)** (not just “Stopped”).
- After capturing screenshots and committing docs, delete the RG to avoid ongoing charges.

---

## Optional evidence (portfolio)
If you want to prove cleanup discipline, capture the RG delete dialog:
- Filename suggestion: `99-teardown-rg-delete.png`
(Do not capture subscription/tenant IDs.)

