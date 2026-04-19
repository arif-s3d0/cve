# CVE-2025-8110 — Gogs Symlink RCE

A proof-of-concept exploit for a symlink path-traversal vulnerability in Gogs ≤ 0.13.3 that allows an authenticated user to achieve remote code execution on the host.

## How it works

1. Authenticates to a Gogs instance using provided credentials
2. Creates a repository and pushes a symlink named `authorized_keys` pointing to the target OS user's `~/.ssh/authorized_keys`
3. Uses the Gogs file contents API to write an attacker-controlled SSH public key through the symlink
4. Saves the matching private key locally and prints the SSH command to connect

## Usage

```
python3 gogs-exploit.py --url http://<target> --user <username> --pass <password> [--git-user <os-user>]
```

### Options

| Flag | Description | Default |
|------|-------------|---------|
| `--url` | Gogs base URL | required |
| `--user` | Gogs username | required |
| `--pass` | Gogs password | required |
| `--git-user` | OS user running Gogs | `root` |
| `--repo` | Base name for the created repo | `pwn_repo` |
| `--burp` | Proxy traffic through Burp Suite | off |
| `--burp-host` | Burp host | `127.0.0.1` |
| `--burp-port` | Burp port | `8080` |

## Requirements

- Python 3
- `requests` library
- `git` and `ssh-keygen` available on the system

## Disclaimer

For authorized penetration testing and CTF use only.
