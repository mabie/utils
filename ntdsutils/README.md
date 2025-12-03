# NTDS Utils

Collection of small bash helper scripts for querying NTDS files.

![ntuserfind](assets/ntdsutils.gif)

Includes:

- `nthash`
- `ntdsgrep` / `ntg`
- `ntuserfind` / `ntuf`

Requirements:

- [`ag` (the silver searcher)](https://github.com/ggreer/the_silver_searcher)
- [`fzf`](https://github.com/junegunn/fzf) (required by `ntuserfind` when multiple matches)
- `iconv` + `openssl` (preferred by `nthash`); [`pypykatz`](https://github.com/skelsec/pypykatz) as a fallback

## Installation

```bash
mkdir -p ~/.local/bin
curl -sSL https://raw.githubusercontent.com/mabie/utils/refs/heads/main/ntdsutils/ntdsgrep -o ~/.local/bin/ntdsgrep
chmod +x ~/.local/bin/ntdsgrep
ln -sf ~/.local/bin/ntdsgrep ~/.local/bin/ntg
curl -sSL https://raw.githubusercontent.com/mabie/utils/refs/heads/main/ntdsutils/nthash -o ~/.local/bin/nthash
chmod +x ~/.local/bin/nthash
curl -sSL https://raw.githubusercontent.com/mabie/utils/refs/heads/main/ntdsutils/ntuserfind -o ~/.local/bin/ntuserfind
chmod +x ~/.local/bin/ntuserfind
ln -sf ~/.local/bin/ntuserfind ~/.local/bin/ntuf
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## Usage Examples

### nthash

```bash
nthash 'Passw0rd'
echo 'Passw0rd' | nthash
```

### ntdsgrep / ntg

Search for an NT hash derived from a cleartext password within NTDS dumps.

```bash
ntg 'Passw0rd'                     # Search in ./*.ntds and results/dcsync/*.ntds
ntg 'Passw0rd' /path/to/dcsync     # Directory (recursively) or file
cat contoso.local.ntds | ntg 'Passw0rd'
ntg $(wl-paste)                    # Using Wayland clipboard contents
```

- Uses `nthash` (prefers `iconv`+`openssl`, falls back to `pypykatz`) and `ag`.
- Supports stdin, single file, or recursive directory search; defaults to current dir plus `dcsync/`.

### ntuserfind / ntuf

Find usernames in NTDS dumps, extract their NT hashes, and report cracked passwords from potfiles.

```bash
ntuf exadmin                       # Find user in ./*.ntds and dcsync/*.ntds, select with fzf, print NT hash
ntuf exadmin /path/to/dcsync       # Directory (recursive) or file; checks nearby .pot files for cracked passwords
cat contoso.local.ntds | ntuf ex   # Piped input; use -p /path/to/pot to check a specific potfile
```

- Case-insensitive search via `ag`; same file discovery rules as `ntdsgrep`.
- Requires `fzf` for selection; multi-select supported (tab to pick multiple accounts).
- Outputs NT hashes to stdout; status and cracked notices (with passwords) go to stderr. Honors `NO_COLOR`.
- Potfile resolution: `-p <file>` override, otherwise all `.pot` files alongside each `.ntds` (including `<file>.pot`).
