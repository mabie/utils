# **NTDSGrep**

A lightweight utility to quickly convert cleartext passwords to NT hashes and search them within **DCSync NTDS dump files**.

Supports:

- Searching `.ntds` files in **current directory** and `results/dcsync/`
- **Recursive directory search**
- **Single file input**
- **Piped NTDS data**

Powered by:

- [`pypykatz`](https://github.com/skelsec/pypykatz)
- [`the_silver_searcher (ag)`](https://github.com/ggreer/the_silver_searcher)

---

## **Installation**

```bash
mkdir -p ~/.local/bin
curl -sSL https://raw.githubusercontent.com/mabie/utils/refs/heads/main/ntdsgrep/ntdsgrep -o ~/.local/bin/ntdsgrep
chmod +x ~/.local/bin/ntdsgrep
ln -sf ~/.local/bin/ntdsgrep ~/.local/bin/ntg
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## **Usage Examples**

```bash
ntg 'Passw0rd'                     # Search in ./*.ntds and results/dcsync/*.ntds
ntg 'Passw0rd' /path/to/dcsync     # Directory (recursively) or file
cat contoso.local.ntds | ntg 'Passw0rd'
ntg $(wl-paste)                    # Using Wayland clipboard contents
```
