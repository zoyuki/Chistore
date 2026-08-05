# chistore

The CLI package manager for user terminal utilities. It is written in pure Bash. It's self-updating, cross-platform, and designed not to get in your way.

Ships with `envsnap` — a lightweight system/environment state tracker with diff support.

# Repository Mirrors:

[MansionGIT](https://git.inthemansion.com/moyunni/chistore),
[Github](https://github.com/zoyuki/Chistore)

**Updates are released faster on codeberg. 10 minutes after writing the repository to codeberg, the changes will go to the mirrors.**

## Install

```bash
curl -fsSL "https://github.com/zoyuki/Chistore/raw/branch/main/chistore" -o ~/.local/bin/chistore
chmod +x ~/.local/bin/chistore
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc  # or ~/.zshrc
source ~/.bashrc
```

No root required. Installs to `~/.local/bin` (XDG standard).

## Usage

| Flag       | Action                                  |
|------------|-----------------------------------------|
| `-h`       | Show help                               |
| `-Ss <q>`  | Search available packages               |
| `-S <pkg>` | Install package                         |
| `-Rn <pkg>`| Remove (keep configs)                   |
| `-Rns <pkg>`| Remove completely (purge configs)      |
| `-Qet`     | List installed packages                 |
| `-Qs <q>`  | Search installed packages               |
| `-Syu`     | Check & install updates for all installed packages|

Examples:
```bash
chistore -Ss env
chistore -S envsnap
chistore -Qet
chistore -Rns envsnap
```

## Auto-Update

Runs on every execution. Compares local `VERSION` with the remote file. If outdated:
1. Downloads new binary to a temp file in the same directory
2. Atomically replaces the current script via `mv`
3. Restarts the process with `exec "$0"` using the updated file
4. Sets `_CH_UPDATED=1` to prevent recursive checks

Safe for active sessions. No state loss. Works across Linux, macOS, BSD, and WSL.

### Updating installed packages

It's simple: use `chistore -Syu`. If necessary, specify the name of the packages to exclude updates (in order not to update) or `n` to cancel the update. Press Enter to install

## Uninstall

Remove manager only:
```bash
rm -f ~/.local/bin/chistore
rm -rf ~/.local/share/chistore
```

Remove manager + all installed tools:
1. Run `chistore -Qet` to list installed packages
2. Remove each with `chistore -Rns <package>`
3. Clean up manager files:
```bash
rm -f ~/.local/bin/chistore
rm -rf ~/.local/share/chistore
```

## Repository Structure

```
repo/
├── VERSION          # Current manager version
├── chistore         # Manager script
├── INDEX            # Package names (one per line)
└── <package>/
    ├── info.md      # Metadata (Name, Description, Version, Size, Type, MainFile)
    └── <package>    # Executable (chmod +x)
```

## Notes

- Requires `bash`, `curl`, `diff`, `less` (standard on all POSIX systems)
- No external dependencies, no package managers, no Python
- All paths follow XDG Base Directory spec
- Contributions: PRs welcome. Keep it minimal.
