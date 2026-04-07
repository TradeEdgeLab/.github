# SSH Commit Signing Setup

## One-Time Configuration

```bash
# Tell git to use SSH for signing
git config --global gpg.format ssh

# Point to your SSH key (adjust path if needed)
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Enable signing for all commits
git config --global commit.gpgsign true

# Enable signing for all tags
git config --global tag.gpgsign true
```

## GitHub Setup

1. Go to **Settings → SSH and GPG keys → New SSH key**.
2. Set **Key type** to **Signing Key**.
3. Paste the contents of your public key (`~/.ssh/id_ed25519.pub`).

## Verify It Works

```bash
# Make a test commit — should show "Verified" on GitHub
git commit --allow-empty -m "test: verify signed commits"
git log --show-signature -1
```

## Allowed Signers (Optional, for Local Verification)

```bash
# Create an allowed_signers file
echo "$(git config user.email) $(cat ~/.ssh/id_ed25519.pub)" > ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```
