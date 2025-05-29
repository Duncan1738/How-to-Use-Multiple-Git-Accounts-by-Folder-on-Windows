# How to Use Multiple Git Accounts by Folder on Windows

> Seamlessly manage personal and work GitHub accounts on the same Windows machine — no reconfiguration needed per repo.

![cover](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?ixlib=rb-4.0.3&auto=format&fit=crop&w=1470&q=80)

---

## Why This Matters

If you contribute to **personal projects**, **open-source**, and **company repositories**, you likely have **multiple GitHub accounts**.  
And you’ve probably hit a wall where Git commits the wrong email — or worse — GitHub rejects pushes because of wrong credentials.

### The typical "global config" approach:
```bash
git config --global user.name "Your Name"
git config --global user.email "you@personal.com"

❗ Common Pain Points on Windows
Having to manually set git user/email per repo

Constantly switching SSH keys or HTTPS tokens

GitHub shows commits under the wrong profile

Trouble using tools like Git Bash, CMD, and VS Code with multiple identities

✅ Goal
Set up Git to automatically switch users based on folder paths, like:
C:\Projects\Personal → uses personal GitHub credentials  
C:\Projects\Work     → uses work GitHub credentials
This works system-wide, even in Git Bash, CMD, Windows Terminal, and VS Code.
---
Step-by-Step Setup for Windows
1. Create a Consistent Folder Structure
 use this setup:
C:\Projects\
├── Personal\
└── Work\
-----
2. Create Git Config Files for Each Identity
In your user directory (C:\Users\YourUsername), create:

➕ .gitconfig-personal
[user]
    name = Your Personal Name
    email = yourname@personalmail.com

➕ .gitconfig-work
[user]
    name = Your Work Name
    email = yourname@company.com
---
3. Edit the Main .gitconfig in Your User Folder
Edit or create C:\Users\YourUsername\.gitconfig:
[user]
    name = Default User
    email = default@email.com

[includeIf "gitdir:C:/Projects/Personal/"]
    path = .gitconfig-personal

[includeIf "gitdir:C:/Projects/Work/"]
    path = .gitconfig-work
📌 Use / in paths (even on Windows), and ensure the folder ends with /.

4. Create and Test Repositories
Personal
cd C:\Projects\Personal
mkdir test-personal && cd test-personal
git init
git config user.name   # Should show your personal name
git config user.email  # Should show your personal email
---
Work
cd C:\Projects\Work
mkdir test-work && cd test-work
git init
git config user.name   # Should show your work name
git config user.email  # Should show your work email
✅ You’re now committing using the correct account automatically!
---
Bonus: SSH Key Setup for Multiple GitHub Accounts
If you're using SSH authentication:

1. Generate separate SSH keys
ssh-keygen -t ed25519 -C "personal" -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "work" -f ~/.ssh/id_ed25519_work
---
2. Edit SSH config file ~/.ssh/config
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
---
3. Use the correct host when cloning
# Personal
git clone git@github.com-personal:yourusername/project.git

# Work
git clone git@github.com-work:yourcompany/project.git

🎯 Why This Works So Well
No need to configure every new repository

Easily switch context by simply working inside the correct folder

One change to .gitconfig-personal or .gitconfig-work updates all related repos

Compatible with VS Code, JetBrains IDEs, Windows Terminal, and GitHub CLI

📌 Use Cases
🧠 Developers juggling multiple GitHub accounts

🏢 Consultants working for multiple clients

🎓 Students switching between personal projects and university code

⚙️ Teams with central GitHub orgs and individual OSS contributions


Summary

Problem	Solution
Wrong GitHub email in commits	Folder-based user config
Too many SSH keys, frequent switching	SSH config with separate identities
Manual git config per repo	Automatic per-directory configuration
Global config doesn't scale	Modular .gitconfig setup

🧪 TL;DR – Config Blueprint

# ~/.gitconfig
[user]
    name = Your Name
    email = default@email.com

[includeIf "gitdir:C:/Projects/Personal/"]
    path = .gitconfig-personal

[includeIf "gitdir:C:/Projects/Work/"]
    path = .gitconfig-work



👤 Author
Duncan Kibet





