---
order: 2
---

# Configuration for BuildBuddy

## 1. Obtain Your BuildBuddy API Key

1.  **Sign In:** Go to [buildbuddy.io](https://buildbuddy.io/) and sign in with your Google or GitHub account.
2.  **Quickstart:** Navigate to the "Quickstart" section in your BuildBuddy dashboard.
3.  **Copy API Key:** You'll see a command similar to this:

    ```bash
    build --bes_backend=grpcs://your-instance.buildbuddy.io
    build --remote_header=x-buildbuddy-api-key=xxx ...
    ```

    *   Copy the entire string *after* `--remote_header=`. This is your API key. It will look like `x-buildbuddy-api-key=xxx`.
    *   Note the address after `--bes_backend=`. You only need the part *after* `grpcs://`, e.g., `your-instance.buildbuddy.io`.


## 2. Setup Environment variables for BuildBuddy

1. **Setup the Variables:** Add this to your `build/envsetup.sh` or export them manually:
  ```bash # --- BuildBuddy Connection Settings ---
    export RBE_service="your-instance.buildbuddy.io:443"        # BuildBuddy instance address (without grpcs://, add the port 443)
    export RBE_remote_headers="x-buildbuddy-api-key=xxx"    # Your BuildBuddy API key
    export RBE_use_rpc_credentials=false
    export RBE_service_no_auth=true
   ```

## 3. Important Notes

*   **Personal Cache:** The shared cache is currently unavailable. You must use your own personal BuildBuddy instance/account for caching.
*   **Initial Build Performance:** Since you are starting with a fresh personal cache, your first few builds will be **slower**. During this phase, the system is primarily **uploading** build artifacts to your remote storage. 
*   **Subsequent Builds:** Once your personal cache is populated, subsequent builds (or builds of similar branches) will become significantly faster as the system switches to **downloading** cached results instead of rebuilding locally.