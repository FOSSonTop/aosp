---
order: 1
---

# Building Android with Remote Build Execution (RBE)

## Download and Configure `reclient`

1.  **Download:** Download the `reclient` package from [this link](https://chrome-infra-packages.appspot.com/p/infra/rbe/client/linux-amd64/+/latest). This is a pre-built version of Google's `reclient` tool.
2.  **Extract:** Extract the downloaded archive to a directory within your ROM's source tree. For example, you could create a directory called `rbe` in the root of your AOSP directory and extract it there. It is important to keep the extracted directory structure.
3.  **Note Path:** Remember the *relative* path from your AOSP root to this directory (e.g., `rbe`) or the absolute path.
