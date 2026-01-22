---
date: 2025-12-16
---
# The new cheatsheet

> **Description**: 

#tags

---

## 📋 **Table of Contents**

- [INTRO](#intro) - Intro
- [CONTENT](#CONTENT) - content  
- [CONCLUSION](#Conlusion) - Conclusion

---

### **1. Générer une clé SSH**


``` bash
ssh-keygen -t ed25519 -C "<email>"
```

**At the prompt**:
- Press **Enter** to accept the default location (`~/.ssh/id_ed25519`) or chose a custom name.
- Choose a **passphrase** (or leave empty).

### **2. Start the SSH Agent and Add the Key**

- Start the agent 
```bash
eval "$(ssh-agent -s)" 
```
-  Add the key (no sudo!)
```bash 
ssh-add ~/.ssh/id_ed25519  
```

### **3. Display and Copy the Public Key**

```bash
cat ~/.ssh/id_ed25519.pub
```

- **Copy the entire output** (e.g., `ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAA...`).

### **4. Add the Public Key to GitHub**

1. Go to [GitHub → Settings → SSH and GPG keys](https://github.com/settings/keys).
2. Click **New SSH key**.
3. Give it a title (e.g., "WSL Ubuntu").
4. Paste your public key.
5. Click **Add SSH key**.

### **5. Test the SSH Connection**

```bash
ssh -T git@github.com
```

### **6. Change a Git Repository URL to Use SSH**

If your repo is using HTTPS:

```bash
git remote set-url origin git@github.com:your_username/your_repo.git
```

---

### **7. (Optional) Create an SSH `config` File**

```bash
nano ~/.ssh/config
```

Add:
```bash
Host github.com   
	HostName github.com  
	User git  
	IdentityFile ~/.ssh/id_ed25519
```

- Save with `Ctrl+O`, then `Ctrl+X`.

