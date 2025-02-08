---
order: 0
---

# Building Android with Remote Build Execution (RBE)

This documentation outlines how to configure and use Remote Build Execution (RBE) to accelerate Android builds. This includes utilizing remote caching with `reclient`, and optionally, remote execution. While BuildBuddy is featured in this guide, other remote executors such as Buildfarm, Buildbarn, and Nativelinks are also compatible with RBE and AOSP. Future iterations of this documentation will include specific configuration guides for these alternative executors, as well as detailed instructions on configuring and using remote caching independently.

## Contents:

1.  [Configure `reclient`](configure-reclient.md): Download and extract the `reclient` package.
2.  [Configure BuildBuddy (Optional)](configure-buildbuddy.md): Integrate with BuildBuddy for enhanced RBE.
3.  [Configure RBE](configure-rbe.md): Set environment variables to enable and configure RBE.
4.  [Build Normally](../building-101/building.md): Build AOSP as you normally do.
5.  [Results](results.md): View performance benchmarks.
6.  [Contact](contact.md): Get in touch for support.
