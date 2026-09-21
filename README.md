# runtime_components

My personal configuration files ("rc files") for the shell, editor, debugger,
and git. Programs read these files when they start, so keeping them in one
repository means any machine I sit down at can be set up the same way in one
command.

## What's here

| File         | Read by | What it does |
|--------------|---------|--------------|
| `bashrc`     | bash    | History settings, a prompt showing the current git branch, and aliases (`ll`, `gccw`, safer `rm`/`cp`/`mv`) |
| `zshrc`      | zsh     | The same aliases as `bashrc`, plus [oh-my-zsh](https://ohmyz.sh) with the `robbyrussell` theme. Falls back to a plain prompt if oh-my-zsh isn't installed |
| `vimrc`      | vim     | Syntax highlighting, line numbers, 2-space indentation, a column-80 guide, better search. Keeps real tabs in Makefiles |
| `gdbinit`    | gdb     | Saved command history, readable struct printing, no confirmation prompts or paging |
| `gitconfig`  | git     | `main` as the default branch, colored output, and aliases (`git st`, `git lg`, `git unstage`) |
| `install.sh` |         | Copies everything above into place |

## Installing

```bash
git clone https://github.com/[username]/runtime_components.git
cd runtime_components
./install.sh --dry-run    # see what will happen first
./install.sh
```

Then open a new terminal.

The script:

- copies each file to your home directory;
- moves any existing file out of the way to `~/.NAME.backup-DATE-TIME` first, so nothing is ever deleted;
- copies `gitconfig` to `~/.gitconfig.rc` and adds one line to `~/.gitconfig` so git reads it. Your name and email stay in `~/.gitconfig` and never end up in this repository;
- installs oh-my-zsh if `zsh` is available, without changing your login shell.

It is safe to run more than once. Files that haven't changed are skipped.

| Option          | Effect |
|-----------------|--------|
| `--dry-run`, `-n` | Print what would happen, change nothing |
| `--no-omz`      | Skip installing oh-my-zsh |
| `--help`, `-h`  | Show usage |

If you get `Permission denied`, the script isn't executable. Run
`chmod +x install.sh` or use `bash install.sh`.

## Making changes

The installed files are **copies**. To change a setting:

1. Edit the file in this repository, not the copy in your home directory.
2. Run `./install.sh` again.
3. Commit the change.

If you edit `~/.vimrc` directly instead, the change never reaches the
repository, and the next install will move it to a backup.

## Notes

- **macOS uses zsh by default**, so `bashrc` is not read there; `zshrc` is.
- **Switching to zsh on Linux:** `chsh -s "$(command -v zsh)"`. This may not
  work on department machines where shells are managed centrally.
- **Indentation** in `vimrc` is set to 2 spaces. Change `tabstop`,
  `shiftwidth`, and `softtabstop` together if your code standard differs.
- **`gccw`** is `gcc -Wall -Wextra -g`. It is deliberately a new name, not a
  replacement for `gcc`, so you always know which flags you are compiling with.

## Undoing the install

Every replaced file was saved next to the original:

```bash
ls -a ~ | grep backup                       # find the backups
mv ~/.vimrc.backup-20260921-120000 ~/.vimrc # restore one
```

To stop git reading the shared config:

```bash
git config --global --unset include.path ~/.gitconfig.rc
```

## Credits

Structure inspired by [kameranis/My-rc-s](https://github.com/kameranis/My-rc-s).
Shell framework: [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh).
