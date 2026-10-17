# Configure Git and SSH on Ubuntu

Configure your Git identity and an SSH key to authenticate with Git hosting services such as GitHub or GitLab.

## 1. Configure your Git identity

Set the name and email that Git records on your commits:

```bash
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

Verify the settings:

```bash
git config --global --list
```

These settings apply to all repositories for your user account. To configure only the current repository, run the commands inside it and omit `--global`.

## 2. Install OpenSSH and create a key

Check whether the SSH client is installed:

```bash
ssh -V
```

If it is not installed, install it with:

```bash
sudo apt update
sudo apt install openssh-client
```

Check for existing SSH keys before creating a new one:

```bash
ls -la ~/.ssh
```

If you do not already have a suitable key, create an Ed25519 key:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Press Enter to use the default file location. Setting a passphrase is recommended to protect the private key.

## 3. Add the key to the SSH agent

Start the agent and add your private key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 4. Add the public key to your Git hosting account

Display the public key and copy its contents:

```bash
cat ~/.ssh/id_ed25519.pub
```

In your GitHub or GitLab account settings, open the SSH keys section, add a new key, and paste the public key. Never share or upload the private key (`~/.ssh/id_ed25519`).

## 5. Test the SSH connection

For GitHub, run:

```bash
ssh -T git@github.com
```

For GitLab, run:

```bash
ssh -T git@gitlab.com
```

The hosting service should respond with a message confirming that authentication succeeded.

## Security tips

Restrict access to your SSH directory and private key:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
```

- Use `~/.ssh/config` if you need to manage multiple keys or SSH accounts.
- Keep your private key secret. Add only the `.pub` public key to your hosting account.
