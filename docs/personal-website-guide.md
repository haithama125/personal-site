# Building Your Personal Website: A Complete Beginner's Guide

A step-by-step walkthrough for building a personal/resume website using **VS Code**, **Claude Code**, and **GitHub** — written for someone who has never coded before.

**You're on:** macOS
**You already have:** VS Code, a GitHub account
**You'll build:** A single-page personal website (hero, about, projects, contact)
**You'll learn:** Real version control with Git, branches, GitHub, and how to direct an AI coding assistant.

---

## How to use this guide

Read each part top-to-bottom. Don't skip ahead — each step depends on the one before it. When you see a code block, that means "type this in your terminal." When you see "ask Claude," that means "type this as a prompt inside Claude Code."

Take your time. Total active time is maybe 2–3 hours, but spread it over a couple of sessions if you want.

---

## Part 1: Install the tools you're missing

You already have VS Code and a GitHub account. You still need three things: **Homebrew** (a tool that installs other tools on Mac), **Git**, **Node.js**, and **Claude Code**.

### 1.1 Open the Terminal

Press `Cmd + Space`, type "Terminal", and hit Enter. A black/white window opens. This is where you type commands. When this guide says "run X," it means type X here and press Enter.

### 1.2 Install Homebrew

Homebrew is the standard package manager for Mac. Paste this into Terminal and press Enter:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

It'll ask for your Mac password (the one you use to log in). You won't see characters as you type — that's normal. Press Enter when done. The install takes a few minutes.

When it finishes, it'll print 2–3 lines under "Next steps" that start with `echo`. **Copy those exact lines and run them** — they make `brew` available in your Terminal.

Verify it worked:
```
brew --version
```
You should see something like `Homebrew 4.x.x`.

### 1.3 Install Git and Node.js

```
brew install git node
```

This installs both at once. Verify:
```
git --version
node --version
npm --version
```

All three should print version numbers. If any say "command not found," close Terminal, open it again, and try once more.

### 1.4 Tell Git who you are

Git stamps every change you make with your name and email. Run these (replace the placeholders with your real info — use the email tied to your GitHub account):

```
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
```

The third line makes Git use `main` as the default branch name, which matches GitHub.

### 1.5 Install Claude Code

```
npm install -g @anthropic-ai/claude-code
```

Verify:
```
claude --version
```

You're done with installs.

---

## Part 2: Create your project folder

Code lives in folders. Let's make one for your website.

```
mkdir -p ~/Projects/personal-site
cd ~/Projects/personal-site
```

