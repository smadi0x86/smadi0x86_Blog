---
description: >-
  Git is a distributed version control system that allows multiple developers to
  work on the same codebase without interfering with one another.
---

# Git

## <mark style="color:red;">Installing Git</mark>

#### <mark style="color:purple;">If not already installed:</mark>

```bash
sudo apt-get install git
```

## <mark style="color:red;">Configuring Git</mark>

#### <mark style="color:purple;">Set your username and email:</mark>

```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

## <mark style="color:red;">Setting Up GitLab</mark>

### <mark style="color:yellow;">Create a New Repository</mark>

* Login to GitLab.
* Click on the "+" icon at the top right and select "New project".

### <mark style="color:yellow;">Cloning a GitLab Repository</mark>

```bash
git clone https://gitlab.com/yourusername/yourrepository.git
```

## <mark style="color:red;">Regular Git Workflow</mark>

### <mark style="color:yellow;">Pull Latest Changes</mark>

#### <mark style="color:purple;">To ensure you have the latest version:</mark>

```bash
git pull
```

### <mark style="color:yellow;">Make Changes</mark>

Edit, add, or delete files.

### <mark style="color:yellow;">Stage Changes</mark>

```bash
git add .
```

### <mark style="color:yellow;">Commit Changes</mark>

```bash
git commit -m "Your descriptive message here"
```

### <mark style="color:yellow;">Push Changes to GitLab</mark>

```bash
git push origin main
```

## <mark style="color:red;">Integrating an Existing Folder with GitLab</mark>

#### <mark style="color:purple;">Navigate to your folder:</mark>

```bash
cd /path/to/your/folder
```

#### <mark style="color:purple;">Initialize Git:</mark>

```bash
git init
```

#### <mark style="color:purple;">Connect to your GitLab repository:</mark>

```bash
git remote add origin git@gitlab.com:yourusername/yourrepository.git
```

#### <mark style="color:purple;">Commit any existing changes:</mark>

```bash
git add .
git commit -m "Initial commit or a relevant message"
```

#### <mark style="color:purple;">Push to GitLab:</mark>

```bash
git push -u origin main
```

## <mark style="color:red;">Renaming the Default Branch</mark>

#### <mark style="color:purple;">Rename your local branch:</mark>

```bash
git branch -m oldname newname
```

#### <mark style="color:purple;">Push the renamed branch to GitLab:</mark>

```bash
git push -u origin newname
```

#### <mark style="color:purple;">Update the default branch on GitLab:</mark>

* Go to your project.
* "Settings" > "Repository".
* Set new default branch.

## <mark style="color:red;">Dealing with Authentication Issues</mark>

### <mark style="color:yellow;">SSH Authentication</mark>

#### <mark style="color:purple;">Ensure you have set up SSH keys. If not:</mark>

```bash
ssh-keygen -t rsa -b 4096 -C "youremail@example.com"
cat ~/.ssh/id_rsa.pub
```

Copy and add the key to GitLab.

### <mark style="color:yellow;">Changing Remote URL to SSH</mark>

#### <mark style="color:purple;">If facing HTTP authentication issues:</mark>

```bash
git remote set-url origin git@gitlab.com:yourusername/yourrepository.git
```

## <mark style="color:red;">Using git blame</mark>

### <mark style="color:yellow;">Basic Usage</mark>

#### <mark style="color:purple;">View the blame for a file:</mark>

```bash
git blame filename.ext
```

#### <mark style="color:purple;">Specifying a Range:</mark>

```bash
git blame filename.ext -L 10,20
```

#### <mark style="color:purple;">Ignoring Whitespaces:</mark>

```bash
git blame -w filename.ext
```

#### <mark style="color:purple;">Viewing Changes Since a Specific Commit:</mark>

```bash
git blame filename.ext COMMIT_HASH^..
```

#### <mark style="color:purple;">Displaying More Detailed Information:</mark>

```bash
git blame -l filename.ext
```
