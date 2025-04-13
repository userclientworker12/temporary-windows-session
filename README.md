# Temporary Windows Environment via GitHub Actions

This repository contains a GitHub Actions workflow to launch a temporary Windows environment running on a GitHub-hosted runner.

**Features:**

*   Uses `windows-latest` runner.
*   Sets maximum job timeout (usually 6 hours).
*   Provides interactive access **via SSH or Web Shell** using `tmate`.
*   Allows customization for installing tools (edit the workflow file).

**IMPORTANT LIMITATIONS:**

*   **Ephemeral:** The environment exists *only* for the duration of the workflow job (max ~6 hours). It is completely destroyed afterwards.
*   **No RDP:** Standard Remote Desktop is **not** available. Access is via a temporary SSH/Web connection provided by `tmate`.
*   **Not a Persistent VM:** This is for temporary testing, debugging, or running long tasks within the CI/CD environment, not for hosting services.
*   **Resource Limits:** Subject to GitHub Actions resource and usage limits based on your account type (free/paid).

**How to Run:**

1.  Go to the **Actions** tab in this repository.
2.  Select the **"Temporary Windows Environment Session"** workflow from the left sidebar.
3.  Click the **"Run workflow"** dropdown button on the right.
4.  Click the green **"Run workflow"** button.

**How to Connect (Using tmate):**

1.  Once the workflow run starts, click on it to view its progress.
2.  Click on the **"Start Temporary Windows Session"** job.
3.  Wait for the **"Setup tmate session"** step to start running.
4.  Look carefully in the logs of that step. You will see output similar to this:
    ```
    Web shell: https://tmate.io/t/....
    SSH: ssh ...@...tmate.io
    ```
5.  **To connect:**
    *   **Web Shell:** Copy the `https://tmate.io/t/...` URL and paste it into your web browser.
    *   **SSH:** Copy the `ssh ...@...tmate.io` command and paste it into your local terminal/shell (you need an SSH client installed).
6.  You now have a command-line interface into the running GitHub Actions Windows runner. You can run commands, explore files, etc.
7.  The session (and the workflow job) will remain active until you disconnect from `tmate` (type `exit` in the tmate shell) or the 6-hour job timeout is reached.

**Customization:**

*   Edit the `.github/workflows/temporary-windows-env.yml` file.
*   Add `choco install ...` commands or uncomment/add `actions/setup-*` steps in the "Setup Environment" step to install the software you need *before* the `tmate` session starts.
