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

## Task 2

### Pre-commit configuration (`.pre-commit-config.yaml`)

```yaml
repos:
  - repo: https://github.com
    rev: v8.24.2
    hooks:
      - id: gitleaks
  - repo: https://github.com
    rev: v5.0.0
    hooks:
      - id: detect-private-key
      - id: check-added-large-files
```

### Blocked commit output & verification (3.5)

```text
Detect hardcoded secrets.................................................Failed
- hook id: gitleaks
- exit code: 1

Finding:     GH_PAT=ghp_16C7e42F292c6912E7710c838347Ae178B4a # gitleaks:allow
Secret:      ghp_16C7e42F292c6912E7710c838347Ae178B4a # gitleaks:allow
RuleID:      github-pat
File:        submissions/leak-attempt.txt
Line:        1

5:42PM INF 1 commits scanned.
5:42PM INF scanned ~48 bytes (48 bytes) in 28.6ms
5:42PM WRN leaks found: 1

detect private key.......................................................Passed
check for added large files..............................................Passed
```

Verification after cleanup (`git log --oneline -1`):
- `014cdc3 docs(lab3): record Task 1 SSH commit signing verification` (confirms the blocked commit was never recorded in history).

### Analysis: Specific Allowlist vs. Folder Exclusion
- **Specific allowlist entry in `.gitleaks.toml`:** Permits only an exact known placeholder string or fingerprint while keeping secret scanning active across the rest of the file and repository. It stops being safe if the allowlist regex is written too broadly (e.g., matching any `AKIA...` prefix), which would let a real AWS access key slip through.
- **Path exclusion for `docs/`:** Disables secret detection entirely for every file under `docs/`, meaning `gitleaks` will never inspect changes in that directory. It stops being safe the moment a developer pastes real credentials, tokens, or connection strings into a documentation example or runbook inside `docs/`.
