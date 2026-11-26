# Lab 3 — Secure Git

## Task 1

### Git signing configuration (3.1)
- `gpg.format`: `ssh`
- `user.signingkey`: `/home/codespace/.ssh/id_ed25519.pub`
- `commit.gpgsign`: `true`

### Signature verification output (3.3)

```text
commit 168292908f017e7dcdc6cdbd9bd498d1aee5ae37 (HEAD -> feature/lab3, origin/feature/lab3)
Good "git" signature for namespaces=git with ED25519 key SHA256:Gf9T/Fu6PTMJfDnEUW2eA+F1J5lcfSg7OBzIIXZudL0
Author: remuscostache-star <remus.costache@gmail.com>
Date:   Thu Nov 26 22:59:30 2026 +0000

    test: first signed commit
```

- **Verified commit on GitHub:** https://github.com

### Analysis (Spoofing & Repudiation)
Without cryptographic signing, Git trusts the `user.name` and `user.email` configured locally on any machine, allowing an attacker to spoof a teammate's identity and inject malicious code under their name. The `Verified` badge proves mathematically via asymmetric SSH keys that the commit was signed by the holder of the corresponding private key, providing authenticity and non-repudiation.
