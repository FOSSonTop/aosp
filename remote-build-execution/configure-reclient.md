---
order: 1
---

# Setup RBE

This replaces the old manual `reclient` download steps. It uses a patched `reclient` bundle.

## 1. Apply the Fix

Run these commands from the **root of your Android source tree** (where you normally run `source build/envsetup.sh`):

0. **Clone the fix repository:**
   ```bash
   git clone https://github.com/Rares6567/new_rbe_fix tutorial
   ```

1. **Build the patched reclient bundle:**
   ```bash
   bash tutorial/scripts/build_patched_reclient.sh .
   ```
   *Note the path of the output folder printed by the script (usually `tutorial/output` or similar). You will need it for `RBE_DIR`.*

2. **Patch the build system defaults:**
   ```bash
   patch -p1 < tutorial/patches/android-rbe-buildbuddyfix-defaults.patch
   ```

## 2. Next Steps

After applying this fix:

1. Set `RBE_DIR` to the **output folder path** produced by `build_patched_reclient.sh` (see Step 1 above).
2. Proceed to [Configure BuildBuddy](configure-buildbuddy.md) and then [Configure Environment Variables](configure-rbe.md) to finish enabling RBE.
