# 🔒 Ultimate Guide to GPG Keys for Git & GitHub

This **ultimate guide** walks you through generating a GPG key on Linux, linking it to GitHub, and signing commits to earn that shiny **Verified** badge. With _detailed steps_, __essential commands__, and ~~stylish~~ Markdown formatting, you'll be a GPG pro in no time! 🚀

---

## 🌟 Why Use GPG with GitHub?

GPG keys cryptographically sign your commits, proving **you're the real deal**. Here's why it's awesome:

- **Authenticity**: Prove *you* authored the commit, not some impostor. 🔐
- **Trust**: Get that green **Verified** badge on GitHub. ✅
- **Security**: Protect collaborative projects from tampering. 🛡️
- **Professional cred**: Show you mean business with secure workflows. 💼

---

## 🛠️ Prerequisites

Before diving in, ensure you have:

- **Git**: Check with `git --version`.
- **GPG**: Check with `gpg --version`.
- A **GitHub account** with a verified email ([check here](https://github.com/settings/emails)).
- A Linux terminal (Ubuntu or similar).

Install GPG if missing:
```bash
sudo apt update && sudo apt install gnupg
```

---

## 📝 Step 1: Generate Your GPG Key

Let's create a **modern**, secure GPG key using ECC (Elliptic Curve Cryptography).

1. **Kick off key generation**:
   ```bash
   gpg --full-generate-key
   ```

2. **Select key type**:
   - Choose `9` for **ECC (sign and encrypt)** — it's __faster__ and ~~more secure~~ than RSA.
   - Hit `Enter`.

3. **Pick the curve**:
   - Select `1` for **Curve 25519** (Ed25519) — the _gold standard_ for signing.
   - Press `Enter`.

4. **Set expiration**:
   - Enter `1y` for 1 year or `0` for no expiration (not recommended).  
     *Pro tip*: Expiring keys are safer; you can extend later.
   - Press `Enter`.

5. **Add user details**:
   - **Name**: Your name or GitHub username.
   - **Email**: Use the __exact email__ tied to your GitHub account.
   - **Comment**: Optional (leave blank for simplicity).
   - Confirm with `O` (Okay).

6. **Choose a passphrase**:
   - Set a **strong passphrase** to lock your private key.  
     *Example*: Use a password manager to store it securely.
   - Confirm it.

7. **Generate the key**:
   - GPG creates your public and private key pair. This takes a few seconds. ⏳

---

## 🔍 Step 2: Find Your GPG Key ID

You need the key ID to configure Git and GitHub.

1. **List secret keys**:
   ```bash
   gpg --list-secret-keys --keyid-format=long
   ```

2. **Spot your key**:
   Look for something like:
   ```
   sec   ed25519/AB1234567890CDEF 2025-05-18 [SC]
         Key fingerprint = 1234 5678 90AB CDEF 1234 5678 90AB CDEF 1234 5678
   uid                 Your Name <you@example.com>
   ssb   cv25519/1234567890ABCDEF 2025-05-18 [E]
   ```

   - The key ID is `AB1234567890CDEF` (after `ed25519/`).
   - Copy it or jot it down.

---

## 🔑 Step 3: Export Your Public Key

GitHub needs your public key to verify your signed commits.

1. **Export in ASCII format**:
   ```bash
   gpg --armor --export AB1234567890CDEF
   ```

2. **Copy the output**:
   You'll see:
   ```
   -----BEGIN PGP PUBLIC KEY BLOCK-----
   mQENBF...
   -----END PGP PUBLIC KEY BLOCK-----
   ```

   Copy the _entire block_, including `-----BEGIN` and `-----END`.

3. **(Optional) Save to file**:
   Back it up with:
   ```bash
   gpg --armor --export AB1234567890CDEF > mypublickey.asc
   ```

---

## 🌍 Step 4: Add Public Key to GitHub

1. **Head to GitHub**:
   - Visit [Settings > SSH and GPG keys](https://github.com/settings/keys).
   - Click **New GPG key** or **Add GPG key**.

2. **Paste the key**:
   - Drop the copied public key block into the text box.

3. **Save it**:
   - Hit **Add key**.  
     GitHub will now recognize your signed commits. 🎉

---

## ⚙️ Step 5: Configure Git for Signing

Set up Git to sign all commits with your GPG key.

1. **Link your key to Git**:
   ```bash
   git config --global user.signingkey AB1234567890CDEF
   ```

2. **Enable auto-signing**:
   ```bash
   git config --global commit.gpgsign true
   ```

3. **Check your setup**:
   Verify with:
   ```bash
   git config --global --list
   ```
   Look for:
   ```
   user.signingkey=AB1234567890CDEF
   commit.gpgsign=true
   ```

4. **(Optional) Set Git user details**:
   Match your Git config to your GitHub email and GPG key:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "you@example.com"
   ```

---

## 🧪 Step 6: Test Your Signed Commits

Time to test your setup with a sample repo!

1. **Create a test repo**:
   ```bash
   mkdir test-gpg && cd test-gpg
   git init
   echo "# Test GPG Signing" > README.md
   git add README.md
   git commit -m "My first signed commit 🎉"
   ```

2. **Verify the signature locally**:
   ```bash
   git log --show-signature -1
   ```

   You should see:
   ```
   commit abc123...
   gpg: Signature made Sun May 18 12:51:00 2025 +0600
   gpg:                using ED25519 key AB1234567890CDEF
   gpg: Good signature from "Your Name <you@example.com>" [ultimate]
   ```

3. **Push to GitHub**:
   - Create a new repo on GitHub (don’t initialize with a README).
   - Link and push:
     ```bash
     git remote add origin https://github.com/yourusername/test-gpg.git
     git branch -M main
     git push -u origin main
     ```

4. **Check GitHub**:
   - Go to your repo’s **Commits** tab.
   - Your commit should sport a **Verified** badge. 🥳

---

## 🛡️ Step 7: Backup & Manage Keys

Keep your keys safe and ready for future use.

1. **Backup private key**:
   Export it securely:
   ```bash
   gpg --export-secret-keys --armor AB1234567890CDEF > myprivatekey.asc
   ```
   *Warning*: Store this file in a __secure location__.

2. **Import on another device**:
   ```bash
   gpg --import myprivatekey.asc
   ```

3. **Extend key expiration**:
   If your key is expiring:
   ```bash
   gpg --edit-key AB1234567890CDEF
   ```
   At `gpg>` prompt:
   ```
   expire
   ```
   Set a new date, then:
   ```
   save
   ```
   Update GitHub with the new public key:
   ```bash
   gpg --armor --export AB1234567890CDEF
   ```

4. **Revoke a key** (if compromised):
   ```bash
   gpg --generate-revocation AB1234567890CDEF > revoke.asc
   ```
   Import if needed:
   ```bash
   gpg --import revoke.asc
   ```

---

## 🐛 Troubleshooting

- **"gpg: signing failed: No secret key"**:
  - Double-check key ID: `gpg --list-secret-keys --keyid-format=long`.
  - Verify Git config: `git config --global user.signingkey`.

- **Commits not verified on GitHub**:
  - Ensure Git email matches GitHub email: `git config --global user.email`.
  - Confirm public key is added to GitHub.

- **Passphrase prompts annoying?**:
  Install `gpg-agent`:
  ```bash
  sudo apt install gpg-agent
  ```

- **Still stuck?**:
  - Read [GitHub Docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits).
  - Ask in [GitHub Community](https://github.community).

---

## 💡 Tips for Pros

- **Multiple devices?** Generate a new key per device or securely transfer your private key.
- **Key safety**: __Never share__ your private key. Use a password manager for passphrases.
- **Expiration strategy**: Set 1-2 year expirations and extend as needed.
- **Automate backups**: Script key exports to a secure cloud (encrypted, of course).
- **Explore GPG**: Use it for signing emails or encrypting files! 🔒

---

## 📋 Command Cheat Sheet

| Task                     | Command                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------|
| Install GPG              | `sudo apt update && sudo apt install gnupg`                                               |
| Generate key             | `gpg --full-generate-key`                                                                 |
| List keys                | `gpg --list-secret-keys --keyid-format=long`                                              |
| Export public key        | `gpg --armor --export YOUR_KEY_ID`                                                        |
| Export private key       | `gpg --export-secret-keys --armor YOUR_KEY_ID > myprivatekey.asc`                         |
| Configure Git            | `git config --global user.signingkey YOUR_KEY_ID`<br>`git config --global commit.gpgsign true` |
| Set Git user             | `git config --global user.name "Your Name"`<br>`git config --global user.email "you@example.com"` |
| Test commit              | `git commit -m "My signed commit"`                                                        |
| Verify commit            | `git log --show-signature -1`                                                             |
| Extend expiration        | `gpg --edit-key YOUR_KEY_ID` then `expire`                                                |
| Revoke key               | `gpg --generate-revocation YOUR_KEY_ID > revoke.asc`                                      |

---

## 🎉 Wrap-Up

You're now a **GPG master**! Your commits will glow with **Verified** badges, and your workflow is secure as Fort Knox. Keep rocking Git, and explore more GPG tricks for emails or file encryption.  

*— Your Git/GPG Sidekick*  
Happy coding! ✨
