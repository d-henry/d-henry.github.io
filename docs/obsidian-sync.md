# Obsidian + GitHub Sync: A Complete Setup Guide

This guide walks you through setting up [Obsidian](https://obsidian.md/) so your notes automatically sync across multiple devices using GitHub — completely free, with no paid sync subscription required.

**No programming or Git experience needed.** Every step is spelled out. If you've never opened a terminal in your life, you can still do this — we'll use point-and-click tools the whole way through, with an optional command-line appendix at the end for anyone curious.

---

## What You're Actually Building

Before diving in, here's the mental model:

- **Obsidian** is just a note-taking app that stores your notes as plain text files in a folder on your computer (called a "vault").
- **GitHub** is a free online storage service built for tracking changes to files. We're going to (ab)use it as a private backup + sync location for your notes folder.
- **Git** is the tool that moves your notes back and forth between your computer and GitHub. Think of it like a very smart, very safe version of copy-paste that also keeps a history of every change.
- The **Obsidian Git plugin** does all the Git work for you with a couple of clicks — you will almost never need to type a Git command.

Once set up: you write notes on your laptop, the plugin uploads them to GitHub. You open Obsidian on your phone or another computer, the plugin downloads the latest notes. That's the whole workflow.

---

## Part 1: Create Your GitHub Account and Repository

### Step 1.1 — Create a GitHub account

1. Go to [github.com](https://github.com/) in your web browser.
2. Click **Sign up** in the top-right corner.
3. Enter your email, create a password, and pick a username.
4. Verify your email address when GitHub sends you a confirmation link.

### Step 1.2 — Create a new repository for your notes

A "repository" (or "repo") is just a project folder on GitHub.

1. Once logged in, click the **+** icon in the top-right corner of the page, then click **New repository**.
2. Under **Repository name**, type something like `obsidian-vault` (no spaces — use a hyphen instead).
3. Set the repository to **Private**. This is important — your notes should not be public. Click the **Private** radio button.
4. Leave **"Add a README file"** checked — this gives the repo an initial file so it isn't empty.
5. Click the green **Create repository** button at the bottom.

You now have an empty, private, online folder ready to hold your notes. Leave this browser tab open — you'll come back to it.

---

## Part 2: Install Git

The Obsidian Git plugin needs the actual Git program installed on your computer to work behind the scenes, even though you won't type Git commands yourself.

### Windows

1. Go to [git-scm.com/downloads](https://git-scm.com/downloads).
2. Click **Windows**, then download the 64-bit installer.
3. Run the installer. On every screen, the default options are fine — just keep clicking **Next**, then **Install**.
4. When it finishes, click **Finish**.

### Mac

1. Open the **Terminal** app (press `Cmd + Space`, type "Terminal", press Enter).
2. Type `git --version` and press Enter.
3. If Git isn't already installed, macOS will pop up a prompt asking to install the "Command Line Developer Tools." Click **Install** and wait for it to finish.

### Verify it worked (all platforms)

- **Windows:** Open the Start menu, search for "Git Bash," and open it. Type `git --version` and press Enter. You should see something like `git version 2.44.0`.
- **Mac:** In Terminal, type `git --version` and press Enter.

If you see a version number, Git is installed correctly. You will not need to use this window again — the Obsidian plugin handles everything from here.

---

## Part 3: Set Up Authentication (Personal Access Token)

GitHub no longer accepts your account password for Git operations — you need a **Personal Access Token (PAT)** instead. Think of it as a special, revocable password just for Git.

### Step 3.1 — Generate the token

1. On GitHub, click your profile picture (top-right) → **Settings**.
2. Scroll to the bottom of the left sidebar and click **Developer settings**.
3. Click **Personal access tokens** → **Tokens (classic)**.
4. Click **Generate new token** → **Generate new token (classic)**.
5. Under **Note**, type something like `Obsidian Vault Sync` so you remember what it's for.
6. Under **Expiration**, choose **No expiration** (simplest for personal use) or a long duration like 1 year if you'd rather rotate it periodically.
7. Under **Select scopes**, check the box next to **repo** (this automatically checks all the sub-boxes under it).
8. Scroll down and click **Generate token**.
9. **Important:** GitHub shows you the token exactly once. Click the copy icon next to it and paste it somewhere safe temporarily (a password manager, or a sticky note you'll delete after this setup) — you'll need it in the next steps and you cannot view it again after leaving this page.

If you ever lose it, you can just generate a new one and repeat the setup — it costs nothing to redo.

> **Security note:** "No expiration" means this token can read and write your notes repo forever until you manually delete it. That's fine for a personal setup, but if you ever lose a device or stop using it, go revoke the token: **Settings → Developer settings → Personal access tokens → Tokens (classic)**, find it in the list, and click **Delete**. That immediately cuts off access from anything using that token.

---

## Part 4: Install Obsidian and Create Your Vault

### Step 4.1 — Install Obsidian

1. Go to [obsidian.md](https://obsidian.md/) and click **Get Obsidian**.
2. Download and install the version for your operating system (Windows, Mac, or mobile).
3. Open Obsidian.

### Step 4.2 — Create a new vault

1. On the welcome screen, click **Create new vault**.
2. Give it a name (e.g., `Notes`).
3. Choose where to save it on your computer — the default location is fine.
4. Click **Create**.

You now have a working notes app with an empty vault, unrelated to GitHub so far. Let's connect them.

---

## Part 5: Install the Obsidian Git Plugin

### Step 5.1 — Enable community plugins

1. In Obsidian, click the **gear icon** (Settings) in the lower-left corner.
2. Click **Community plugins** in the left sidebar.
3. If you see a warning about restricted mode, click **Turn on community plugins** and confirm.

### Step 5.2 — Install the "Git" plugin

1. Still in **Community plugins**, click **Browse**.
2. In the search box, type `Git`.
3. Find the plugin literally named **Git** (by Vinzent, sometimes shown as "Obsidian Git"). Click on it.
4. Click **Install**.
5. Once installed, click **Enable**.

You'll now see a new **Git** section in your left sidebar settings, and new Git-related commands available.

---

## Part 6: Connect Your Vault to GitHub

This is the step that links your local notes folder to the GitHub repository you created in Part 1.

### Step 6.1 — Point your vault at the GitHub repo

Because your vault folder already has notes (even if just the welcome note) and your GitHub repo already has a README, we need to link them together. The simplest reliable way for a first-timer is to clone the GitHub repo directly into place using the plugin's built-in clone command:

1. In Obsidian, press `Ctrl + P` (Windows) or `Cmd + P` (Mac) to open the **Command palette**.
2. Type `Git: Clone an existing remote repo` and select it.
3. It will ask for the repository URL. Go back to your GitHub repo's page in the browser, click the green **Code** button, and copy the **HTTPS** URL (looks like `https://github.com/yourusername/obsidian-vault.git`).
4. Paste that URL into Obsidian's prompt and press Enter.
5. It will ask for a folder location — choose a **new, empty folder** (not your existing vault folder). For example, choose to create a folder called `Notes-Synced` on your Desktop.
6. The first time you do this, Obsidian/Git will prompt you for credentials:
   - **Username:** your GitHub username.
   - **Password:** paste the **Personal Access Token** you generated in Part 3 (not your actual GitHub password).
7. Once cloning finishes, open that new folder as your Obsidian vault: **File → Open another vault → Open folder as vault**, and select the folder you just cloned into.

> **Why clone into a new folder instead of linking your existing vault directly?** Git needs the folder to match what's on GitHub exactly when it starts tracking it. Starting from a fresh clone avoids confusing merge conflicts on your very first sync. It's an extra step now that saves you a headache later.

### Step 6.2 — Bring over your existing notes (if you have any)

If you already had a vault with notes in it before starting this guide, move them into the new Git-connected vault now:

1. Open your **old** vault's folder in your computer's file explorer (Finder on Mac, File Explorer on Windows). In Obsidian, you can find it via **Settings → About → Show/Open vault folder** while that old vault is open.
2. Select all your note files and any attachment folders (images, PDFs, etc.) — everything *except* the hidden `.obsidian` folder for now.
3. Copy those files and paste them into your **new**, Git-connected vault folder (the one you cloned in Step 6.1).
4. If you had custom settings, themes, or plugins configured in your old vault and want to keep them, you can also copy over the old `.obsidian` folder — but do this *before* Step 6.3 below, so the file we're about to create can exclude the parts of it that shouldn't be synced.

From this point forward, **the new cloned folder is your permanent vault** — always open Obsidian to this one, and it's safe to delete the old one once you've confirmed everything moved over correctly.

### Step 6.3 — Tell Git to ignore a couple of noisy files

Obsidian keeps track of things like which panes and tabs you have open in a file called `.obsidian/workspace.json`. That file changes almost every time you touch Obsidian — even just clicking around — which means if you sync it, you'll get pointless "conflicts" between your devices over things like window layout instead of actual notes. We want Git to ignore it.

1. In your file explorer, go into your new vault folder.
2. Create a new plain text file named exactly `.gitignore` (note the leading dot, and no `.txt` at the end — on Windows you may need to name it `.gitignore.` with a trailing dot to stop Explorer from renaming it).
3. Open it in any text editor and add these two lines:

```
.obsidian/workspace.json
.obsidian/workspace-mobile.json
```

4. Save the file.

This only needs to be done once — it travels with the repo to every device once you push it.

> **Heads up on attachments:** GitHub rejects any individual file over 100MB and warns you above 50MB. If you keep large videos or big PDFs as note attachments, they may fail to sync — this setup is best suited for text notes and reasonably-sized images.

### Step 6.4 — Do your first backup (commit + push)

"Commit" means "save a snapshot of my changes." "Push" means "upload that snapshot to GitHub."

1. Open the Command palette (`Ctrl/Cmd + P`).
2. Type `Git: Commit all changes` and press Enter. A box pops up for a commit message — type something like `Initial notes` and press Enter/click the checkmark.
3. Open the Command palette again and type `Git: Push` and press Enter.
4. If prompted for credentials again, use your GitHub username and the Personal Access Token (same as before).

Go check your GitHub repository page in the browser and refresh it — you should see your notes files listed there now. That confirms sync is working.

---

## Part 7: Set Up Automatic Syncing

Rather than manually committing and pushing every time, let the plugin do it for you.

1. Open Obsidian **Settings → Git** (in the plugin list on the left, it should appear as its own settings tab once enabled).
2. Find **Vault backup interval (minutes)** and set it to something like `10`. This makes the plugin automatically commit and push your changes every 10 minutes if anything changed.
3. Find **Auto pull interval (minutes)** and set it similarly (e.g., `10`). This automatically downloads changes made from other devices.
4. Optional but recommended: enable **Pull changes before push** and **Push on backup** if you see those toggles — this reduces the odds of conflicts.

With this on, you can mostly forget Git exists. Just write notes; the plugin syncs in the background.

---

## Part 8: Add a Second Device (Phone or Another Computer)

### On another computer

Repeat **Part 2** (install Git), **Part 4** (install Obsidian), and **Part 5** (install the Git plugin). Then instead of creating a new vault, use the same clone step from **Part 6.1** to pull down your existing notes:

1. `Git: Clone an existing remote repo`, paste the same GitHub URL, choose a folder, sign in with your GitHub username + the same Personal Access Token (or generate a new token for this device).
2. Open that folder as your vault.

### On your phone (iOS or Android)

1. Install the **Obsidian** app from the App Store or Google Play.
2. Create a vault (any name — it'll be replaced by the sync).
3. Go to **Settings → Community plugins**, turn on community plugins, and install/enable the **Git** plugin, exactly as in Part 5.
4. Open the Command palette (tap the icon that looks like `>_` or swipe from the edge, depending on your Obsidian version) and run `Git: Clone an existing remote repo`, same as before, using your GitHub username and Personal Access Token.
5. Turn on the same auto-backup/auto-pull interval settings from Part 7.

**Mobile note — read this:** on desktop, the plugin uses the real Git program you installed in Part 2. On mobile, there's no such thing, so the plugin uses a different, JS-based Git implementation behind the scenes. It generally works, but it's less mature than the desktop version — expect it to be somewhat slower, occasionally flakier on first clone, and worth double-checking after major Obsidian or plugin updates since mobile support has changed between versions. Phone syncing can also lag behind because mobile operating systems restrict what apps can do in the background.

If your phone notes look out of date, don't panic — open the Command palette and run `Git: Pull` manually, which reliably forces a fresh download. If a clone or sync fails outright on mobile, doing that same clone once on a laptop/desktop first and confirming it works there is a good way to tell whether the problem is your repo or just the mobile Git implementation.

---

## Part 9: What to Do If You Get a Merge Conflict

This is the scariest-sounding part for beginners, but it's rare if you let auto-pull run before you start editing, and it's not dangerous — Git never deletes your data, it just needs you to pick which version to keep.

A conflict happens when you edited the same note on two devices before they had a chance to sync with each other.

**If it happens:**

1. The Git plugin will show a notice mentioning a merge conflict, and the affected note will contain both versions of the text, separated by markers that look like this:

```
<<<<<<< HEAD
This is the version from this device.
=======
This is the version from the other device.
>>>>>>> some-random-id
```

2. Open the note. Manually edit it: delete the version you don't want to keep, and delete the `<<<<<<<`, `=======`, and `>>>>>>>` marker lines themselves. Keep just the clean text you want.
3. Save the note.
4. Run `Git: Commit all changes` from the Command palette, then `Git: Push`.

That's it — conflict resolved. To avoid this entirely, get in the habit of waiting a few seconds after opening Obsidian on any device (letting auto-pull finish) before you start typing.

---

## Quick Reference: The Only Commands You Need

Access all of these via the Command palette (`Ctrl/Cmd + P`), typing `Git:` to filter the list:

| Command | What it does | When to use it |
|---|---|---|
| `Git: Pull` | Downloads the latest notes from GitHub | When switching to a device you haven't used in a while |
| `Git: Commit all changes` | Saves a snapshot of your current changes | Rarely needed manually if auto-backup is on |
| `Git: Push` | Uploads your snapshot to GitHub | Rarely needed manually if auto-backup is on |
| `Git: Commit and Sync` | Does commit + pull + push in one step | Good manual "sync now" button |

---

## Appendix: Doing It From the Command Line (Optional)

If you ever get curious about what the plugin is doing under the hood, here's the equivalent raw Git workflow — you never need this for the plugin-based setup above to work, but it's useful if you want to understand Git itself.

```bash
# Clone your notes repo to a new folder
git clone https://github.com/yourusername/obsidian-vault.git

# Move into that folder
cd obsidian-vault

# After making changes, save a snapshot
git add .
git commit -m "Updated notes"

# Upload it to GitHub
git push

# Download the latest changes from GitHub
git pull
```

When prompted for a password on the command line, use your Personal Access Token, not your GitHub account password.

---

## Optional: A Plugin Worth Trying Once You're Set Up

Unrelated to syncing, but worth knowing about once your vault is up and running: [Cornell Notes for Obsidian](https://github.com/bytetiles/obsidian-cornell-notes) renders the classic two-column Cornell note-taking layout (cues on the left, notes on the right) right inside Obsidian's Reading view. You write it as a fenced ` ```cornell ` code block using `::cue` and `::note` markers, and it supports lists, tables, code, and images in either column. Install it the same way you installed the Git plugin — **Settings → Community plugins → Browse** and search for "Cornell Notes."

One quirk to know going in: the two-column layout only renders in **Reading view**, not Live Preview, and it won't render Obsidian's `~sub~`/`^sup^` shorthand (use HTML tags or Unicode characters instead if you need those).

---

## Links

- [Obsidian](https://obsidian.md/)
- [Obsidian Git plugin (GitHub page)](https://github.com/Vinzent03/obsidian-git)
- [GitHub: Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Git downloads](https://git-scm.com/downloads)
