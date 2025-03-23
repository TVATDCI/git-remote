# GIT-REMOTE (extended version)

# # Setting Up Multiple GitHub Accounts on One Laptop

This repository provides a step-by-step guide on how to set up and manage multiple GitHub accounts on a single laptop using SSH authentication. It is designed to help developers, students, and professionals who need to work with multiple GitHub accounts seamlessly.

## 1️⃣ **Adding a Second GitHub Account to This Laptop**

### **Step 1: Generate a New SSH Key for the Second GitHub Account**

```sh
ssh-keygen -t rsa -b 4096 -C "your-second-email@example.com"
```

- Save it as `~/.ssh/id_rsa_second`
- When prompted, enter a passphrase (or leave empty for no passphrase).

### **Step 2: Add the New SSH Key to the SSH Agent**

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa_second
```

### **Step 3: Add the SSH Key to GitHub**

- Copy the public key:

```sh
cat ~/.ssh/id_rsa_second.pub
```

- Go to **GitHub → Settings → SSH and GPG keys → New SSH Key**
- Paste the copied key and save.

### **Step 4: Configure SSH for Multiple GitHub Accounts**

Edit (or create) the SSH config file:

```sh
nano ~/.ssh/config
```

Add the following lines:

```sh
# First GitHub Account
Host github.com-first
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_first

# Second GitHub Account
Host github.com-second
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_second
```

Save and exit (`Ctrl + X`, then `Y`, then `Enter`).

### **Step 5: Test the SSH Connection**

```sh
ssh -T git@github.com-second
```

✅ Expected output:

```sh
Hi YourUsername! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 2️⃣ **Cloning a Repository from the Second GitHub Account**

```sh
git clone git@github.com-second:YourSecondUsername/YourRepo.git
```

- Replace `YourSecondUsername` and `YourRepo` with your actual GitHub username and repo name.

### **Verify Remote URL**

```sh
cd YourRepo
git remote -v
```

✅ Expected output:

```sh
origin  git@github.com-second:YourSecondUsername/YourRepo.git (fetch)
origin  git@github.com-second:YourSecondUsername/YourRepo.git (push)
```

---

## 3️⃣ **Setting Local Git Config for the Second Account**

Since the global config still points to the first account, we need to configure Git locally for this specific repo.

```sh
git config user.name "YourSecondUsername"
git config user.email "your-second-email@example.com"
```

### **Verify Local Git Config**

```sh
git config --list --local
```

✅ Expected output:

```sh
user.name=YourSecondUsername
user.email=your-second-email@example.com
```

---

## 4️⃣ **Testing the Setup with a Commit**

```sh
echo "Testing new setup for second GitHub account" >> test-second.txt
git add test-second.txt
git commit -m "Test commit for second GitHub account"
git push origin main
```

✅ If successful, you should see the commit appear on GitHub!

---

## 🎯 **Final Notes**

- Now, you can **seamlessly switch between accounts** by navigating to the appropriate repo.
- All future commits in the second account's repo will use the second GitHub account.
- To switch accounts, just navigate between different repositories configured for each account.

---

🎉 **You're now set up to use multiple GitHub accounts on one laptop!** 🚀
