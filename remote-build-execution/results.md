---
order: 4
---

# Building Android with Remote Build Execution (RBE)

## Results

These results demonstrate the potential build time improvements with RBE.

### System 1: 16GB RAM, 8 Cores

*   **Specs:** AMD EPYC 9634 8-Core, 16GB RAM, KVM Server

    ![specs](https://github.com/user-attachments/assets/4cb45f91-d131-4c9f-ac59-b43dda3b2629)
    *System 1 Specifications (fastfetch output): AMD EPYC 9634 8-Core CPU, 16GB RAM, KVM Server, Debian GNU/Linux 12 (bookworm).*

*   **Build Time (R8, D8, CXX, JAVAC remote build and caching):**

    ![results1](https://github.com/user-attachments/assets/b42aec9f-89c8-4f87-b7bc-3e372713eab8)
    *Image showing build time of approximately 2 hour 30 minutes with R8, D8, CXX, and JAVAC using remote build and caching.*

*   **Build Time (Everything cached except metalava):**

    ![result2](https://github.com/user-attachments/assets/4a65667b-876d-4d28-b3ba-f3611414bda8)
    *System 1 Build Time (everything): 1 hour 27 minutes 43.496 seconds. RBE Stats: down 33.48 GB, up 14.86 GB, 90889 cache hits, 4289 remote executions, 121 local fallbacks.*

### System 2: 8GB RAM, 4 Cores

*   **Specs:** Intel Core i5-6500T, 8GB RAM, 48GB ZRAM, BTRFS, CachyOS

    ![specs2](https://github.com/user-attachments/assets/413a8def-2fe5-4d49-991b-0422fdbce007)
    *System 2 Specifications (fastfetch output): Intel Core i5-6500T CPU, 8GB RAM, 48GB ZRAM, BTRFS filesystem.*

*   **Build Time (R8, D8, CXX, JAVAC remote build and caching):** > 5 hours
*   **Build Time (Everything cached):** ~ 4 hours 8 minutes

### Analyzing Build Results

After the build completes, you can examine `out/soong/rbe_metrics.txt` to get detailed information about RBE usage. This file contains data about:

*   What was retrieved from the cache.
*   How long remote execution or local execution took for different build actions.
*   Which tools (R8, D8, etc.) were executed remotely.
*   Which tools failed remotely and fell back to local execution.
