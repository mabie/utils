# Ding

Run commands in the background and receive a desktop notification when they finish.

Requirements:

- [`notify-send`](https://gitlab.gnome.org/GNOME/libnotify) from libnotify

## Installation

```bash
mkdir -p ~/.local/bin
curl -sSL https://raw.githubusercontent.com/mabie/utils/refs/heads/main/ding/ding -o ~/.local/bin/ding
chmod +x ~/.local/bin/ding
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## Usage Examples

### ding

Run a command and notify when it exits:

```bash
ding nmap 192.168.0.0/16
ding sleep 300
ding bash -c 'some command; another command'
```

`ding` returns immediately after starting the command. It disowns the
background job, so the command continues running after the wrapper exits.
