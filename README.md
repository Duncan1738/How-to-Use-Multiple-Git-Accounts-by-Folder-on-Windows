# How to Use Multiple Git Accounts by Folder on Windows

> Seamlessly manage personal and work GitHub accounts on the same Windows machine — no reconfiguration needed per repo.

![cover](https://www.google.com/imgres?q=multiple%20git%20accounts&imgurl=https%3A%2F%2Fguisantos.dev%2Fapi%2Fog%3Ftitle%3DHow%2520to%2520use%2520multiple%2520git%2520accounts%2520on%2520the%2520same%2520machine%26date%3DJune%252023%2C%25202023%26width%3D1200%26height%3D630&imgrefurl=https%3A%2F%2Fwww.guisantos.dev%2Fblog%2Fhow-to-use-multiple-git-accounts&docid=1gbhv-l5rDpz-M&tbnid=UfsqevCACpKAkM&vet=12ahUKEwiOp_Ho8eOOAxU3_8kDHaKOBhIQM3oECHIQAA..i&w=1200&h=630&hcb=2&ved=2ahUKEwiOp_Ho8eOOAxU3_8kDHaKOBhIQM3oECHIQAA)

---

##  Why This Matters

If you contribute to **personal projects**, **open-source**, and **company repositories**, you likely have **multiple GitHub accounts**.  
You’ve probably run into problems like:

- Git committing with the wrong email
- GitHub rejecting pushes due to incorrect credentials
- Commits showing under the wrong GitHub profile

This guide helps you configure Git **once**, so it automatically uses the right credentials based on the **folder you're in**.

---

## 🛠️ Step-by-Step Setup for Windows

### 1. Create a Consistent Folder Structure

```bash
C:\Projects\
├── Personal\
└── Work\
```

Make sure you always clone personal projects into `C:\Projects\Personal\`  
and work-related projects into `C:\Projects\Work\`.

---

### 2. Create Git Config Files for Each Identity

In your home directory (`C:\Users\YourUsername`), create:

#### `.gitconfig-personal`
```ini
[user]
    name = Your Personal Name
    email = personal@example.com
```

#### `.gitconfig-work`
```ini
[user]
    name = Your Work Name
    email = work@example.com
```

---

### 3. Edit the Main `.gitconfig` in Your User Folder

Edit `C:\Users\YourUsername\.gitconfig`:

```ini
[user]
    name = Default User
    email = default@example.com

[includeIf "gitdir:C:/Projects/Personal/"]
    path = .gitconfig-personal

[includeIf "gitdir:C:/Projects/Work/"]
    path = .gitconfig-work
```

📌 Important:
- Use **forward slashes** (`/`) in paths even on Windows.
- End each folder path with a **trailing slash** (`/`).

---

### 4. Test Your Setup

#### Personal:
```bash
cd C:\Projects\Personal
mkdir test-personal && cd test-personal
git init
git config user.name    # Should return "Your Personal Name"
git config user.email   # Should return "personal@example.com"
```

#### Work:
```bash
cd C:\Projects\Work
mkdir test-work && cd test-work
git init
git config user.name    # Should return "Your Work Name"
git config user.email   # Should return "work@example.com"
```

✅ You’re now committing using the correct GitHub identity automatically!

---

## 🔐 Bonus: SSH Key Setup for Multiple GitHub Accounts

If you use **SSH authentication**, do the following:

### 1. Generate Separate SSH Keys

```bash
ssh-keygen -t ed25519 -C "personal" -f ~/.ssh/id_ed25519_personal
ssh-keygen -t ed25519 -C "work" -f ~/.ssh/id_ed25519_work
```

### 2. Configure Your SSH `config` File

Create or edit `~/.ssh/config`:

```ini
# Personal GitHub
Host github.com-personal
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

# Work GitHub
Host github.com-work
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
```

---

### 3. Use the Correct Host When Cloning

```bash
# Personal
git clone git@github.com-personal:yourusername/project.git

# Work
git clone git@github.com-work:yourcompany/project.git
```

---

## 🧠 Why This Works So Well

✅ No need to configure each repo individually  
✅ Easily switch context by changing folders  
✅ Works in Git Bash, CMD, VS Code, Windows Terminal, etc.  
✅ Keeps work and personal identities clean and separate  

---

## 🧰 Use Cases

- Developers juggling **multiple GitHub accounts**
- Consultants working with **multiple clients**
- Students separating **personal and school work**
- OSS contributors and professional developers

---

## 📦 Summary

| Problem | Solution |
|--------|----------|
| Wrong GitHub identity | Folder-based Git config |
| Switching SSH keys constantly | Host aliasing in SSH config |
| Repeating config for every repo | Global includeIf setup |
| Global Git config issues | Modular `.gitconfig` files |

---

## 🧪 TL;DR – `.gitconfig` Blueprint

```ini
# ~/.gitconfig
[user]
    name = Your Name
    email = default@example.com

[includeIf "gitdir:C:/Projects/Personal/"]
    path = .gitconfig-personal

[includeIf "gitdir:C:/Projects/Work/"]
    path = .gitconfig-work
```

🔧 Now you can manage multiple GitHub accounts without frustration — like a pro.
