# Tips and Tricks

## 1. Reducing Storage Requirements with BTRFS Compression

BTRFS compression can dramatically reduce the storage space needed for AOSP builds.

1.  **Create a BTRFS Filesystem:**

```bash
sudo mkfs.btrfs /dev/your_storage_partition  # Replace with your actual partition
```

2.  **Mount with Compression:**

```bash
sudo mount /dev/your_storage_partition /mnt/aosp -o defaults,noatime,compress-force=zstd,space_cache=v2,commit=120
```
Replace `/mnt/aosp` with your desired mount point.

3.  **fstab Entry (Optional):**  For persistent mounting, add a line to your `/etc/fstab`:

```
/dev/your_storage_partition /mnt/aosp btrfs defaults,noatime,compress-force=zstd,space_cache=v2,commit=120 0 0
```

**Benefits:** This can reduce storage usage by around 50% (e.g., a 195GB build might only take up 101GB).

**Example Compression Statistics:**
```
Processed 2056218 files, 2145323 regular extents (2147448 refs), 1192781 inline.
Type       Perc     Disk Usage   Uncompressed Referenced
TOTAL       55%      130G         235G         236G
none       100%       69G          69G          70G
zstd        36%       60G         165G         165G
```
**Note:** You *might* be able to achieve even greater compression with `dwarfs`, but this is more complex to set up.