- `mkdir -p` creates a folder (and any parent folders that don't exist).
- `~` is shorthand for your home folder (e.g., `/Users/yourname`).
- `cd` means "change directory" — you're now *inside* that folder.

Open it in VS Code:
```
code .
```

The `.` means "this current folder." VS Code opens with an empty file tree on the left. (If `code .` doesn't work, open VS Code, go to View → Command Palette, type "Shell Command: Install 'code' command in PATH," and run it. Then try again.)

In VS Code, open a terminal: **Terminal → New Terminal** (or `` Ctrl + ` ``). This terminal is already inside your project folder. Use this from now on instead of the standalone Terminal app.

---

## Part 3: Understand Git in 2 minutes

Before you type any Git commands, here's what they actually mean. Don't skip this — it'll save you confusion later.

- **Repository (repo):** A folder that Git is tracking. Your project folder will become one.
- **Commit:** A snapshot of your project at a moment in time. Like a save point in a video game. Each commit has a message describing what changed.
- **Branch:** A parallel timeline. The default branch is `main`. When you want to try something risky or work on a feature, you make a branch. If it works, you merge it back into `main`. If it doesn't, you throw the branch away — `main` is untouched.
- **Remote:** A copy of your repo that lives somewhere else (on GitHub). You **push** your local commits up to it, and **pull** other commits down.
- **Push / Pull:** Upload / download commits between your computer and GitHub.

The mental model: you work locally, save snapshots (commits) as you go, and periodically push them to GitHub for backup and sharing. Branches let you experiment without breaking what works.

---

## Part 4: Turn your folder into a Git repo

In your VS Code terminal (still inside `~/Projects/personal-site`):

```
git init
```

That created a hidden `.git` folder. Git is now watching this folder.

Create a `.gitignore` file. This tells Git to ignore certain files (like build artifacts and secrets). In VS Code, click the "New File" icon in the file tree, name it `.gitignore`, and paste:

```
node_modules/
.DS_Store
.env
.env.local
dist/
build/
.vercel
```

Save it (`Cmd + S`).

Make your first commit:
```
git add .
git commit -m "Initial commit"
```

- `git add .` stages all your changes (the `.` means "everything in this folder").
- `git commit -m "..."` creates the snapshot with that message.

Check the history:
```
git log
```

You should see one commit, by you. Press `q` to exit.

---

## Part 5: Connect to GitHub

### 5.1 Create the repo on GitHub

In your browser, go to **github.com** and click the **+** in the top right → **New repository**.

- **Repository name:** `personal-site` (or whatever you like)
- **Description:** "My personal website"
- **Public** (so you can show it off later) or **Private** (your call)
- **Do NOT** check "Add a README" or "Add a .gitignore" — your local repo already has commits, and adding files on GitHub now will create conflicts.
- Click **Create repository**.

GitHub shows a page with setup commands. You want the section labeled **"…or push an existing repository from the command line."** It looks like:

```
git remote add origin https://github.com/yourname/personal-site.git
git branch -M main
git push -u origin main
```

### 5.2 Set up GitHub authentication

The first time you `git push`, GitHub will need to verify it's really you. The easiest path on Mac is to install GitHub's command-line tool:

```
brew install gh
gh auth login
```

Follow the prompts: choose **GitHub.com**, **HTTPS**, **Yes** to authenticate Git with your GitHub credentials, then **Login with a web browser**. It'll print a one-time code, copy it, open the browser, and complete the login there.

### 5.3 Push your repo

Now run those three commands GitHub gave you (paste them into your VS Code terminal, replacing `yourname` with your actual GitHub username):

```
git remote add origin https://github.com/yourname/personal-site.git
git branch -M main
git push -u origin main
```

Refresh the GitHub page — your `.gitignore` file should be there. You're now backed up to GitHub.

---

## Part 6: Build the site with Claude Code

Time for the fun part. In your VS Code terminal:

```
claude
```

The first run, it'll ask you to log in (browser opens, sign in with your Anthropic account). After that you're at a prompt inside Claude Code.

### 6.1 Your first prompt

Type this (or your own version of it):

> Build me a single-page personal website using plain HTML, CSS, and a small amount of JavaScript — no frameworks. The page should have: a hero section with my name "[YOUR NAME]" and a one-line tagline, an About section (placeholder text I'll edit), a Projects section with 3 project cards (placeholder), and a Contact section with my email. Make it responsive (works on phone), use a clean modern design with good typography, and include a dark mode toggle. Put everything in a single index.html file with inline CSS in a <style> tag and inline JS in a <script> tag, so it's easy for me to read.

Claude Code will create `index.html`. When it's done, in your VS Code file tree double-click `index.html` to open it. To preview it in a browser, right-click the file → **Reveal in Finder** → double-click it. Or install the **Live Server** extension in VS Code (Extensions panel on the left, search "Live Server" by Ritwick Dey, install it). Then right-click `index.html` → **Open with Live Server**. Live Server auto-refreshes as you save.

### 6.2 Iterate

Look at the page. Things you'll probably want to tweak:

> Move the dark mode toggle to the top-right corner of the page and make it a small icon button instead of a text button.

> The hero section feels too cramped on mobile — increase the vertical padding and make the tagline font smaller.

> Replace the project card placeholders with real ones: 1) "[Project name]" — [one-line description], built with [tech]. 2) ... 3) ...

Claude Code edits the file in place. Reload the browser (or Live Server does it for you).

When you're happy with a meaningful change, commit it. Exit Claude Code with `Ctrl + C` twice (or type `/exit`), then in the terminal:

```
git add .
git commit -m "Add hero, about, projects, contact sections"
git push
```

That third command pushes to GitHub. You'll see the file appear there. Get into the habit of committing often — every time you finish "a thing." Your future self will thank you.

---

## Part 7: The branching workflow

Now you'll learn the real-world Git rhythm: never work directly on `main`. Make a branch for every change.

### 7.1 Create a branch

Say you want to add a "Skills" section. Before touching anything:

```
git checkout -b add-skills-section
```

- `git checkout -b name` creates a new branch called `name` and switches you to it.
- You're now on `add-skills-section`. `main` is frozen at its last state.

Verify:
```
git branch
```
You'll see `* add-skills-section` (the `*` marks the current one) and `main`.

### 7.2 Make the change

Run `claude` again and ask:

> Add a "Skills" section between About and Projects. Show skills grouped into 3 categories: Languages, Tools, Other. Use small pill-shaped tags.

When you like the result, commit:

```
git add .
git commit -m "Add Skills section with categorized pill tags"
```

### 7.3 Push the branch

```
git push -u origin add-skills-section
```

The `-u origin add-skills-section` tells Git "send this branch up to GitHub and remember that's where it lives." Future pushes from this branch are just `git push`.

### 7.4 Open a Pull Request on GitHub

Refresh your GitHub repo page. You'll see a yellow banner: "add-skills-section had recent pushes" with a **Compare & pull request** button. Click it.

A pull request (PR) is a proposal: "I want to merge these changes into main." On a team, others would review it. Solo, you're reviewing your own work — but it's still useful because GitHub shows you a clean diff of exactly what changed.

Add a description if you want, then click **Create pull request**.

### 7.5 Merge it

Click **Merge pull request** → **Confirm merge**. Your changes are now on `main` on GitHub.

Bring `main` back to your computer:

```
git checkout main
git pull
```

Delete the old branch (it's done its job):

```
git branch -d add-skills-section
git push origin --delete add-skills-section
```

That's the loop. **Every new piece of work** = new branch → commit → push → PR → merge → delete branch.

### 7.6 Why bother with branches solo?

Three real benefits, even when no one's reviewing:

1. **`main` is always working.** If you mess up a branch badly, just `git checkout main` and you're back to a known-good state.
2. **Cleaner history.** PRs group related commits. Six months from now, you can scan PRs to remember what you did.
3. **Muscle memory.** This is exactly how teams work. Doing it solo means you're already fluent when you join one.

---

## Part 8: Deploy your site to the internet

Vercel is the easiest free host and connects directly to GitHub.

1. Go to **vercel.com** and sign up with your GitHub account.
2. Click **Add New… → Project**.
3. Find `personal-site` in the list and click **Import**.
4. Leave all settings as default (Vercel auto-detects it's a static site).
5. Click **Deploy**. Wait ~30 seconds.

You get a URL like `personal-site-yourname.vercel.app`. That's your live site.

**The magic part:** Every time you push to `main` from now on, Vercel automatically redeploys. Every PR also gets a *preview URL* you can check before merging.

---

## Part 9: Hook up a custom domain (optional)

Your site lives at `personal-site-yourname.vercel.app` by default. That's fine, but if you want `yourname.com` instead, here's how. This part costs money — domains are usually $10–15/year.

### 9.1 Buy a domain

You buy domains from a *registrar*. Good options:

- **Cloudflare Registrar** (cloudflare.com/products/registrar) — sells domains at cost, no markup. Cheapest option, but you have to make a Cloudflare account.
- **Namecheap** (namecheap.com) — friendly UI, fair prices.
- **Porkbun** (porkbun.com) — cheap, good UX, no upselling.

Avoid **GoDaddy** — they're more expensive and aggressive with upsells.

Search for the name you want (e.g. `yourname.com`). If `.com` is taken or pricey, `.dev`, `.me`, `.io`, and `.xyz` are all reasonable alternatives. Buy it. You usually don't need any of the add-ons they push (privacy protection is often free now; you don't need their email or hosting since Vercel handles that).

### 9.2 Add the domain in Vercel

In Vercel, open your project → **Settings** (top tab) → **Domains** (left sidebar).

Type your domain (e.g. `yourname.com`) and click **Add**. Vercel asks if you want to also handle `www.yourname.com` — say yes, it'll redirect one to the other.

Vercel then shows you DNS records you need to add at your registrar. They look like:

```
Type: A      Name: @     Value: 76.76.21.21
Type: CNAME  Name: www   Value: cname.vercel-dns.com
```

(The exact values may differ — copy what Vercel shows you, not what's here.)

### 9.3 Add the DNS records at your registrar

DNS records are how the internet knows "when someone types `yourname.com`, send them to this server." Each registrar has a slightly different DNS settings page, but the steps are the same:

1. Log into your registrar.
2. Find your domain → look for **DNS settings** / **DNS records** / **Manage DNS**.
3. Delete or ignore any default "parking page" records.
4. Add the records Vercel gave you, one by one. The `Name` field is sometimes called "Host" — `@` means "the root domain itself" (i.e. `yourname.com`). `www` means the `www.yourname.com` subdomain.
5. Save.

### 9.4 Wait

DNS changes propagate across the internet. Usually 5–15 minutes, sometimes up to a few hours. Vercel's Domains page will show a green checkmark next to your domain when it's working.

Once it's live, Vercel automatically gets you a free HTTPS certificate (the padlock icon in the browser). You don't need to do anything for that.

### 9.5 Email at your domain (optional)

You probably want `you@yourname.com` to actually receive mail. Vercel doesn't do email — you need a separate service. Options:

- **Cloudflare Email Routing** (free) — forwards `you@yourname.com` to your Gmail. You can't *send* from it, only receive, but for a personal site this is often enough. Requires moving your domain's DNS to Cloudflare (free).
- **Google Workspace** ($7/month) — full inbox at your domain through Gmail's UI.
- **Fastmail** ($5/month) — independent, privacy-respecting alternative.

Skip this if you don't need it. You can always add it later.

---

## Part 10: Your daily workflow, summarized

Once everything is set up, your loop for any change looks like:

1. `cd ~/Projects/personal-site`
2. `git checkout main && git pull` — make sure you're starting from the latest.
3. `git checkout -b descriptive-branch-name` — new branch.
4. Open VS Code (`code .`), run `claude`, make your changes.
5. Preview in browser (Live Server).
6. `git add . && git commit -m "what you did"` — commit when something works.
7. `git push -u origin branch-name` — push to GitHub.
8. Open PR on GitHub, review the diff, merge.
9. `git checkout main && git pull` — sync local main.
10. `git branch -d branch-name` — cleanup.

Vercel auto-deploys after step 8. Your live site updates within a minute.

---

## Glossary

- **Branch:** A parallel line of development. You merge it back when done.
- **Commit:** A snapshot of your changes with a message.
- **Diff:** The line-by-line difference between two versions.
- **Merge:** Bringing changes from one branch into another.
- **Pull:** Download new commits from GitHub to your computer.
- **Pull Request (PR):** A proposal to merge a branch into another. Lets you review changes before merging.
- **Push:** Upload your local commits to GitHub.
- **Remote:** A copy of your repo on another server (GitHub).
- **Repository (repo):** A folder Git is tracking.
- **Staging:** Marking files as "to be included in the next commit" (`git add`).

---

## Common Git commands cheat sheet

```
git status                        # what's changed?
git add .                         # stage all changes
git commit -m "message"           # snapshot staged changes
git push                          # upload to GitHub
git pull                          # download from GitHub
git checkout -b new-branch        # create & switch to new branch
git checkout main                 # switch to main
git branch                        # list branches
git branch -d branch-name         # delete a branch (locally)
git log --oneline                 # short history
git diff                          # see what's changed but not staged
```

---

## When you get stuck

- **Claude Code is right there.** Quit any browser, open the VS Code terminal, run `claude`, and ask: "I ran `git push` and got this error: [paste]. What does it mean and how do I fix it?" It'll diagnose.
- **GitHub Docs:** docs.github.com has clean tutorials.
- **The Pro Git book** (free): git-scm.com/book — chapters 1–3 cover everything you'll need for years.

You don't need to memorize commands. You need to understand the *concepts* (commits, branches, remote) so you can recognize what you want to do. The exact syntax you can always look up or ask Claude for.
