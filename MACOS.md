## Git Configuration

### SSH Configs

1. Personal Account SSH

```zsh
> ssh-keygen -t ed25519 -C "sjschultz11@gmail.com" -f ~/.ssh/id_ed25519_personal
```

2. Work Account SSH

```zsh
> ssh-keygen -t ed25519 -C "sschultz@magicmusicapp.com" -f ~/.ssh/id_ed25519_work
```

3. SSH Config file

```zsh
> nano ~/.ssh/config
```

Paste into the SSH config file:
```
# Common settings for all hosts
Host *
  AddKeysToAgent yes
  UseKeychain yes

# Personal - GitHub
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519_personal

# Work - GitLab
Host gitlab.com
  HostName gitlab.com
  User git
  IdentityFile ~/.ssh/id_ed25519_work
```

### Prime the keys to the macOS Keychain:

```zsh
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_personal
ssh-add --apple-use-keychain ~/.ssh/id_ed25519_work
```

### Global Git Config for SSH Authentication & Commit Signing

In `~/.config/git/config`:
```
[user]
    name = Soren Schultz
    email = sjschultz11@gmail.com
    signingkey = /Users/sjsch/.ssh/id_ed25519_personal.pub

[gpg]
    format = ssh

[commit]
    gpgsign = true

# Conditional include for work projects
# Ensure the path ends with a slash /
[includeIf "gitdir:~/Documents/MagicMusic/"]
    path = ~/.config/git/config-work
```

In `~/.config/git/config-work`:
```
[user]
    email = sschultz@magicmusicapp.com
    signingkey = /Users/sjsch/.ssh/id_ed25519_work.pub
```

### Github/Gitlab Settings config

- Add personal SSH public key from `~/.ssh/id_ed25519_personal.pub` to github as both an Authentication key and a Signing key.

- Add work SSH public key from `~/.ssh/id_ed25519_work.pub` to gitlab as both an Authentication key and a Signing key.

### Verification
Run:
```zsh
ssh -T git@github.com
ssh -T git@gitlab.com
```

In any git repository outside `~/Documents/MagicMusic/`:
```zsh
> git config --show-origin user.email
file:/Users/sjsch/.config/git/config  sjschultz11@gmail.com
```

And in any git repository inside `~/Documents/MagicMusic/`:
```zsh
> git config --show-origin user.email
file:/Users/sjsch/.config/git/config-work  sschultz@magicmusicapp.com
```
