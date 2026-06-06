# Clone a VibeTemplate Repo and Change It to Your Own GitHub Repo

This guide explains how to clone a VibeTemplate repository into a new local folder and then point that local project to your own personal GitHub repository.

## Placeholders

Replace these values with your actual repository names:

```bash
TEMPLATE_REPO_URL="https://github.com/maludb/native-lamp-vibetemplate.git"
NEW_FOLDER="www"
GITHUB_USERNAME="your-github-username"
NEW_REPO_NAME="my-new-repo"
```

Your new GitHub repository URL will look like one of these:

HTTPS:

```bash
https://github.com/your-github-username/my-new-repo.git
```

SSH:

```bash
git@github.com:your-github-username/my-new-repo.git
```

---
### 1. Make the parent directory of apache home writeable

```bash
cd /
sudo chmod 777 var
cd /var
sudo mv www www-original
```

### 2. Clone the template repo into a new folder on the server

```bash
git clone https://github.com/maludb/native-lamp-vibetempate.git /var/www
cd /var/www
```
### 3. Remove write privileges on the parent of apache home

```bash
cd /
sudo chmod 755 /var
cd www
sudo chmod 777 www
cd /var/www
```

### 4. Create your new repo on GitHub

Create a new empty repository in your personal GitHub account.

Do **not** initialize it with a README, `.gitignore`, or license if you already have files locally.

Example new repo:

```text
https://github.com/your-github-username/my-new-repo
```

### 5. Change the remote from the template repo to your own repo

Check the current remote:

```bash
git remote -v
```

It will probably show the template repo as `origin`.

Change `origin` to your new GitHub repo.

Using HTTPS:

```bash
git remote set-url origin https://github.com/your-github-username/my-new-repo.git
```

Using SSH:

```bash
git remote set-url origin git@github.com:your-github-username/my-new-repo.git
```

Verify the change:

```bash
git remote -v
```

### 6. Push the code to your new GitHub repo

Make sure the branch is named `main`:

```bash
git branch -M main
```

Push to your repo:

```bash
git push -u origin main
```

Your local project is now connected to your own GitHub repo.

## Verify Everything Worked

Run:

```bash
git remote -v
git status
```

You should see your GitHub repo listed as `origin`, not the original template repo.
