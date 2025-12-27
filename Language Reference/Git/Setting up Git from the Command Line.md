This guide walks you through configuring Git on your NixOS system using SSH authentication.

## 1. Install Git

If you're using **NixOS system config** (`/etc/nixos/configuration.nix`):

```nix
{
  environment.systemPackages = with pkgs; [
    git
    openssh
  ];
}
```

## 2. Set Your Git Identity

Run this once (globally):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

> 💡 Use the email associated with your GitHub account.

## 3. Generate an SSH Key (if You Don't Have one)

Check if you already have one:

```bash
ls ~/.ssh/id_ed25519.pub
```

If not:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

Just press enter for each prompt to accept defaults.

## 4. Add Your SSH Key to GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

Copy the output, then:

- Go to GitHub → ⚙️ **Settings** → **SSH and GPG Keys**
- Click **New SSH Key**
- Paste your key and give it a name (like `nixos-laptop`)

## 5. Test SSH Connection

```bash
ssh -T git@github.com
```

You should see:

```
Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
```

## 6. Clone

```bash
git clone git@github.com:yourusername/your-repo.git
```

## 7. Push to GitHub!

```bash
git add .
git commit -m "Update config"
git push
```

## 🎯 Optional: Declarative Git Config with Home Manager

If you use Home Manager, add this to `home.nix`:

```nix
{
  programs.git = {
    enable = true;
    userName = "Your Name";
    userEmail = "you@example.com";
  };

  programs.ssh.enable = true;
}
```

Then run:

```bash
home-manager switch
```