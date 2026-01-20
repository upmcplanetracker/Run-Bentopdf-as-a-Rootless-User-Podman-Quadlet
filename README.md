BentoPDF Podman User Quadlet
============================

**Disclaimer:** I am not affiliated with, associated, authorized, endorsed by, or in any way officially connected with the BentoPDF project.

This guide demonstrates how to deploy **BentoPDF**—a privacy-first, client-side PDF toolkit—as a **User (Rootless) Quadlet** on Ubuntu 25.10.

System Information
------------------

*   **Tested Environment:** Ubuntu 25.10
*   **Podman Version:** 5.4.x
*   **Configuration Type:** User-level Quadlet (Rootless)

1\. The Quadlet File (bentopdf.container)
-----------------------------------------

Download or create the `bentopdf.container` file. This version uses `default.target` to ensure the container starts when your user session begins.

2\. Installation & Setup
------------------------

### Step A: Create the Quadlet Directory

Unlike system quadlets, user quadlets live in your home directory:

`mkdir -p ~/.config/containers/systemd`

### Step B: Move the File

Move the `bentopdf.container` file into that new directory:

`mv bentopdf.container ~/.config/containers/systemd/`

### Step C: Enable Lingering (Optional but Recommended)

To ensure the container stays running after you log out of SSH, enable lingering for your user account:

`sudo loginctl enable-linger $USER`

3\. Activation
--------------

Reload the **user** systemd daemon and start the service. Note the `--user` flag—this is critical for rootless deployments:

`systemctl --user daemon-reload`
`systemctl --user start bentopdf`
`systemctl --user enable bentopdf`

4\. Verification
----------------

Check the status of your user-level service:

`systemctl --user status bentopdf`

Check the logs:

`journalctl --user -u bentopdf -f`

**Accessing the UI:** Once running, BentoPDF will be available at `http://[Your-IP]:8080`.

5\. Updating
------------

Since `AutoUpdate=registry` is enabled, you can update BentoPDF using the podman auto-update command for your user:

`podman auto-update`
