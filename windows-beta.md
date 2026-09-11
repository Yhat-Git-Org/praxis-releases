# Praxis Windows beta — start here

This beta is for 64-bit Intel/AMD Windows PCs. Use Windows 11 for the first test. ARM Windows has not been verified. You do not need Rust, Python, Bun, or developer tools to run Praxis.

The first setup has four parts: install the supporting tools, install Praxis, sign in, and connect your firm's files. Start with fictional documents while testing the beta.

## 1. Accounts you will use

- **Praxis:** create your firm in the app, or join with an invitation code from your firm administrator.
- **GitHub:** each person needs their own account and access to the firm's private repository. This holds shared workspace files and their history.
- **Codex:** this guide uses Codex as the assistant. Sign in with your own supported ChatGPT account. Assistant usage is separate from Praxis; installing Praxis does not provide assistant credits.

Your Praxis, GitHub, and assistant sign-ins are separate. Do not share passwords or access tokens with colleagues or paste them into an assistant conversation.

## 2. Install the supporting tools

Do this on each Windows computer. On a managed work computer, ask IT to install the tools if installation is restricted.

### Git and large-file support

1. Download and install [Git for Windows](https://git-scm.com/downloads/win).
2. Keep the default options, including **Git from the command line and also from third-party software** and **Git Credential Manager** when offered.
3. Open the Windows Start menu, type **Command Prompt**, and open it normally. Administrator mode is not needed for the commands below.
4. Copy each line below, press Enter, and wait for it to finish before running the next:

```bat
git --version
git lfs version
git lfs install
```

The first two commands should print version numbers. The last enables large-file support for your Windows user. If `git lfs` is not recognized, install [Git LFS](https://git-lfs.com/), close Command Prompt, reopen it, and repeat these commands.

### Node.js and the assistant connection

1. Install the **LTS Windows Installer (.msi)** from [Node.js](https://nodejs.org/en/download). Keep npm and the PATH option enabled. You do not need the optional additional native build tools.
2. Close Command Prompt and open a new one so it sees the installation.
3. Run these commands one at a time:

```bat
node --version
npm --version
npm install -g @openai/codex@0.154.0 @agentclientprotocol/codex-acp@1.10.0
codex --version
where codex-acp
codex login
```

The install command downloads the pinned Codex CLI and the adapter that connects it to Praxis. It can take a few minutes. `where codex-acp` should show an installed path, normally under your user's AppData folder.

`codex login` opens a browser. Complete the sign-in there and return to Command Prompt. Then verify:

```bat
codex login status
```

Use these native Windows installations. Installing the tools only inside WSL will not make them available to the Windows Praxis app. You do not need to install the separate Codex desktop app.

Sources: [Codex CLI installation](https://developers.openai.com/codex/cli/), [Codex ACP adapter](https://github.com/agentclientprotocol/codex-acp).

## 3. Install Praxis

1. Download the beta installer whose name ends in **-setup.exe**, together with **SHA256SUMS.txt**, from the release shared by the Praxis team.
2. Double-click the installer and follow its prompts. It installs for your Windows user.
3. This beta is unsigned. Windows may display **Windows protected your PC** or an **Unknown publisher** warning. If the file came from the expected Praxis release and you choose to proceed, use **More info → Run anyway** when available. If your organization's policy blocks it, ask IT; do not disable Windows security.
4. If asked to install **Microsoft Edge WebView2 Runtime**, allow it and wait for completion. Praxis uses it to display the application; internet access may be required.
5. Open **Praxis** from the Start menu. If you installed prerequisites while Praxis was open, quit and reopen it first.

For an optional integrity check, open PowerShell in the download folder and run:

```powershell
Get-FileHash .\Praxis_0.1.0_x64-setup.exe -Algorithm SHA256
```

Substitute the actual installer filename if it differs. Compare the result with SHA256SUMS.txt; case does not matter. A checksum checks the download against the release file; it does not replace publisher signing.

## 4. Sign in to Praxis

- **Starting a firm:** choose the create-firm option on the sign-in screen and enter your firm name, email address, and password. You become its owner.
- **Joining a firm:** ask your administrator for an invitation code, choose **Join your firm**, and use the email address the invitation was created for.
- **Already registered:** sign in with your Praxis email and password.

This beta does not provide self-service password recovery. Keep your password in your password manager. The initial firm allowance is one seat; additional seats require operator provisioning. An invitation does not itself increase that allowance.

## 5. Set up a new firm's files — administrator only

Skip this section if your firm already has a connected workspace; use section 6 instead.

1. Sign in to [GitHub](https://github.com/) in your browser and [create a repository](https://github.com/new).
2. Choose the owner/account that should hold the firm's files. Give it a name such as `firm-workspace` and select **Private**.
3. Leave initialization options off: do not add a README, .gitignore, or license. Praxis needs an empty repository for this new-workspace path.
4. Copy its HTTPS address, for example `https://github.com/YOUR-ACCOUNT/firm-workspace.git`.
5. In Praxis, open **Settings → Storage**, paste that address into **Private GitHub repository**, and choose **Save firm repository**.
6. Open **Workspace setup** and choose **Check this computer**. Git, large-file storage, and Codex should be ready. You only need one assistant; missing Claude or Grok is not a blocker.
7. Expand **Starting a new firm workspace?**. Enter the firm name and a new folder name, then choose **Choose location and create workspace**.
8. Choose a local parent folder outside OneDrive, Dropbox, iCloud, or other cloud-sync folders. For example, create a `PraxisWorkspaces` folder directly inside your Windows user folder. Avoid Documents/Desktop if OneDrive manages them.
9. Choose **Connect this folder**. If Git Credential Manager opens a browser or sign-in window, sign in to GitHub with the account that can access the repository.
10. Choose **Save and sync**. Wait until Praxis confirms the changes are shared. Check the repository in your browser: it should now contain the workspace files, including AGENTS.md, src, clients, and unbound.

If GitHub authentication does not open or connection fails, open Command Prompt and run:

```bat
git ls-remote https://github.com/YOUR-ACCOUNT/firm-workspace.git
```

Replace the example with the actual repository address. Complete the browser sign-in. A new empty repository may produce no output; returning without an error is normal. Then retry **Connect this folder** in Praxis. Never put a password or token in the repository URL.

## 6. Join an existing firm workspace

1. Ask your administrator to give your own GitHub account access to the private repository. Accept any GitHub invitation.
2. Separately, join the firm in Praxis using its invitation code. Your administrator must also make a Praxis seat available.
3. Open **Workspace setup**. The firm's configured repository should be shown.
4. Choose a new folder name, then **Choose location and download workspace**. Choose a local parent folder outside cloud-sync folders.
5. Complete GitHub sign-in if prompted. If authentication fails, use the `git ls-remote` check in section 5, then retry.
6. Ask the administrator to assign the matters you should work on. Praxis membership and GitHub repository access are separate; matter assignments guide work but do not hide repository files from people who have repository access.

Do not create a second empty workspace when the firm already has files: download the existing one.

## 7. Try the first document

1. Start a new conversation and select **Codex**.
2. For the first beta test, send: “Create a fictional client Jane Example and a matter called Beta test. Draft a short letter confirming an appointment. Use fictional details and show me the document.”
3. Confirm that the assistant responds and that the rendered document opens. Request a small edit and check the preview changes.
4. Export the document as a PDF and open it in your preferred PDF reader.
5. Choose **Save and sync** after the assistant finishes, and confirm the changes reach GitHub.

Review all documents before using them. The assistant processes material you supply through your chosen provider. Workspace files are local and shared through your firm's GitHub repository; offline changes are not on GitHub until synchronization succeeds.

## Common problems

| What you see | What to do |
|---|---|
| A command “is not recognized” | Close and reopen Command Prompt. Verify the relevant installer completed and PATH was enabled. |
| PowerShell says npm scripts are disabled | Use **Command Prompt** for the prerequisite commands. You do not need to change execution policy. |
| Codex says “Setup needed” in Praxis | Run `where codex-acp` in Command Prompt, verify the install succeeded, then quit and reopen Praxis. |
| Assistant is installed but will not respond | Run `codex login status`; sign in again if needed. Check your provider account/allowance. Installation alone does not establish sign-in. |
| Cannot reach Praxis or sign in | Check internet access. The hosted beta can take a moment to wake up; retry after a short wait. If it persists, send the error text to the beta organizer. |
| GitHub says repository not found or permission denied | Check the exact repository URL, the signed-in GitHub account, and whether its repository invitation was accepted. |
| Workspace folder is rejected | Choose a new folder outside OneDrive/Dropbox and outside any existing repository. |
| Download failed and left a folder | Keep the folder until its contents are checked. Retry using a different new folder name; do not blindly delete a folder that may contain work. |
| No seat / no matter assignment | Ask the firm's Praxis administrator. A GitHub invitation does not grant these permissions. |
| Windows blocks the unsigned app | Ask IT on managed devices. This beta has no trusted publisher signature. |

## Updates, removal, and feedback

Automatic Praxis updates are not included in this beta. Quit Praxis before installing a newer beta from the team's release page. Keep the supporting tools at the documented versions until the beta guide changes.

To remove the app, use **Windows Settings → Apps → Installed apps → Praxis → Uninstall**. Keep your workspace folders and GitHub repository unless you deliberately want to remove their contents; uninstalling the app is not a request to delete legal work.

When reporting a problem, include the installer version, Windows version, the step you were on, and the exact error. Redact client details, passwords, and tokens. The beta organizer will ask you to verify installation, account creation, restart/sign-in persistence, workspace sharing, assistant drafting, and PDF export on a real Windows PC.

## Feedback (Praxis 0.1.1 and later)

Choose **Feedback** in the top toolbar to report a problem, suggest an improvement, or ask a question. Add a short summary and details. Use **Add screenshots or images**, or paste a screenshot into Details. You can attach three PNG/JPEG images per message, up to 2 MiB each. Review screenshots for client details before sending.

To check progress or reply, open **Feedback → … → View feedback and replies**, then select your report. Use Refresh to load recent updates. Only you and designated Praxis feedback reviewers can see your report and images; other members of your firm cannot. There are no email notifications or automatic updates in this beta. If sending fails, your draft stays in the dialog while the app remains open.
