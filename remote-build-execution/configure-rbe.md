---
order: 3
---

# Building Android with Remote Build Execution (RBE)

### 3. Set Environment Variables

These environment variables configure `reclient` to use RBE. It's highly recommended to add these to your `build/envsetup.sh` file so they are automatically set each time you initialize your build environment.

**IMPORTANT SEE NOTES BEFORE YOU DO THIS**

```bash
# --- Enable RBE and General Settings ---
export USE_RBE=1
export RBE_DIR="path/to/reclient"                      # Path to the extracted reclient directory (relative or absolute)
export NINJA_REMOTE_NUM_JOBS=72                        # Number of parallel remote jobs (adjust based on your RAM, AOSP default is 500)

# --- Unified Downloads/Uploads (Recommended) ---
export RBE_use_unified_downloads=true
export RBE_use_unified_uploads=true

# --- Execution Strategies (remote_local_fallback is generally best) ---
export RBE_R8_EXEC_STRATEGY=remote_local_fallback
export RBE_D8_EXEC_STRATEGY=remote_local_fallback
export RBE_JAVAC_EXEC_STRATEGY=remote_local_fallback
export RBE_JAR_EXEC_STRATEGY=remote_local_fallback
export RBE_ZIP_EXEC_STRATEGY=remote_local_fallback
export RBE_TURBINE_EXEC_STRATEGY=remote_local_fallback
export RBE_SIGNAPK_EXEC_STRATEGY=remote_local_fallback
export RBE_CXX_EXEC_STRATEGY=remote_local_fallback    # Important see below.
export RBE_CXX_LINKS_EXEC_STRATEGY=remote_local_fallback
export RBE_ABI_LINKER_EXEC_STRATEGY=remote_local_fallback
export RBE_ABI_DUMPER_EXEC_STRATEGY=    # Will make build slower, by a lot. Keeping this for documentation
export RBE_CLANG_TIDY_EXEC_STRATEGY=remote_local_fallback
export RBE_METALAVA_EXEC_STRATEGY=remote_local_fallback
export RBE_LINT_EXEC_STRATEGY=remote_local_fallback

# --- Enable RBE for Specific Tools ---
export RBE_R8=1
export RBE_D8=1
export RBE_JAVAC=1
export RBE_JAR=1
export RBE_ZIP=1
export RBE_TURBINE=1
export RBE_SIGNAPK=1
export RBE_CXX_LINKS=1
export RBE_CXX=1
export RBE_ABI_LINKER=1
export RBE_ABI_DUMPER=    # Will make build slower, by a lot. Keeping this for documentation
export RBE_CLANG_TIDY=1
export RBE_METALAVA=1
export RBE_LINT=1

# --- Resource Pools ---
export RBE_JAVA_POOL=default
export RBE_METALAVA_POOL=default
export RBE_LINT_POOL=default
```

*   **`USE_RBE=1`**: Enables RBE.
*   **`RBE_DIR`**: The path to your extracted `reclient` directory.
*   **`*_EXEC_STRATEGY`**: Controls how different build steps are handled. `remote_local_fallback` means try remotely first, then fall back to local execution if the remote execution fails.
*   **`RBE_*=1`**: Enables RBE for specific build tools.
*   **`NINJA_REMOTE_NUM_JOBS`**: The number of parallel jobs to run remotely. Start with 72 and increase if you have more RAM. 128 should be safe for 16GB RAM systems. You can go higher (e.g., 500) if you have significantly more RAM.
*   **`RBE_*_POOL`**: Specifies the resource pool to use. The `default` pool is usually sufficient.

**Important Notes:**

*   Make sure to switch **`RBE_CXX_LINKS_EXEC_STRATEGY`** to **`local`** after your first build is done to reduce build times. If rebuilding C/C++ targets is needed change to **`remote_local_fallaback`**
*   Many of these options are not officially documented by Google and were discovered through AOSP source code analysis.
