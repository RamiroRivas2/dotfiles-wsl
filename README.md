# dotfiles-wsl

The **Linux half** of my dotfiles - manages my WSL Ubuntu with real
Nix + [home-manager](https://github.com/nix-community/home-manager),
following [kunchenguid/dotfiles](https://github.com/kunchenguid/dotfiles)
from [the video](https://youtu.be/5N-okeDdIuI) as closely as Linux allows.

The **Windows half** (WezTerm, fonts, PowerShell, winget tools) lives in
[RamiroRivas2/dotfiles](https://github.com/RamiroRivas2/dotfiles) - Nix
can't manage Windows, so that repo uses winget + PowerShell instead.

## How this maps to his repo

| His file | This repo |
|---|---|
| `flake.nix` (nixpkgs + nix-darwin + home-manager) | `flake.nix` (nixpkgs + home-manager only) |
| `configuration.nix` (macOS settings, Homebrew) | not needed - no system layer to manage in WSL |
| `home.nix` | `home.nix`, ported almost line for line |
| `bootstrap.sh` (installs Nix, first switch) | `bootstrap.sh` (Nix already installed, first switch) |
| `rebuild.sh` → `darwin-rebuild switch` | `rebuild.sh` → `home-manager switch` |
| `home/` configs symlinked via `~/.dotfiles` | same trick, same path |

## Fresh-machine setup

Inside WSL Ubuntu, with Nix installed
([Determinate installer](https://install.determinate.systems)):

```sh
git clone https://github.com/RamiroRivas2/dotfiles-wsl.git ~/github/dotfiles-wsl
cd ~/github/dotfiles-wsl
./bootstrap.sh
```

Then make zsh the login shell (one-time, asks for your password):

```sh
command -v zsh | sudo tee -a /etc/shells && chsh -s "$(command -v zsh)"
```

Open a new WSL session and you're in zsh with the starship prompt.

## Daily use

Edit the config files in place, then apply:

```sh
./rebuild.sh
```

That's it - same workflow as the video. `home/nvim`, `home/tmux.conf`, and
`home/AGENTS.md` are symlinked (edits are live, no rebuild needed); package
lists and program settings in `home.nix` need a rebuild.

## What's in it

- **Packages** (all via Nix): ripgrep, fd, fzf, jq, lazygit, Neovim
- **zsh**: ghost-text autosuggestions (Ctrl+F accepts), syntax highlighting,
  his aliases (`..`, `add`, `push`, `pull`, `m`, `cc`, `co` - the last two
  skip agent permission prompts, know what they do)
- **starship**: his exact minimal prompt
- **Neovim**: same config as the Windows repo (lazy.nvim, oil, snacks, neogit)
- **tmux**: my existing `~/.tmux.conf`, now version-controlled
- **AGENTS.md**: shared agent policy, linked to `~/.claude/CLAUDE.md`

Fonts are intentionally absent: WSL text renders in WezTerm on the Windows
side, so Hack Nerd Font is installed by the Windows repo.
