---
order: 0
---

# Build Tips and Tricks

This section provides various tips and tricks specifically related to building AOSP (Android Open Source Project). These guides are designed to help you overcome common build challenges and optimize your build process.

## Available Build Tips

- **[Reducing Storage Requirements with BTRFS Compression](./reducing-storage-needs.md)**: Learn how to use BTRFS compression to dramatically reduce the storage space needed for AOSP builds, potentially reducing storage usage by around 50%.

- **[Working on Low-RAM Machines with zram](./reducing-ram-needs.md)**: AOSP builds typically require 32GB+ of RAM. This guide shows you how to use `zram` (compressed RAM) to build on systems with less physical RAM (e.g., 8GB).

These tips can help you build AOSP more efficiently, especially if you're working with limited hardware resources.
