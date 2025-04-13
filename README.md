# Temporary Windows RDP Session via GitHub Actions & ngrok

This workflow attempts to provide a temporary **RDP-based** graphical Windows desktop session running on a GitHub Actions runner, tunneled via ngrok.

**EXTREME WARNINGS APPLY:**

*   **HIGHLY EXPERIMENTAL & UNRELIABLE:** This is NOT a standard or supported use case for GitHub Actions. It pushes the boundaries and may break without notice due to changes in the runner environment.
*   **POOR PERFORMANCE:** RDP over internet tunnels like ngrok is **extremely sensitive to latency**. Expect significant lag, slow screen updates, and a potentially unusable experience for many tasks.
*   **SECURITY RISK:** Enabling RDP and tunneling it publicly (even via an obscure ngrok URL) increases the security exposure of the temporary runner environment.
*   **EPHEMERAL:** The session is **completely destroyed** when the workflow job ends (max ~6 hours). No data persists.
*   **RDP Specifics:** Requires setting a password for the runner user and enabling RDP services/firewall rules on the fly.

**Prerequisites:**

1.  **ngrok Account:** Sign up at [ngrok.com](https://ngrok.com).
2.  **ngrok Authtoken:** Get it from your ngrok dashboard.
3.  **GitHub Secret:** Create a repository secret named `NGROK_AUTHTOKEN` and paste your token there (`Settings` > `Secrets and variables` > `Actions`).
4.  **RDP Client:** Use the built-in "Remote Desktop Connection" (`mstsc.exe`) on Windows or another RDP client.

**How to Run:**

1.  Go to the **Actions** tab.
2.  Select the **"Temporary Windows RDP Session via ngrok"** workflow.
3.  Click **"Run workflow"** -> **"Run workflow"**.

**How to Connect:**

1.  Open the running workflow job.
2.  Wait for the step **"Display Connection Info and Keep Job Alive"** to run.
3.  Look in the logs for lines like:
    ```
    Computer:      tcp://0.tcp.ngrok.io:12345
    Username:      runneradmin
    Password:      SomeRandomP@ssw0rd!
    ```
4.  Open your local **Remote Desktop Connection** application (`mstsc.exe`).
5.  In the **"Computer"** field, enter the **hostname and port** from the ngrok address.
    *   *Example:* If the log shows `tcp://2.tcp.ngrok.io:12345`, you enter `2.tcp.ngrok.io:12345`
6.  Click **"Connect"**.
7.  When prompted for credentials:
    *   Enter the **Username** shown in the logs (e.g., `runneradmin`). You might need to click "More choices" -> "Use a different account".
    *   Enter the **Password** shown in the logs.
8.  Accept any certificate warnings (as it's a temporary, self-managed machine).
9.  If it works (and that's a big *if*), you should connect to the graphical desktop of the runner. **Be prepared for lag.**

**Disconnecting:**

*   Disconnect or Log Off from the RDP session as usual.
*   The GitHub Actions job will continue running until its 6-hour timeout. Manually cancelling the workflow run on GitHub is recommended when finished.
